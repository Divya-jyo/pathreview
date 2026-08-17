## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/156

**Issue title:** README scorer test fixture is too short for its own word-count assertion

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The test `test_readme_with_all_quality_signals` in `tests/unit/test_readme_scorer.py` asserts that a README fixture should score `word_count > 100` and be categorized as `"comprehensive"`, but the fixture README used in the test only contains ~51 words. This causes the test to fail — not because the README scorer logic is wrong, but because the test data doesn't actually satisfy the condition it's asserting. A successful fix extends the fixture's README content (or corrects the assertion, if a lower threshold was intended) so the test validates real scorer behavior instead of failing on insufficient test data.

**Branch name:** fix/156-readme-scorer-fixture-length

**Setup confirmation:** [ ] App runs locally at localhost:5173 (backend setup complete — Docker services running, Python deps installed, DB migrated and seeded successfully; frontend `npm install` step still pending, as Node.js/npm is not yet installed on this machine)

**Cohort ledger:** [x] Issue added to cohort ledger

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/Divya-jyo/pathreview/commit/526e31f

**Reproduction summary:**
Ran `pytest tests/unit/test_readme_scorer.py -q` in the activated venv and confirmed the failure described in the issue: `test_readme_with_all_quality_signals` fails with `assert 51 > 100`. The test's captured output shows the fixture README scores `word_count=51`, `category=minimal`, `score=0.87`. The scorer is working correctly — it's the test's fixture data that doesn't meet the word-count threshold it asserts.

**PLAN.md link:** https://github.com/Divya-jyo/pathreview/blob/fix/156-readme-scorer-fixture-length/PLAN.md

**Walkthrough video (recommended):** Skipped — not part of the grade.

**Blockers or open questions:**
Frontend `npm install` still pending (Node.js not yet installed) — not required for this backend-only fix, but noting for full local app verification later.
## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
Implemented the fix for issue #156 (commit `8cc040a`) — extended the fixture README in `test_readme_with_all_quality_signals` from ~51 words to 500+ words. Discovered the actual "comprehensive" threshold is 500 words (not 100) by reading `agent/tools/readme_scorer.py`. All 23 tests in `test_readme_scorer.py` pass.

Ran the full suite (`pytest -q`): 52 pre-existing failures unrelated to this issue (bias detector, PII scrubber, resume parser, tech detector, review service, etc.) — none involve `test_readme_scorer.py`. My change introduces no new failures.

Ran `make check`: `ruff` and `black` pass on my changed file. `mypy` reports 24 pre-existing "missing type annotation" errors across every test function in `test_readme_scorer.py` (not just mine) — this predates my change and affects the whole file uniformly. Committed with `--no-verify` to bypass this pre-existing, unrelated failure; documenting it here per course guidance.

**Next steps:**
Open a draft PR for review, request peer/mentor feedback in Slack.

**Blockers:**
None currently. Frontend `npm install` still pending (Node.js not installed), not required for this fix.
### Check-in 2 (end of week)

**PR link:** https://github.com/Divya-jyo/pathreview/pull/1

**Branch:** fix/156-readme-scorer-fixture-length

**What you built:**
Extended the fixture README in `test_readme_with_all_quality_signals` from ~51 words to 500+ words so it correctly clears the "comprehensive" word-count threshold (500 words) defined in `agent/tools/readme_scorer.py`. No production code was changed — the scorer logic was already correct; only the test's sample data was insufficient.

**Tests added or updated:**
Updated the fixture in `tests/unit/test_readme_scorer.py` (no new test functions — existing test now passes with corrected sample data). All 23 tests in this file pass.

**Self-review confirmation:** [x] make check passes (with documented pre-existing mypy gap, unrelated to this change) [x] make test-unit passes (with documented pre-existing failures in other files, unrelated to this change)

**Draft PR feedback received from:** none — posted in Slack,no response received by deadline

## Week 10 — Iteration & reflection

### Reviewer feedback

**Feedback received:** [ ] Yes  [x] No — still awaiting review

**Summary of feedback:**
No feedback was received. I posted my draft PR in the course Slack channel requesting review, but no reviewer or peer responded by the submission deadline. Per Su26 course notes, reviewer feedback was not an active feature this term.

**How you responded:**
N/A — no feedback was received to respond to.

---

### Reflection

**What was harder than you expected?**
Environment setup took far longer than I expected, and it happened before I even touched the actual issue. Docker Desktop repeatedly failed mid-download with connection drops across two different networks (home Wi-Fi and a phone hotspot), which took real troubleshooting to isolate — it turned out to be a Docker Desktop container-store setting, not a network problem, even though the symptoms looked exactly like one. Separately, `make` and `npm` weren't installed on my Windows machine at all, so I had to install Chocolatey from scratch just to get `make` working. None of this was related to my actual issue, but it consumed most of my available time in Week 7.

**What did you learn about working in a large codebase?**
I learned that a fix that looks simple from the issue title can still require real investigation. My issue's assertion (`word_count > 100`) implied a threshold of 100 words, but when I extended my fixture to just over 100 words, the test still failed — because the actual "comprehensive" category threshold in the scoring logic was 500 words, not 100. I only found this by searching the actual source file (`readme_scorer.py`) instead of trusting the test's naming. I also learned that a codebase can have many pre-existing failures (52 failing tests, 24 missing type annotations) that are completely unrelated to your change, and that documenting this honestly — rather than trying to fix everything or hide it — is the expected, professional approach.

**How did AI tools help — and where did they fall short?**
AI helped most with troubleshooting unfamiliar tooling errors (Docker registry failures, PowerShell vs. Git Bash syntax differences, pre-commit hook behavior) and with drafting structured documentation like PLAN.md and PR descriptions. It fell short when I pasted multi-line code blocks manually, since indentation broke silently and caused confusing Python errors (a function's code was accidentally read as class-level code) — I had to carefully compare line-by-line with the actual file content to catch it, since the error message didn't directly point to "indentation is wrong."

**What would you do differently if you started over?**
I would install Node.js, `make`, and confirm Docker was fully working *before* selecting an issue, rather than discovering these gaps mid-task. I'd also check an issue's GitHub comment thread for existing claim activity before committing to it, since my first two candidate issues (#156 and #89) turned out to already have dozens of claim comments and linked PRs — though I later learned claims aren't exclusive in this course, so this mattered less than I initially thought.

**What are you most proud of from this module?**
Getting the environment fully working despite three separate blocking issues (Docker networking, missing `make`, missing `npm`) without giving up or skipping the actual assignment. I'm also proud that I caught the real 500-word threshold by reading the actual source code, rather than assuming the number in the test's own assertion was the true threshold.