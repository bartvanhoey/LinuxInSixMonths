# Project Health Report
**Report:** Health Audit

**Project:** LinuxInSixMonths | **Stack:** Markdown documentation (no code, no manifests) | **Date:** 2026-11-18 | **Assessed by:** Claude

**Scope:** Docs-only repository (`README.md` + 26 week files, 690 lines). No build system, so every dimension used the Fallback column in `references/tools-by-stack.md` and carries a `?`. Build Health = `markdownlint-cli2` + link checks. Test Coverage = presence of automated checks.

### Grades

| Dimension | Grade | Score | Key Finding |
|-----------|-------|-------|-------------|
| Build Health | F? | 55 | 0 broken links, but 161 markdownlint findings (151 are MD013 line-length) |
| Code Quality | D? | 62 | 2 content issues in 0.69K lines (2.9/1K): unrendered `<pid>`, heading ending in `...` |
| Architecture | A? | 95 | 26/26 week files linked from README; prev/next/overview nav and 7 anchors all resolve |
| Test Coverage | F? | 0 | No automated lint or link check (no CI, no tooling config beyond `.markdownlint.json`) |
| Dead Code | A? | 100 | 0 orphaned files, 0 TODO/FIXME/TBD markers |
| API Surface | A~? | 92 | All 26 week files follow the same template (Done, Goal, Exercises, Deliverable) |
| Security | A? | 96 | No dependencies, no hardcoded secrets (4 grep hits are prose about secrets) |
| Documentation | B? | 88 | Comprehensive README; no LICENSE; week files are terse (22 lines each) |

### Overall GPA: 2.5 — Fair

> Fair — functional but accumulating tech debt. The content is well organised; the gap is tooling, not substance.

### Priority Recommendations

1. **Build Health (F → C):** Resolve the lint noise.
   - 151 of 161 findings are MD013 (80-column limit) on prose lines. Either set `"MD013": false` in `.markdownlint.json` (it already disables MD036 and MD041), or accept and wrap. Disabling leaves 10 findings.
   - `README.md:20` — MD060 table pipe spacing (8 findings, one header row).
   - `README.md:92` — MD026 trailing `...` in heading.
   - `weeks/week-06-processes-and-signals.md:13` — MD033, `<pid>` parsed as HTML.
   Estimated effort: 15 minutes

2. **Code Quality (D → A):** Fix the two real content issues.
   - `weeks/week-06-processes-and-signals.md:13` — write `` `/proc/<pid>` `` in code formatting; as written `<pid>` disappears when rendered, so the reader sees `/proc/`.
   - `README.md:92` — rename the heading to drop the trailing `...` (this also fixes the `#` anchor if you ever link to it).
   Estimated effort: 5 minutes

3. **Test Coverage (F → C):** Add automated checks.
   - Run `markdownlint-cli2` and a link checker (e.g. `lychee`) in a pipeline or pre-commit hook. This is a nice small exercise for Weeks 21-22 too.
   Estimated effort: 1 hour

4. **Documentation (B → A):** Add a `LICENSE` file (or state the repo is private). Optionally add 2-3 reference links or example commands per week file.
   Estimated effort: 1-2 hours

### Conclusion

The curriculum is structurally sound: every week file is linked, uniformly templated, and navigation is fully consistent. The low grades come from missing tooling: lint is run with a strict default (80 columns) that the prose does not follow, and nothing checks the repo automatically. Settling the MD013 question and fixing the `<pid>` rendering bug is the quickest win.
