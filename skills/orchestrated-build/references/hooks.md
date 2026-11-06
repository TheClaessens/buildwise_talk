# The Stop hook that runs the tests

An instruction in CLAUDE.md ("always run the tests") can be skipped. A hook cannot. Two files, committed in the repository, so everyone who clones it gets the same guardrail.

`.claude/settings.json`
```json
{
  "hooks": {
    "Stop": [{
      "hooks": [{ "type": "command", "command": ".claude/hooks/run-tests.sh" }]
    }]
  }
}
```

`.claude/hooks/run-tests.sh`
```bash
#!/bin/bash
# runs every time Claude wants to finish
pnpm test 1>&2 || exit 2
# exit 2: the test output goes back to Claude, and it has to keep working
```

Replace `pnpm test` with the project's test command. Make the script executable. Docs: https://code.claude.com/docs/en/hooks

For Codex, the Buildwise AI Development Framework ships an equivalent session hook; use that where the project is Codex-first.
