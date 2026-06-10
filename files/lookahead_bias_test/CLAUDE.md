# CLAUDE.md — Lookahead-Bias Test for LLM Forecasts

This file instructs Claude Code to run the lookahead-bias test from
*"A Test of Lookahead Bias in LLM Forecasts"* on the user's own LLM
forecasting application. The user has text data X_t for entities i
(firms, countries, assets), uses an LLM to predict an outcome Y_{t+h},
and wants to know whether the LLM's in-sample forecasts are contaminated
by memorized outcomes from its training data.

Follow the five steps below in order. Do not skip the sanity checks.
Never fabricate model outputs or regression results; if a step fails,
stop and report the failure.

## What the test does

1. **Forecast signal** mu_hat: the LLM's prediction from the user's own pipeline.
2. **Recall signal**: a date-only query (entity + target date, no text) measuring
   what the model memorized. LAP = P(up) + P(down) is the lookahead propensity;
   (U-D) = P(up) - P(down) is the recalled direction.
3. **Validation regression**: realized outcome on (U-D). Predictive power on
   high-LAP observations, with a null on low-LAP observations and a passing
   placebo, is evidence consistent with memorization.
4. **Detection regression**: Y on mu_hat, LAP, and LAP x mu_hat on the
   pre-cutoff sample. The one-sided test of lookahead bias is beta_3 > 0
   on the interaction.
5. **Post-cutoff placebo**: after the model's training cutoff, LAP should
   collapse toward zero and beta_3 should be indistinguishable from zero.

Verdict: beta_3 > 0 pre-cutoff + validation pattern + passing placebo
=> the in-sample LLM forecasts are contaminated by lookahead bias.

## Before running: ask the user for

1. **Data file** (CSV or parquet) and column mapping (see format below).
2. **Model and backend**: model ID and serving details. For open-weight models,
   the HuggingFace model/tokenizer name (vLLM is the reference setup; the paper
   uses Llama-3.3-70B-Instruct). For APIs: base URL, auth environment variable
   (never hardcode keys), and whether the endpoint returns next-token
   logprobs/top_logprobs. Token-level probabilities are required; the sampling
   fallback (below) needs separate approval. If the API is external, confirm
   the user permits sending their text and entity names to that provider.
3. **Training cutoff date** of the exact frozen model version, from the model
   card. Split on realization date: pre-cutoff = target_date <= cutoff,
   post-cutoff = target_date > cutoff.
4. **Forecast horizon h** and the realization-period convention for time fixed
   effects (trading day, calendar month, fiscal quarter, ...). Construct
   `target_period` from `target_date` at that frequency.
