---
name: develop
description: Frontend development conventions for Vue-based projects — architecture layering (Repository → Service → Store → View → Component), JavaScript style, naming, file organization, CSS/BEM, i18n, and commit message rules. Use whenever writing, reviewing, or refactoring frontend code in a project that has installed this skill. Inspect the project first, read the reference file for every layer the task actually touches (not just the primary file's own layer — a component task often also touches its store, composable, service, util, or styles), and prefer these rules over conflicting patterns already present in the codebase.
---

# Develop — Frontend Development Conventions

Single entry point for the frontend coding conventions shared across every Vue-based
front-end project that installs this skill. It replaces invoking a separate skill per
topic (`/vue`, `/typescript`, `/pinia`, ...) — one `develop` skill covers the whole stack.

These rules come from real, recently-written code across several projects, not an
invented style guide. If a project's existing code disagrees with a rule here, treat
that as a signal to ask whether to refactor (see `references/GENERAL.md`'s Refactoring
Rules), not to silently copy the older pattern.

## Workflow

Follow this for every request that touches frontend code, no matter how small — a
one-line fix or a single class rename is not exempt:

1. **Inspect the project first.** Look at its actual structure and tooling (UI
   framework, state library, existing file layout), and check whether it has its own
   local override doc — commonly a `PROJECT.md` at the project root, or its
   own `CLAUDE.md`. A project's own doc always wins over this skill when the two
   genuinely conflict, since it records deliberate, project-specific deviations (a
   different UI kit, Vuex instead of Pinia, a project's actual HTTP client shape, and
   so on).
2. **Always read `references/GENERAL.md` first.** It carries the cross-cutting rules —
   architecture, JS style, naming, imports, formatting — that apply no matter which
   specific layer the task touches.
3. **Identify every layer the task actually touches — not only the layer of the
   primary file named in the request.** Implementing/refactoring/fixing a single file
   routinely pulls in other layers along the way: a component task may add a new store
   call (`STORE.md`) or composable (`COMPOSABLE.md`), which itself may reach into a
   service (`SERVICE.md`) or repository (`REPOSITORY.md`); it may need a new util
   (`UTIL.md`), enum/constant (`ENUM.md`/`CONSTANT.md`), style (`STYLE.md`), or copy
   (`LOCALE.md`). Read the reference file for each layer you end up touching, not just
   the one the request names first — see the index below. The point is not loading
   *every* file on *every* task, it's loading every file for a layer you actually write
   or change code in, however many that turns out to be.
4. **Implement following the loaded rules**, preferring them over an inconsistent
   pattern already present in the project's own code.
5. **Avoid introducing architectural inconsistency** — don't mix conventions from two
   different reference files for the same kind of file, and don't invent a new pattern
   the docs don't cover without asking.
6. **Before finishing, re-check the diff** against every reference file read in steps
   2–3 — not only the specific thing the request asked for, but the rest of the
   file/section touched along the way.
7. If the code you're looking at contradicts both this skill and the project's own
   override doc, ask before proceeding rather than guessing which one is stale.

## Reference index — one row per layer, load each one your task actually touches

| File | Layer | When to load it |
|---|---|---|
| `GENERAL.md` | Cross-cutting JS style, naming, imports, formatting, refactoring rules | **Always — every task** |
| `REPOSITORY.md` | HTTP/API access layer | the task adds/changes a repository, or a store/service you're touching calls one |
| `SERVICE.md` | Business logic / calculations layer | the task adds/changes a service, or a store/component/view you're touching calls one |
| `STORE.md` | Reactive state layer (Pinia-shaped; adapt to an equivalent if the project uses something else, e.g. Vuex) | the task adds/changes a store, or a view/component you're touching reads/writes one |
| `COMPONENT.md` | Reusable Vue components | the task adds/changes a component |
| `VIEW.md` | Routed pages | the task adds/changes a view |
| `COMPOSABLE.md` | Reusable Vue-specific reactive logic | the task adds/changes a composable, or a component/view/store you're touching uses one |
| `ENUM.md` | Closed-category value sets | the task adds or references an enum |
| `CONSTANT.md` | Scattered fixed values tied to one file/subject | the task adds or references a constant |
| `CONFIG.md` | Values that must differ by environment | the task adds or references a config value |
| `UTIL.md` | Framework-independent helpers, and the schema-driven "filter" pattern | the task adds or calls a util/filter |
| `STYLE.md` | CSS/SCSS authoring rules (BEM, class ordering, partial organization) | the task adds/changes any styling |
| `LOCALE.md` | i18n: messages, enum labels, validation strings | the task adds/changes any user-facing copy |
| `COMMIT.md` | Commit message conventions (Conventional Commits) | writing the commit message for the task |

These aren't mutually exclusive — a single task commonly loads several of these at
once because the layers call each other (see step 3 above). Skip a row only when the
task truly never touches that layer, not because it wasn't the layer named first in
the request.

Architecture, at a glance (see `references/GENERAL.md` for the full explanation):

```text
Repository → Service → Store → View (→ Component)
```

Composables, Utils, Enums, Constants, Config, and Locale are cross-cutting support
layers usable from any of the above — not steps in the pipeline. Middleware (route
guards, HTTP interceptors) has no shared reference here — its conventions are always
project-specific; check the project's own override doc.

## What does not belong in these references

- Project-specific business logic, domain rules, or feature requirements.
- One-off decisions relevant to a single PR/ticket.
- A frozen snapshot of something derivable by reading the current code (exact file
  paths, current dependency versions).

If you're updating a reference file in this skill, keep it about shared
architecture/style, not any one project's content.
