## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/156

**Issue title:** README scorer test fixture is too short for its own word-count assertion

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The test `test_readme_with_all_quality_signals` in `tests/unit/test_readme_scorer.py` asserts that a README fixture should score `word_count > 100` and be categorized as `"comprehensive"`, but the fixture README used in the test only contains ~51 words. This causes the test to fail — not because the README scorer logic is wrong, but because the test data doesn't actually satisfy the condition it's asserting. A successful fix extends the fixture's README content (or corrects the assertion, if a lower threshold was intended) so the test validates real scorer behavior instead of failing on insufficient test data.

**Branch name:** fix/156-readme-scorer-fixture-length

**Setup confirmation:** [ ] App runs locally at localhost:5173 (backend setup complete — Docker services running, Python deps installed, DB migrated and seeded successfully; frontend `npm install` step still pending, as Node.js/npm is not yet installed on this machine)

**Cohort ledger:** [x] Issue added to cohort ledger