5. **Outcome direction definition** and the recall-query wording: the outcome
   description and the reference period for "up vs down" (e.g., "the closing
   stock price compared to the previous trading day"). The outcome column must
   be signed so that larger values mean "up". If the raw outcome is heavily
   skewed toward one direction, tell the user the recall query will be
   uninformative and recast the outcome as a direction of change first.
6. **Forecast prompt**: the user's own production prompt and parser, used
   verbatim. Do not modify the production prompt to improve parse rates unless
   the user explicitly agrees the modified prompt is now the object under test.

## Input data format

One row per (entity, text-date) observation:

| column        | description                                                  |
|---------------|--------------------------------------------------------------|
| row_id        | unique row ID; create a deterministic one if absent          |
| entity_id     | stable identifier                                            |
| entity_name   | full name as the LLM would know it                           |
| ticker        | ticker or short identifier (optional but recommended)        |
| text          | the text X_t fed to the forecasting prompt                   |
| text_date     | date of the text                                             |
| target_date   | realization date/period of Y_{t+h}                           |
| outcome       | signed numeric realized Y_{t+h}; larger = "up"               |

Validate before querying: target_date > text_date on every row, outcome
numeric and non-missing, text non-empty. Drop failing rows and report the
count. Preserve `row_id` through every file, merge, and table.

The sample should contain observations on both sides of the training cutoff
(split on target_date). If everything is pre-cutoff, the placebo in Step 5 is
infeasible — warn the user and run Steps 1-4 only.

## Step 0: environment

```bash
pip install pandas pyarrow openai transformers
```

Regressions run in Stata with `reghdfe` (`ssc install reghdfe estout`),
executed in batch mode (`stata-mp -b do src/<name>.do`). Do all data
preparation in Python and only the estimation in Stata. If Stata is
unavailable, the same specifications can be estimated with pyfixest's
`feols`; say so in the report.

Create `outputs/` for intermediate parquet files and `results/` for tables.
Temperature 0 and fixed seeds everywhere. Cache all raw LLM responses as
JSONL, one line per query, keyed by
`sha256(task | model_id | row_id or entity+target_date | prompt | params)`;
never reuse a cached response whose key differs. Write files to a temp path
and rename atomically. Before launching any query batch, print the query
count and a cost estimate, and get explicit confirmation; if the dataset
exceeds ~10,000 rows, suggest a random subsample first.

## Step 1: forecast signal mu_hat

Query the model with the user's forecast prompt for every row, temperature 0.
Map responses to numeric mu_hat with the user's own label mapping. Save
`outputs/forecast.parquet` with row_id, entity_id, text_date, target_date,
mu_hat, parsed label, and a pointer into the raw JSONL.

Sanity check: report the parse rate (share of responses mapping to a valid
label). If below 95%, show 10 unparsed examples and ask the user how to
proceed; do not silently change the prompt or parser.

## Step 2: recall query, LAP, and (U-D)

Recall is one query per unique (entity_id, target_date) pair — deduplicate
before querying, then merge many-to-one back onto rows. Adapt this template
(from the paper) using the user's outcome description and reference period:

```
On {target_date}, did {outcome description} of {entity_name} ({ticker})
go up or down compared to {reference period}?
Answer based only on what you recall about {entity_name} ({ticker}) on that
specific date. If you do not recall, answer "unknown".
Respond with exactly one word and nothing else: up, down, or unknown.
```

Rules:
- The query must contain **no text, no fundamentals, no contemporaneous
  context** — only entity identifier and target date.
- The recall date is the **realization** date of Y_{t+h}, not the text date.
- Temperature 0, max_tokens 1, request top-20 logprobs at the answer position.
- **Check label tokenization first** on the exact tokenizer
  (`transformers.AutoTokenizer` for open-weight models). Do not assume the
  labels are single tokens: verify. If a label is multi-token, pick a
  synonymous single-token label; if none exists, stop and ask the user.
- Compute P(up), P(down), P(unknown) by summing exp(logprob) over the
  returned tokens whose decoded string — stripped of whitespace and
  lowercased — equals the label. Deduplicate by token ID. Do not renormalize:
  LAP is raw probability mass.
- If a label never appears in the top-20 logprobs its mass is censored, not
  zero; report how often this occurs and treat high censoring as a warning.
- Fallback when logprobs are unavailable: >= 25 samples per query at
  temperature 1, answer frequencies in place of probabilities. This
  multiplies the query count ~25x — print the new count and get separate
  confirmation, and label all downstream results as approximated.

Save `outputs/recall.parquet` with entity_id, target_date, p_up, p_down,
p_unknown, lap = p_up + p_down, ud = p_up - p_down.

Sanity checks (report all):
- Distribution of LAP pre- vs post-cutoff (mean, quartiles, histogram counts).
  Expect LAP visibly lower post-cutoff. If post-cutoff LAP stays high, the
  model is guessing rather than abstaining — flag this prominently.
- Residual probability mass: 1 - (p_up + p_down + p_unknown). If large,
  distinguish an unconstrained prompt from top-20 censoring before revising.

## Step 3: validation regression

Merge forecast, recall, and outcomes into `outputs/panel.parquet` on row_id
(forecast) and (entity_id, target_date) (recall); fail if the merge changes
the row count. Do the remaining preprocessing in Python, not Stata: encode
entity and period as integer group IDs (`entity_num`, `period_num`), compute
the LAP median and a `high_lap` indicator (default: pooled median of LAP; use
the median of entity-level mean LAP if within-entity LAP is noisy — report
which), and export `outputs/panel_pre.dta` and `outputs/panel_post.dta`.
Then estimate with entity and realization-period fixed effects, clustering by
entity:

```stata
use outputs/panel_pre, clear
* pooled
reghdfe outcome ud, absorb(entity_num period_num) cluster(entity_num)
* high- and low-LAP halves
reghdfe outcome ud if high_lap == 1, absorb(entity_num period_num) cluster(entity_num)
reghdfe outcome ud if high_lap == 0, absorb(entity_num period_num) cluster(entity_num)
```

Expected pattern under memorization: theta positive and significant on the
high-LAP half, near zero on the low-LAP half. Report all three estimates with
t-statistics. If the high-LAP theta is null, say so plainly — there is no
evidence of memorization and the detection test in Step 4 has little power.

## Step 4: detection regression

On the **pre-cutoff** sample:

```stata
use outputs/panel_pre, clear
reghdfe outcome c.mu_hat##c.lap, absorb(entity_num period_num) cluster(entity_num)
```

The coefficient on `c.mu_hat#c.lap` is beta_3. The test is one-sided for
beta_3 > 0: if the t-statistic is positive, p_one_sided = p_two_sided / 2; if
it is nonpositive, p_one_sided = 1 - p_two_sided / 2. Record singletons and
any omitted (collinear) coefficients reported by reghdfe and include them in
the report; export every coefficient table with esttab.

## Step 5: post-cutoff placebo

On the post-cutoff sample alone, re-estimate the Step 3 and Step 4
regressions. Expected under the memorization mechanism: LAP collapsed toward
zero (already checked in Step 2) and beta_3 indistinguishable from zero. The
placebo passes when the one-sided p-value for beta_3 > 0 exceeds 0.10. A
surviving beta_3 > 0 post-cutoff indicates the recall measure is proxying for
something other than memorization — report this as a failed placebo.

## Final report

Write `results/REPORT.md`, and save every regression's coefficient table to
CSV in `results/` (the report must quote these files, not memory):

1. Sample description: N pre/post cutoff, entities, period, rows dropped in
   validation and by the estimator.
2. LAP distribution table, pre vs post cutoff.
3. Validation table: pooled / high-LAP / low-LAP theta with t-stats.
4. Detection table: beta_1, beta_2, beta_3 with t-stats on the pre-cutoff
   sample, one-sided p-value for beta_3 > 0.
5. Placebo table: post-cutoff beta_3.
6. A one-paragraph verdict using exactly this decision rule (all on the
   pre-cutoff detection coefficient):
   - Contamination detected: beta_3 > 0 (one-sided p < 0.05) with the
     validation pattern present and the placebo passing.
   - No evidence of contamination: beta_3 indistinguishable from zero with
     LAP showing meaningful variation (otherwise report "underpowered").
   - Mixed/invalid: validation fails or placebo fails — explain which.

If contamination is detected, recommend restricting backtests to data after
the model's training cutoff. Do not recommend prompt-level fixes (masking,
"ignore future information") as a remedy: their effectiveness must be
re-verified by rerunning this test.

## Notes for Claude

- Run LLM queries concurrently but respect the backend's rate limits; default
  to 8 concurrent requests for APIs, batch inference for vLLM.
- All randomness fixed: temperature 0, fixed seed for any sampling fallback.
- Derived parquet files must be reproducible from the raw JSONL by a single
  deterministic processing script; keep that script in the repo.
- If the user's panel is a single time series (one entity), drop entity fixed
  effects, use Newey--West standard errors (Stata `newey`), and warn that
  power is limited. With few entities (< ~20 clusters), warn that
  cluster-robust inference is unreliable.
