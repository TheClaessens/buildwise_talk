# Review standards (starter set)

Replace or extend these with the project's own conventions. Reviewers get this file plus AGENTS.md or CLAUDE.md. Every finding carries a severity: **blocker** (must fix before release), **should** (fix in this run if cheap), **nit** (note it, move on).

## Axis 1: does it do what the ticket says
- Every acceptance line in the ticket is met, and the reviewer can say how they checked.
- Nothing outside the ticket's scope changed without a reason in the handback.
- Decisions from the grill are respected. A builder who disagreed should have said so, not silently done otherwise.

## Axis 2: will the next person understand it
- Files stay within their ownership; no new cross-cutting module appeared without a contract change.
- Names say what things are. No `utils2`, no `temp`, no `final`.
- Functions do one thing and fit on a screen. Nesting deeper than three levels is a should.
- Errors are handled where they can be acted on, and not swallowed.
- No duplicated logic that already exists in the codebase (reviewers grep before approving).
- Tests exist at the seams named in the contract, and they run green with the project's test command.
- No secrets, no hard-coded environment values, no commented-out code left behind.
- Dependencies added only with a line in the handback saying why.

## What reviewers return
A list of findings: file, line, severity, one sentence, and the fix if obvious. No prose summary of the diff; the orchestrator can read.
