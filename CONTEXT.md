# spectreconsole/spectre.console context
> refreshed 2026-09-03 | upstream default: main @ a3065cf81cd9351a08690434a7f4631a1699b13d

## Identity & policies
- upstream: spectreconsole/spectre.console, default branch `main`, primary language C# (.NET), English-first (yes — issues/docs/UI in English).
- CLA/DCO: none (attestation-based licensing per CONTRIBUTING prerequisites; no CLA bot, no DCO sign-off).
- AI-assisted PR policy: PR template has a CONDITIONAL disclosure line ("If you have used generative AI... you will need to disclose this here"). Vetted policy passport (2026-08-24) classified `ai_disclosure_required: false` — disclosure only applies if AI was used; fork PRs carry no AI mention. `bans_ai: false`.
- signed commits required: no.
- PR template: `.github/pull_request_template.md` — REQUIRES an issue number ("Do NOT open a PR without discussing the changes on an open issue, first." / `Fixes #`). issue-first required; flag as promotion prerequisite.
- external tracker: github.

## Conventions (verified from merged PRs)
- branch naming: dominant human pattern `fix/...` and `feature/...` (e.g. `fix/table-measurer-infinite-loop-2131`, `feature/GH-2152`); renovate bot uses `renovate/...`.
- commit style: mixed — plain imperative ("Fix infinite loop in TableMeasurer...", "Allow wrapping of status text") and occasional Conventional Commits (`fix(generator): ...`, `docs: ...`). Plain imperative dominant for human commits.
- test command: `dotnet test` (xunit + Verify snapshot tests with `.verified.txt` expectations). Lint/analyzers via Roslynator in build.
- CI: GitHub Actions; substantive checks are build + tests + analyzers.
- how outside PRs get merged: responsive — recent external PRs merged within days (e.g. #2162, #2172, #2169). Maintainer patriksvensson active.

## Maintainer picture
- active maintainer: patriksvensson (responds to issues, e.g. #2193, #1893). Other maintainers occasionally.
- areas actively worked: progress/prompts, source generator, table rendering.

## Issue-area health
- Most open issues carry `needs triage` or `area: X` labels; no `accepted`/`confirmed`/`ready` labels observed on open issues.
- Contested/redesign signals: #2193 (TestConsole hang) — maintainer pushed back on repro (thread-safety), contested.
- Open + concrete + unclaimed bugs we could pick: #2197 (BreakdownChart all-zero), #2184 (Panel header truncation).

## Gap ledger (dedupe — READ FIRST, never re-pick)
- `2026-09-03` issue #2197 (BreakdownChart renders first item as 100% when all values are zero) — pr-opened (fork PR https://github.com/olitreadwell/spectre.console/pull/1). Root cause: `BreakdownBar.Render` divides by `maxValue=0` and calls `Ratio.Distribute` with all-zero ratios (Debug.Assert fires in debug; first item gets full width in release). Fix: guard `maxValue <= 0` in `BreakdownBar.Render` (yield break). Regression test `Should_Not_Render_Bar_When_All_Values_Are_Zero` + `AllZero.Output.verified.txt`. 753 tests pass. No open/closed/merged upstream PR referenced #2197 (deduped).
- `2026-09-03` issue #2182 (ProgressContext.IsFinished premature true) — dropped (claimed: open PR #2183 by MateusMo).
- `2026-09-03` issue #2168 (cursor hidden after cancelling list prompt) — dropped (claimed: open PR #2174 by HillkirkLautaro).
- `2026-09-03` issue #2167 (ProgressTask negative MaxValue) — dropped (claimed: open PR #2170).

## Mined gaps (discovered, not yet attempted)
- `2026-09-03` docs/clean-code #2184 Panel drops/truncates a header wider than its content even with room — `Panel.Measure` ignores header width; `AddTopBorder` renders header via Rule which ellipsizes. Repro in issue. status: proposed (not attempted this cycle).
