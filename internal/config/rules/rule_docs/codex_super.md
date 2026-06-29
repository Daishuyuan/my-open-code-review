# Codex Super Review Profile

This profile imports the practical parts of the Codex `code-review` skill into
OpenCodeReview's per-file engine. It is mandatory when selected, but it does not
replace project, global, or language-specific rules.

## Findings Contract

- Report findings first. Do not spend comments on summaries, praise, or correct
  code.
- Each finding must have concrete evidence, a meaningful consequence, and a
  specific location in newly added or modified code.
- Drop speculative, duplicate, pre-existing, style-only, formatting-only, and
  linter-only findings unless the user explicitly asked for exhaustive cleanup.
- Use unchanged files only to verify changed-code behavior. Do not comment on
  unrelated issues discovered while gathering context.
- If a candidate issue cannot be proven with the diff and minimal surrounding
  context, do not report it.

## Severity Rubric

- Critical: likely security breach, data loss, corruption, crash, or silent
  integrity failure.
- High: likely user-facing breakage, API or compatibility contract break, or
  serious operational risk in normal use.
- Medium: meaningful edge-case failure, missing regression coverage for changed
  behavior, or maintainability issue that will materially slow safe iteration.
- Low: optional improvement. Omit Low findings unless the review request asks
  for exhaustive feedback.

## Risk Lenses

Use these lenses as a checklist. Trigger a lens only when the changed file or
diff content makes it relevant.

- Bug lens: trace data flow, state changes, concurrency, cleanup, transactions,
  fallback behavior, silent failures, nil or empty values, and boundary
  conditions.
- Security lens: inspect authentication, authorization, secret handling, user
  input, shell and file access, path handling, deserialization, redirects,
  network calls, logging, and error disclosure. Security failures must fail
  closed.
- Contract lens: verify API, schema, DTO, event, migration, config, CLI flag,
  and serialization compatibility. Look for callers or persisted data that the
  changed contract may break.
- Test lens: check whether changed behavior has focused regression coverage.
  Report missing tests only when the untested path is important enough to hide a
  real regression.
- Quality lens: report maintainability only when complexity, duplication, or
  unclear boundaries create a concrete future bug risk.
- History lens: consider git history only for hotspots, reversions, unusual
  legacy patterns, or when history changes the confidence of a finding.

## Tool Use

- Start from changed hunks, then read the smallest surrounding code needed to
  verify or reject a candidate.
- Use search tools to find callers, interfaces, migrations, tests, and
  validations only when they materially affect a candidate finding.
- Prefer one verified issue over several plausible but unproven concerns.
