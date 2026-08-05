---
layout: archive
title: ""
permalink: /research/
author_profile: true
---

{% include base_path %}

<style>
/* ── Section headers ── */
.research-section-title {
  font-size: 1.5rem;
  font-weight: 700;
  letter-spacing: 0.5px;
  color: #2c3e50;
  margin-top: 2.2rem;
  margin-bottom: 0.3rem;
  padding-bottom: 0.45rem;
  border-bottom: 2.5px solid #3498db;
  display: inline-block;
}

/* ── Paper card ── */
.paper-card {
  background: #fafbfc;
  border: 1px solid #e8ecf1;
  border-left: 4px solid #3498db;
  border-radius: 6px;
  padding: 1.25rem 1.4rem;
  margin-bottom: 1.6rem;
  transition: box-shadow 0.2s ease, border-left-color 0.2s ease;
}
.paper-card:hover {
  box-shadow: 0 3px 12px rgba(52,152,219,0.12);
  border-left-color: #2980b9;
}

/* ── Paper title ── */
.paper-title {
  font-size: 1.1rem;
  font-weight: 700;
  color: #1a1a2e;
  margin: 0 0 0.55rem 0;
  line-height: 1.4;
}
.paper-title a {
  color: #1a1a2e;
  text-decoration: none;
  border-bottom: 1.5px solid transparent;
  transition: border-color 0.2s;
}
.paper-title a:hover {
  border-bottom-color: #3498db;
}

/* ── "Chef's recommendation" tag ── */
.paper-recommend {
  display: inline-block;
  font-size: 0.72rem;
  font-weight: 600;
  background: #fff5e1;
  color: #b8651b;
  border: 1px solid #f1c889;
  border-radius: 999px;
  padding: 0.1rem 0.6rem;
  margin-left: 0.4rem;
  vertical-align: 2px;
  white-space: nowrap;
}

