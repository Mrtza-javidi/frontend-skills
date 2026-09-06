# frontend-skills

Reusable frontend engineering conventions for AI coding agents, packaged as portable
[Agent Skills](https://code.claude.com/docs/en/skills) — installable into any project
and usable by any compatible coding agent (Claude Code today; Codex, Cursor, etc. as
their tooling adopts the same standard), instead of copy-pasted docs per project.

Agent-agnostic by design: this repo holds only portable skill definitions — no
`.claude/`, no agent-specific config, no symlinks or submodules.

## Structure

```text
frontend-skills/
│
├── skills/
│   └── develop/
│       ├── SKILL.md            # what it covers, workflow, reference index
│       └── references/         # detailed, topic-specific rules
│           ├── GENERAL.md, REPOSITORY.md, SERVICE.md, STORE.md, COMPONENT.md,
│           ├── VIEW.md, COMPOSABLE.md, ENUM.md, CONSTANT.md, CONFIG.md,
│           └── UTIL.md, STYLE.md, LOCALE.md, COMMIT.md
│
├── README.md
└── LICENSE
```

## Skills

- **`develop`** — frontend development conventions (architecture layering, JS style,
  naming, CSS/BEM, i18n, commit messages). See `skills/develop/SKILL.md` for what it
  covers and how an agent should apply it.

## Installing into a project

Intended usage, once a skills installer is in place. Install one specific skill:

```bash
npx skills add https://github.com/eghamat24/frontend-skills --skill develop
```

Or install every skill this repo has (reasonable here since everything in it is
frontend-scoped):

```bash
npx skills add https://github.com/eghamat24/frontend-skills
```

(either form also works against your own fork, e.g.
`https://github.com/Mrtza-javidi/frontend-skills`.)

Until you have a skills CLI wired up, install manually: copy or `git subtree`/clone the
whole `skills/` folder (or just `skills/develop/` for a single skill) into wherever your
agent looks for skills — for Claude Code, that's `.claude/skills/` inside the target
project, one subfolder per skill.

## Adding another skill

This repo is scoped to frontend conventions, so any future addition here should also be
frontend-related. Same shape as `develop`:

```text
skills/<name>/
├── SKILL.md
└── references/
```

`SKILL.md` holds the frontmatter (`name`, discovery `description`) and workflow;
`references/` holds the detailed rules, loaded on demand. Don't duplicate a rule
between the two.

`COMMIT.md` currently lives under `skills/develop/references/` even though it isn't
frontend-specific, since `develop` is the only skill so far — fine as-is unless a
second skill here would otherwise need to duplicate it.
