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