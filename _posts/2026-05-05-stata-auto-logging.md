---
title: "Never Lose a Stata Session Again: Auto-Logging via profile.do"
date: 2026-05-05
permalink: /posts/2026/05/05/stata-auto-logging/
tags:
  - stata
  - workflow
  - reproducibility
---

<style>
.code-wrapper {
  position: relative;
}
.copy-btn {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  background: transparent;
  color: #9ca3af;
  border: none;
  border-radius: 4px;
  padding: 0.3rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.35rem;
  font-size: 0.75rem;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  transition: color 0.15s;
  z-index: 1;
}
.copy-btn:hover { color: #fff; }
.copy-btn svg {
  width: 16px;
  height: 16px;
  fill: none;
  stroke: currentColor;
  stroke-width: 2;
  stroke-linecap: round;
  stroke-linejoin: round;
}
.copy-btn .copy-label { color: currentColor; }
</style>

<script>
document.addEventListener("DOMContentLoaded", function() {
  document.querySelectorAll("pre").forEach(function(pre) {
    var wrapper = document.createElement("div");
    wrapper.className = "code-wrapper";
    pre.parentNode.insertBefore(wrapper, pre);
    wrapper.appendChild(pre);

    var btn = document.createElement("button");
    btn.className = "copy-btn";
    btn.innerHTML = '<svg viewBox="0 0 24 24"><rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path></svg><span class="copy-label">Copy code</span>';
    wrapper.appendChild(btn);

    btn.addEventListener("click", function() {
      var code = pre.querySelector("code");
      var text = (code || pre).textContent;
      navigator.clipboard.writeText(text).then(function() {
        btn.innerHTML = '<svg viewBox="0 0 24 24"><polyline points="20 6 9 17 4 12"></polyline></svg><span class="copy-label">Copied!</span>';
        setTimeout(function() {
          btn.innerHTML = '<svg viewBox="0 0 24 24"><rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path></svg><span class="copy-label">Copy code</span>';
        }, 2000);
      });
    });
  });
});
</script>

If you've ever stared at a screen full of regression output and thought
*"I really hope I remember to save this log,"* this post is for you.
Stata's `log using` is great when you remember to type it. The trouble
is, you don't — and three weeks later you realize the only record of
that one regression you can't reproduce was scrolled off the terminal.

Here's a five-minute setup that auto-captures every Stata session,
batch or interactive, into a uniquely named file. No mental load, no
overwrites, no lost work.

## The idea

Stata reads a file called `profile.do` from your *personal* ado
directory whenever it starts. Whatever you put in `profile.do` runs
automatically at the beginning of every session. So we put a
`log using` command in there.

But two snags:

1. **Filenames must be unique.** Otherwise repeated runs clobber
   each other.
2. **Multiple sessions can start in the same second.** If you launch
   ten batch jobs in parallel, a timestamp-only filename collides.

The fix is a timestamp **plus** a six-digit random tag. The (time,
random) tuple is essentially unique even under heavy parallelism.

## The setup

First, find your personal ado directory:

```stata
sysdir
```

Look for the line beginning `PERSONAL:`. It's typically
`~/ado/personal/`. Make sure the directory exists, plus a
subdirectory for the logs:

```bash
mkdir -p ~/ado/personal/stata_logs
```

Then drop this file at `~/ado/personal/profile.do`:

```stata
* ============================================================================
* profile.do -- runs automatically at Stata startup.
* Persistent session log: every interactive or batch session writes to
* a uniquely-named file under $log_dir. Filename format:
*     YYYY_M_D_HMS_RRRRRR.log
* where RRRRRR is a 6-digit random tag drawn from /dev/urandom.
* ============================================================================

global log_dir "~/ado/personal/stata_logs"
capture mkdir "$log_dir"

* ---- Build the YYYY_M_D_HMS timestamp ----
local wjm   = subinstr(subinstr("`c(current_date)'", ":", "", .), " ", "", .)
local year  = year(date("`wjm'", "DMY"))
local month = month(date("`wjm'", "DMY"))
local day   = day(date("`wjm'", "DMY"))
local sj    = subinstr(subinstr("`c(current_time)'", ":", "", .), " ", "", .)
local stamp = "`year'_`month'_`day'_`sj'"

* ---- 6-digit random suffix from /dev/urandom ----
* Disambiguates concurrent sessions started in the same second.
tempfile rndfile
capture !od -An -N4 -tu4 /dev/urandom 2>/dev/null | tr -d ' \n' > "`rndfile'"
local rnd_str = "000000"
capture file open rh using "`rndfile'", read
if _rc == 0 {
    capture file read rh rndline
    capture file close rh
    capture local rnd = mod(real("`rndline'"), 1000000)
    if !missing(`rnd') {
        local rnd_str = string(`rnd', "%06.0f")
    }
}

* ---- Open the session log ----
log using "$log_dir/`stamp'_`rnd_str'.log", text
di "Log started: $log_dir/`stamp'_`rnd_str'.log"
```

That's it. Nothing else to remember. Every Stata session — every
`do` script, every interactive session, every batch run — quietly
deposits a complete transcript into `~/ado/personal/stata_logs/`.

## Verifying it works

Start two Stata batch processes simultaneously:

```bash
echo 'di "session A"' | stata-mp >/dev/null 2>&1 &
echo 'di "session B"' | stata-mp >/dev/null 2>&1 &
wait
ls -t ~/ado/personal/stata_logs/ | head -2
```

You should see two distinct files, both stamped with the same second
but different six-digit tails:

```
2026_5_5_074739_365771.log
2026_5_5_074739_749474.log
```

Open either: it contains the full session output, ready for grep.

## Why the random tag matters

`/dev/urandom` gives 32 bits of entropy per draw, modded down to a
six-digit integer. The collision probability between any two
sessions starting in the *same* second is roughly $1/10^6$. If you
launch a hundred jobs at once, the probability that *any* pair
collides is around $\binom{100}{2} / 10^6 \approx 0.5\%$, and the
losing job will get a "file exists" error from Stata rather than
silently overwriting. A clean failure mode.

If you wanted higher safety, you could draw eight digits instead of
six (change `1000000` to `100000000` and `%06.0f` to `%08.0f`). Six
is fine for human-scale parallelism.

## Why no `, replace`

I deliberately omitted the `replace` flag on `log using`. Filenames
are unique by construction, so `replace` would only ever fire on a
genuine collision — at which point I'd rather see Stata error out
than silently overwrite a sibling job's log. The cost of being
notified about a once-in-blue-moon collision is much lower than the
cost of losing data.

## What it gives you

- **Zero-friction reproducibility.** Every regression coefficient
  you've ever run on this machine is grep-able.
- **Audit trail under parallelism.** If you launch ten regression
  scripts at the same time, all ten produce distinct logs.
- **No naming discipline required.** You can name your `do` files
  whatever you like; the session log is named by the runtime, not
  the script.

## Caveats

- Logs accumulate. If you generate hundreds per week, gzip
  files older than 60 days from a cron — straightforward.
- `profile.do` runs at startup, so if it errors (say, your log
  directory is missing and your filesystem is read-only) Stata
  prints a warning and continues without logging. The `capture`
  guards prevent a fatal abort.
- Project-local `do` files that themselves call `log using` will
  open a *second* nested log; Stata supports up to five concurrent
  logs and treats them independently. Your auto-log keeps running
  in the background.

I've been using this for a while now and the cognitive relief is
real. Type `do my_messy_analysis.do` knowing that whatever happens
next is on disk.

---

*Drafted as a working note while setting up a fresh research machine.
The code above is plug-and-play; copy it into `~/ado/personal/profile.do`
and your next Stata session is logged automatically.*
