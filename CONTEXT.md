# spectreconsole/spectre.console context
> refreshed 2026-09-30 | upstream default: main @ 01027efcd6c40c5ca1b2f0eb22261336c7209318

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

## Fork / CI notes
- `2026-09-30` Fork Actions are now ENABLED (`GET /repos/olitreadwell/spectre.console/actions/permissions` -> enabled:true, allowed_actions:all). The earlier "fork Actions not connected" note (2026-09-03) is stale: after enabling, a push to a fork branch triggers `.github/workflows/ci.yaml` (job "Build": `dotnet tool restore && dotnet make`). A queued/never-run workflow can be nudged by a force-push (synchronize event).
- `2026-09-30` Local build note for this container: `dotnet make` (Cake) needs the .NET 8/9/10 runtimes because the test projects multi-target; only .NET 10 was preinstalled, so 8.0 + 9.0 runtimes were added. `dotnet make` default target = Clean, Build (warnings-as-errors), Test, Package; `Lint` (`dotnet format style --verify-no-changes`) is a separate target and NOT part of CI's default, and has pre-existing findings in untouched files (`EmojiEmitter.cs`, `SelectionPromptTests.cs`, `TextPromptTests.cs`, `Rendering/Segment.cs`, `Widgets/Figlet/FigletText.cs`).

## Gap ledger (continued)
- `2026-09-30` trivial-cleanup pass (typos + broken links) — pr-opened (fork PR https://github.com/olitreadwell/spectre.console/pull/7, branch `fix/docs-typos-dead-links`, base `main`). Packed 9 genuine, verified, meaning-preserving fixes in 9 files (diff +9/-9, no whitespace churn): `CONTRIBUTING.md` licence link `LICENSE`->`LICENSE.md` (404 verified); `README.jp.md` `./appendix/emojis`->`https://spectreconsole.net/appendix/emojis` (404 verified, no `appendix/` dir); `TypeNameHelper.cs` dead `aspnet/Common/blob/dev/...`->`dotnet/runtime/.../TypeNameHelper.cs` (both checked); `SegmentLine.cs` "Preprends"->"Prepends"; `TableRowCollection.cs` column-bound message "rows"->"columns" (+ updated test expectation in `TableRowCollectionTests.cs`); 3x `resources/scripts/Generate-*.ps1` "occured"->"occurred". Search: codespell 2.4.3 whole-repo (only these + data/translation false positives), manual misspelling grep, external-URL liveness (docs.microsoft.com->learn redirect is 200, not a fix), plus the whole `README.*` link set. No open/closed/merged upstream PR or issue covers these (deduped via `gh search`); precedent #1998 merged a README typo. Upstream issue-first gate is unresolved (repo template asks for an issue; no upstream issue exists) and is recorded as a promotion prerequisite in the fork PR body.
- `2026-09-25`/`2026-09-26`/`2026-09-27` engine runs on this repo — error (no artefact). Ignore; nothing produced.
