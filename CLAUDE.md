# Agent Pipeline Template — repo guide

This repository **is** the template, not an app built from it. If you're working inside a project that was bootstrapped *from* this template, this file shouldn't be here — you'd have your own project-specific `CLAUDE.md` instead (written by the `/new-app` skill), with a "Template provenance" section pointing back to this repo.

See `README.md` for the full explanation of the pipeline and the retrospective loop.

## What to do in this repo

- **Bootstrapping a new project?** Use `/new-app <name> "<pitch>"` — see `.claude/skills/new-app/SKILL.md`.
- **Improving the team itself?** Edit the relevant `.claude/agents/*.md` file directly, or let the `retrospective` agent (invoked from a bootstrapped project's pipeline run) propose the edit as its own reviewable commit/PR here.
- **Understanding how a pipeline run actually works** (worktree mechanics, time-boxing, merge strategy, common failure modes) — read `.claude/skills/pipeline/SKILL.md`. That file is the accumulated, hard-won operating knowledge from real releases; keep it current as new lessons show up via retrospectives.

## Conventions for this repo specifically

- Agent `.md` files stay stack-agnostic and app-agnostic. If you're tempted to add something specific to one project's domain or tech stack, it belongs in that project's own `CLAUDE.md`, not here.
- Keep edits to `.claude/agents/*.md` small and targeted — a sentence or a bullet, not a rewrite — matching the same discipline the `retrospective` agent is instructed to follow.
- This repo has no application code of its own to build or test — there's nothing to run here beyond reading/editing markdown.
