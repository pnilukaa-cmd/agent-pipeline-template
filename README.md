# Agent Pipeline Template

A reusable, standalone Claude Code team: seven agent definitions and two skills that turn a rough app idea into researched, designed, specified, built, tested, and reviewed software — with each stage running in its own isolated git worktree, reviewed and merged by the orchestrating session before the next stage starts.

This was extracted from a real multi-release project (a Flutter todo app, built and shipped across several time-boxed sessions using exactly this pattern) — it's not a theoretical design, it's what actually worked, generalized so it isn't tied to that app or that stack.

## What's in here

```
.claude/
  agents/
    researcher.md          # turns feedback/open questions into cited findings
    product-manager.md      # scopes, prioritizes, locks releases, makes the calls
    ux-designer.md           # mockups, interaction design, usability audits
    business-analyst.md       # requirements + Given/When/Then acceptance criteria
    developer.md                # implements against requirements
    qa-tester.md                  # test criteria, real toolchain runs, adversarial testing
    retrospective.md               # improves this template based on what went wrong
  skills/
    pipeline/SKILL.md            # how to run the whole team: worktrees, review, merge, time-boxing
    new-app/SKILL.md               # /new-app — bootstrap a fresh project from this template
```

## The core idea

Six roles, one sequence, each isolated:

**Researcher → Product Manager → UX Designer → Business Analyst → Developer → QA Tester → (optional Senior-dev Review) → Retrospective**

Every stage that touches files runs in its own git worktree. The orchestrating session reviews the actual diff (not just the agent's summary of it), merges into the main branch, cleans up the worktree, and only then starts the next stage. Read `.claude/skills/pipeline/SKILL.md` for the full mechanics — worktree handling, merge strategy, how to handle a stalled or failed agent, time-boxing a release with a locked scope and a pre-authorized cut order, and a list of environment gotchas (disk space, emulator/build resource contention, background-command watchdogs) that cost real time if you don't expect them.

## The retrospective loop

This is the part that makes the template compound instead of staying static. After a pipeline cycle finishes, the `retrospective` agent looks at what actually caused rework this session — an ambiguous requirement, a design that didn't survive contact with implementation, a defect class that recurred more than once — and proposes a small, specific edit to the relevant agent's `.md` file. That edit lands as its own commit (or PR) **in this template repository**, not in whatever app project triggered it. Every future project bootstrapped from this template inherits the fix.

This only works because every app project records where it came from. `/new-app` writes a "Template provenance" section into the new project's root `CLAUDE.md` pointing back here — that's how `retrospective` knows where to send its findings.

## Using this template

### Start a new project

```
/new-app <project-name> "<one-line pitch>"
```

This copies the agent definitions and skills into a fresh sibling project, sets up its `CLAUDE.md` (with the template-provenance link back here), and kicks off the researcher agent with your pitch. See `.claude/skills/new-app/SKILL.md`.

### Run the team on a feature or release

Once a project exists, invoke `/pipeline` (or just describe what you want built — most sessions will recognize a feature/release-sized ask and follow this skill on their own) to run the full research → scope → design → requirements → build → test cycle. For a time-boxed release ("ship what fits by tonight"), say so explicitly — product-manager will produce a locked scope document with a real phase-by-phase budget and a pre-authorized cut order instead of an open-ended feature list.

### Contribute an improvement back

If you notice the team consistently getting something wrong that isn't yet caught by a retrospective, you can propose the same kind of small, targeted edit by hand — same rules apply as the `retrospective` agent follows: the smallest change that would have prevented the specific, evidenced issue, not a speculative rewrite.

## Design principles carried through every agent file

- **Hand off, don't hoard.** Every role has an explicit "what you don't do" section. Researcher doesn't decide, PM doesn't build, UX doesn't write production code, BA doesn't set scope, developer doesn't skip QA, QA doesn't fix bugs itself.
- **Evidence over guesswork.** Findings are labeled established-practice / competitor-pattern / speculative-idea, never presented as settled fact when they aren't.
- **A feature isn't done until it actually runs.** QA's hard requirement: a real toolchain run with pasted output, not a code read-through. This is non-negotiable across every agent file that touches verification.
- **Say what you don't know.** Every role is instructed to flag genuine ambiguity and state assumptions explicitly rather than silently guessing — and, for QA specifically, to say plainly when something is structurally impossible to verify without a real device or environment.

## Stack-agnostic by design

Nothing in `.claude/agents/` assumes a specific language, framework, or platform. `qa-tester.md` shows a placeholder toolchain shape with one concrete Flutter example — swap in whatever your project actually uses. App-specific context (stack, conventions, domain) belongs in each project's own `CLAUDE.md`, written by `/new-app` and refined as the project grows — never in the shared agent files themselves. That's the whole point of keeping this template separate: the roles and process are reusable, the app isn't.
