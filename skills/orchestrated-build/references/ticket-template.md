# Ticket template

One file per ticket in `docs/build/02-tickets/NN-<slug>.md`. A ticket is small enough to build and verify in one session, and functional: a user could notice it.

```markdown
# NN. <What the user can do after this ticket>

## Why
One or two sentences linking this to the spec.

## Scope
- What is in.
- What is explicitly out.

## Likely files
- src/... (owned by this ticket)

## Acceptance
- [ ] Observable check 1 (how to verify it, by hand or by test)
- [ ] Observable check 2

## Decisions
Filled during the grill. Each answer the user gave, or `(assumed)` in automatic mode.
```

Rules of thumb: if a ticket needs more than one builder, split it. If two tickets need the same file, merge them or move the shared part into the contract.