/* ── Link badges ── */
.paper-links {
  margin-bottom: 0.6rem;
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
}
.paper-links a {
  display: inline-block;
  font-size: 0.78rem;
  font-weight: 600;
  padding: 0.2rem 0.65rem;
  border-radius: 4px;
  text-decoration: none;
  transition: filter 0.15s;
}
.paper-links a:hover {
  filter: brightness(0.9);
}
.badge-arxiv   { background: #b31b1b; color: #fff; }
.badge-ssrn    { background: #1a5276; color: #fff; }
.badge-pdf     { background: #e67e22; color: #fff; }
.badge-slides  { background: #8e44ad; color: #fff; }
.badge-poster  { background: #27ae60; color: #fff; }
.badge-video   { background: #e74c3c; color: #fff; }
.badge-demo    { background: #2ecc71; color: #fff; }
.badge-draft   { background: #95a5a6; color: #fff; }
.badge-website { background: #8e44ad; color: #fff; }

/* ── Metadata rows ── */
.paper-meta {
  font-size: 0.88rem;
  color: #555;
  margin-bottom: 0.25rem;
  line-height: 1.55;
}
.paper-meta strong {
  color: #333;
}

/* ── Keywords ── */
.paper-keywords {
  margin: 0.4rem 0 0.5rem;
  display: flex;
  flex-wrap: wrap;
  gap: 0.35rem;
}
.paper-keywords span {
  font-size: 0.73rem;
  background: #eaf2f8;
  color: #2c3e50;
  padding: 0.15rem 0.55rem;
  border-radius: 3px;
}

/* ── Abstract toggle ── */
.abstract-toggle {
  font-size: 0.82rem;
  font-weight: 600;
  color: #3498db;
  cursor: pointer;
  user-select: none;
  display: inline-block;
  margin-top: 0.35rem;
}
.abstract-toggle:hover {
  color: #2980b9;
}
.abstract-content {
  font-size: 0.88rem;
  color: #444;
  line-height: 1.65;
  margin-top: 0.5rem;
  padding: 0.8rem 1rem;
  background: #fff;
  border-radius: 4px;
  border: 1px solid #e8ecf1;
}
</style>

<script>
function toggleAbstract(id) {
  var el = document.getElementById(id);
  var toggle = el.previousElementSibling;
  if (el.style.display === "none" || el.style.display === "") {
    el.style.display = "block";
    toggle.innerHTML = "▾ Abstract";
  } else {
    el.style.display = "none";
    toggle.innerHTML = "▸ Abstract";
  }
}
</script>


<!-- ============================================================ -->
<div class="research-section-title">Finance</div>
<!-- ============================================================ -->

<div class="paper-card">
  <div class="paper-title">
    <a href="https://arxiv.org/abs/2604.02921">Debiasing LLMs by Fine-tuning</a>
  </div>
  <div class="paper-links">
    <a class="badge-arxiv" href="https://arxiv.org/abs/2604.02921">arXiv</a>
    <a class="badge-website" href="https://debias-llm.yutongyan.xyz/">Website</a>
  </div>
  <div class="paper-meta">
    <strong>Authors</strong> <em>(alphabetical):</em> Zhenyu Gao, Wenxi Jiang, <strong>Yutong Yan</strong>
  </div>
  <div class="paper-keywords">
    <span>Large Language Models</span>
    <span>Extrapolation Bias</span>
    <span>Fine-tuning</span>
    <span>LoRA</span>
    <span>Financial Forecasting</span>
  </div>
  <div class="abstract-toggle" onclick="toggleAbstract('abs0')">▸ Abstract</div>
  <div class="abstract-content" id="abs0" style="display:none;">
    Large language models systematically over-extrapolate recent trends when asked to forecast, and prompting cannot fix it. We propose a supervised fine-tuning approach using Low-Rank Adaptation (LoRA), training models on instruction datasets built from rational benchmark forecasts to intervene at the parameter level in how LLMs translate observed information into predictions. In controlled forecasting experiments and stock return prediction tasks, a single round of low-cost fine-tuning aligns AI forecasts with rational benchmarks out-of-sample, correcting extrapolative bias while maintaining generalizability.
  </div>
</div>


<div class="paper-card">
  <div class="paper-title">
    <a href="https://arxiv.org/abs/2512.23847">Detecting Lookahead Bias in LLM Forecasts</a>
  </div>
  <div class="paper-links">
    <a class="badge-arxiv" href="https://arxiv.org/abs/2512.23847" style="display:none">arXiv (June 2026)</a>
    <a class="badge-ssrn" href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5985277">SSRN (July 2026)</a>
    <a class="badge-website" href="https://lookahead-bias.yutongyan.xyz/">Website</a>
  </div>
  <div class="paper-meta">
    <strong>Authors</strong> <em>(alphabetical):</em> Zhenyu Gao, Wenxi Jiang, <strong>Yutong Yan</strong>
  </div>
  <div class="paper-keywords">
    <span>Large Language Models</span>
    <span>Lookahead Bias</span>
    <span>Financial Forecasting</span>
    <span>Asset Pricing</span>
  </div>
  <div class="paper-meta">
    <strong>Presentations:</strong> NBER AI and Economic Measurement 2026 · ABFER 2026 · DFCI 2026 · SoFiE 2026 · CICF 2026
  </div>
  <div class="abstract-toggle" onclick="toggleAbstract('abs1')">▸ Abstract</div>
  <div class="abstract-content" id="abs1" style="display:none;">
    We develop a statistical test to detect lookahead bias in economic forecasts generated by large language models (LLMs). Leveraging state-of-the-art pretraining data detection techniques, we estimate the likelihood that a given prompt appeared in an LLM's training corpus—a statistic we term <strong>Lookahead Propensity (LAP)</strong>. We formally show that a positive correlation between LAP and forecast accuracy indicates both the presence and magnitude of lookahead bias. We apply the test to two forecasting settings: news headlines predicting stock returns and earnings call transcripts predicting capital expenditures. Our approach provides a cost-efficient diagnostic tool for assessing the validity and reliability of LLM-generated forecasts.
  </div>
</div>


<div class="paper-card">
  <div class="paper-title">
    <a href="https://arxiv.org/abs/2603.11838">DatedGPT: Preventing Lookahead Bias in Large Language Models with Time-Aware Pretraining</a>
  </div>
  <div class="paper-links">
    <a class="badge-arxiv" href="https://arxiv.org/abs/2603.11838">arXiv</a>
    <a class="badge-video" href="https://yutongyan.xyz/files/datedgpt_demo_video.mp4">Video</a>
    <a class="badge-poster" href="https://yutongyan.xyz/files/DatedGPT__Preventing_Lookahead_Bias_in_LLMs_with_Time_Aware_with_Pretraining_dec28.pdf">Poster</a>
    <a class="badge-demo" href="https://datedgpt.com">Live Demo</a>
  </div>
  <div class="paper-meta">
    <strong>Authors:</strong> <strong>Yutong Yan</strong>, Raphael Tang, Zhenyu Gao, Wenxi Jiang, Yao Lu
  </div>
  <div class="paper-keywords">
    <span>Large Language Models</span>
    <span>Lookahead Bias</span>
    <span>Time-Aware Pretraining</span>
  </div>
  <div class="paper-meta">
    <strong>Presentations:</strong> AFA Poster Session 2026
  </div>
  <div class="abstract-toggle" onclick="toggleAbstract('abs2')">▸ Abstract</div>
  <div class="abstract-content" id="abs2" style="display:none;">
    In financial backtesting, large language models pretrained on internet-scale data risk introducing lookahead bias that undermines their forecasting validity, as they may have already seen the true outcome during training. To address this, we present DatedGPT, a family of twelve 1.3B-parameter language models, each trained from scratch on approximately 100 billion tokens of temporally partitioned data with strict annual cutoffs spanning 2013 to 2024. We further enhance each model with instruction fine-tuning on both general-domain and finance-specific datasets curated to respect the same temporal boundaries. Perplexity-based probing confirms that each model's knowledge is effectively bounded by its data cutoff year, while evaluation on standard benchmarks shows competitive performance with existing models of similar scale. We provide an interactive web demo that allows users to query and compare responses from models across different cutoff years.
  </div>
</div>


<div class="paper-card">
  <div class="paper-title">
    Deciphering Green Preferences and Climate Risk Perceptions: An NLP Approach
  </div>
  <div class="paper-links">
    <a class="badge-draft" href="#">Draft Available Upon Request</a>
  </div>
  <div class="paper-meta">
    <strong>Authors</strong> <em>(alphabetical):</em> Darwin Choi, Zhenyu Gao, Wenxi Jiang, <strong>Yutong Yan</strong>, Hulai Zhang
  </div>
  <div class="paper-keywords">
    <span>Climate Finance</span>
    <span>Institutional Investors</span>
    <span>ESG</span>
    <span>Textual Analysis</span>
    <span>NLP</span>
  </div>
  <div class="abstract-toggle" onclick="toggleAbstract('abs3')">▸ Abstract</div>
  <div class="abstract-content" id="abs3" style="display:none;">
    We employ Natural Language Processing (NLP) to scrutinize regulatory filings, identifying institutional investors' climate change preferences and risk perceptions. These preferences and risk perceptions grow over time and are stronger after a fund has signed the Principles for Responsible Investment (PRI) or is located in regions with stronger global warming beliefs. Investors preferring green assets tend to decrease their portfolio weights in environmentally unfriendly stocks, reflecting a desire to align investments with their values. However, the relationship between climate risk perceptions and portfolio weights of brown stocks varies due to heterogeneous investment strategies. Investors with higher climate risk perceptions are more likely to support environmental shareholder proposals, whereas investors with green preferences are not. These findings provide new insights into sustainable investing behavior under differing investor motivations.
  </div>
</div>


<!-- ============================================================ -->
<div class="research-section-title">Machine Learning</div>
<!-- ============================================================ -->

<div class="paper-card">
  <div class="paper-title">
    Bandit Algorithms for Factorial Experiments
  </div>
  <div class="paper-links">
    <a class="badge-pdf" href="https://yutongyan.xyz/files/bandits_for_factorial_report.pdf">PDF</a>
    <a class="badge-poster" href="https://yutongyan.xyz/files/bandits_for_factorial_poster.pdf">Poster</a>
    <a class="badge-slides" href="https://yutongyan.xyz/files/bandits_for_factorial_slides.pdf">Slides</a>
  </div>
  <div class="paper-meta">
    <strong>Authors:</strong> <strong>Yutong Yan</strong>, Audrey Durand, Joelle Pineau
  </div>
  <div class="paper-keywords">
    <span>Machine Learning</span>
    <span>Optimization</span>
    <span>Multi-Armed Bandits</span>
  </div>
  <div class="paper-meta">
    <strong>Presentations:</strong> <a href="https://wimlworkshop.org/">WiML Workshop, NeurIPS 2019</a>
  </div>
  <div class="abstract-toggle" onclick="toggleAbstract('abs4')">▸ Abstract</div>
  <div class="abstract-content" id="abs4" style="display:none;">
    A multi-armed bandit algorithm is developed for factorial experiments. Using tools from advanced probability theory, I first prove that UCT algorithm with Laplace bound has a lower computational complexity than the naïve UCT algorithm. I begin by analyzing UCB1 for non-stationary bandit problems, and then prove UCT algorithm with Laplace Bounds achieves a better lower bound. Also, I demonstrate that the probability of suboptimal choices will converge to zero with a convergence of failure probability. In settings of deep learning, experimental results are also consistent with the theoretical regret bound.
  </div>
</div>


<div class="paper-card">
  <div class="paper-title">
    A Theoretical Analysis of Upper Confidence Bound Applied to Trees
  </div>
  <div class="paper-links">
    <a class="badge-pdf" href="https://yutongyan.xyz/files/uct_proof.pdf">PDF</a>
    <a class="badge-slides" href="https://yutongyan.xyz/files/uct_slides.pdf">Slides</a>
  </div>
  <div class="paper-meta">
    <strong>Authors:</strong> <strong>Yutong Yan</strong>, Audrey Durand, Joelle Pineau
  </div>
  <div class="paper-keywords">
    <span>Machine Learning</span>
    <span>Optimization</span>
    <span>Tree Search</span>
  </div>
  <div class="abstract-toggle" onclick="toggleAbstract('abs5')">▸ Abstract</div>
  <div class="abstract-content" id="abs5" style="display:none;">
    Using Factorial experiments, I explore multi-armed bandit problems in which a player selects actions (here a sequence) episodically and observes the outcomes. I consider the Upper Confidence Bound applied to Trees (UCT), a popular algorithm for tree search, in order to identify the sequence of choices that maximizes some objective function. Using synthetic experiments, I demonstrate that applying tighter concentration bounds to Linear Bandits can significantly improve the performance of UCT for tree search. Next step is to investigate various factorial experimental design configurations. I also compare the performance of algorithms under three different formulations of the factorial experiment: 1) standard bandits; 2) linear bandits; and 3) bandits for tree search. I observe that capturing the underlying tree structure is essential for robustness, whether the outcome function is linear or not. Furthermore, I observe that the algorithms employed under the bandits formulation of tree search for factorial experimental designs appear more robust to the noise variance than other approaches.
  </div>
</div>
