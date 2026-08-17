## Solution plan

**Issue:** README scorer test fixture is too short for its own word-count assertion — https://github.com/ascherj/pathreview/issues/156

### Understand
The test `test_readme_with_all_quality_signals` in `tests/unit/test_readme_scorer.py` asserts that a "high quality" README fixture should score `word_count > 100` and be categorized as comprehensive. The fixture README only contains ~51 words. Running the test confirms this: `assert 51 > 100` fails, with actual output `category=minimal, score=0.87, word_count=51`. The scorer itself behaves correctly — it accurately identifies a 51-word document as minimal. The root cause is the test's own fixture data doesn't meet the threshold the test expects it to meet.

### Map
- `tests/unit/test_readme_scorer.py` — contains the fixture README string and the failing assertions inside `test_readme_with_all_quality_signals`
- `agent/tools/readme_scorer.py` — the `ReadmeScorer` class being tested; expected to remain unchanged, but I'll review it to confirm the exact word-count/category thresholds before editing the fixture

### Plan
1. Read `ReadmeScorer`'s scoring logic to find the exact word-count threshold that separates "minimal" from "comprehensive"
2. Extend the fixture README's existing sections (Installation, Usage, Features, Tech Stack) with more descriptive sentences instead of placeholder text, until word count exceeds 100
3. Re-run `pytest tests/unit/test_readme_scorer.py -q` to confirm the test passes
4. Confirm no other tests in the file depend on the old fixture's exact word count or content
5. Run the full test suite (`pytest -q`) to confirm no regressions elsewhere

### Inputs & outputs
- **Input:** the fixture README string inside the test function
- **Output:** an extended fixture README (100+ words) that causes the test to pass, with `category` evaluating to "comprehensive"

### Risks & unknowns
- Unsure whether "comprehensive" depends only on word count or also on presence of specific sections — need to check `readme_scorer.py` before assuming word count alone is sufficient
- Risk of changing the fixture in a way that breaks other assertions in the same test (`has_readme`, `result.success`)
- Need to confirm 100 is the correct intended threshold, not itself a separate bug

### Edge cases
- Fixture content should stay realistic, not just padded with filler words
- Extended fixture shouldn't accidentally trip other quality-signal checks (markdown formatting, code blocks, badges)