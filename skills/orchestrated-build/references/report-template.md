# Build report: <project or ticket set>

Date, mode (manual / automatic), orchestrator model, who approved what.

## What was asked
Two or three sentences. Link to `02-spec.md`.

## What was built
Per ticket: one line, done / partial / dropped, and why if not done.

## What was verified, and how
| Check | How it was run | Result |
|---|---|---|
| Tests | `pnpm test` | 48 passed |
| Manual | opened /dashboard, clicked through the flow | ok |
| QA personas | 3 subagents, questions X Y Z | see below |

## What was not run
Be explicit. "Not run" is a result.

## Assumptions (automatic mode)
Every answer the orchestrator gave itself during the grill, marked `(assumed)`.

## Review findings
| Finding | Severity | Outcome |
|---|---|---|

## QA
What the personas said, which claims were verified, which were not.

## Next
Three things at most. What the user should decide, and what the next run should pick up.
