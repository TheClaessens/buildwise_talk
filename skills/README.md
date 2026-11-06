# Skills

Starting points shared with the talk. Install them in a project with the skills npm package:

```
npx skills add TheClaessens/buildwise_talk
```

or copy a folder into `<project>/.claude/skills/` (shared with the repo) or `~/.claude/skills/` (just you).

- `orchestrated-build/` the factory from chapter 7, generalised: one orchestrator, fixed stages, gates where a human decides. Builds on Matt Pocock's skills (`/research`, `/to-tickets`, `/grill-me`, `/implement`, `/code-review`), with a fallback for each where they are not installed.
