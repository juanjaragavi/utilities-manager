# CLAUDE.md

## Project Overview

A **documentation-only hub** coordinating the Utilities Applications program: four developer-focused AI utilities being consolidated onto a shared backend and a single payment processor.

There is no application code here and none should be added. The deliverable is [README.md](README.md) — an architectural decision record holding the current-state assessment, target architecture, phased roadmap, and known blockers for that consolidation.

## Tech Stack

Markdown, plus Prettier for formatting. That is the entire stack.

- **Node** `>=22.13.0` (`engines`); Prettier `^3.9.8` is the only dependency
- **Package manager: npm.** `package.json` declares `"packageManager": "pnpm@11.25.0"`, but the only lockfile is `package-lock.json` and `node_modules/` was npm-installed. **Do not run `pnpm install`** — it would create a competing lockfile. Treat the `packageManager` field as inherited scaffold noise.
- `package.json` is also misnamed `"devspeak"` at version `1.1.4`. This is **not** the DevSpeak project; `devspeak` is a separate sibling repository this hub references. Do not treat the name as meaningful and do not "fix" either field without asking.

## Project Structure

```
README.md            The deliverable. Program ADR + roadmap. ~350 lines.
AGENTS.md            Agent instructions. Keep in sync with this file.
CLAUDE.md            This file.
.mcp.json            MCP server config (github, context7, playwright, devspeak, …).
lib/documents/       notes.md — local absolute paths to sibling repos. GITIGNORED.
.github/skills/      Seven vendored skills. See Skills below.
.agents/skills/      Byte-identical mirror of .github/skills/.
```

### Skills

Seven vendored skills, in two groups. All are generic — none contain repo-specific content.

**Documentation — writing verifiable claims about code that lives elsewhere:**

| Skill                            | Use when                                            |
| -------------------------------- | --------------------------------------------------- |
| `writing-for-agents`             | Editing `CLAUDE.md`, `AGENTS.md`, or any skill      |
| `verification-before-completion` | About to claim something passes, works, or is done  |
| `markdown-documentation`         | Writing or restructuring `README.md`                |
| `full-output-enforcement`        | Producing long documents that must not be truncated |

**Exploration — reading the six sibling repos, which is where all the code is:**

| Skill                  | Use when                                                          |
| ---------------------- | ----------------------------------------------------------------- |
| `codebase-exploration` | Answering "where is X?" across a sibling repo; three depth levels |
| `codebase-search`      | Tracing calls, pattern matching, locating implementations         |
| `context-map`          | Mapping every relevant file before a multi-file change            |

The exploration skills reference `fd` and `ast-grep`, **neither of which is installed on this machine**. `rg` is available. Substitute `find` for `fd`, and `rg` for `ast-grep` structural patterns, or install the tools.

`.agents/skills/` must stay byte-identical to `.github/skills/` — update both or neither.

### The repositories this hub coordinates

None live here. All six are expected to be cloned as **siblings** (`../<repo-name>`):

| Sibling                                                                          | Role                                                        |
| -------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `agent-spec-smith`, `safe-prompt-engine`, `dev-text-forge`, `prompt-craft-smith` | The four utility products                                   |
| `devspeak`                                                                       | Primary reference — the only codebase with working payments |
| `talentassisto`                                                                  | Secondary reference — i18n runtime and test-config patterns |

## Commands

```bash
npm install       # npm only — see Tech Stack
npm run lint      # prettier --check .
npm run format    # prettier --write .
```

Those two scripts are the complete set in `package.json`. There is **no build, no test runner, no CI** (`.github/workflows/` does not exist) and no deploy step. `lint` is the only gate, and it must pass before you commit.

## Conventions

**Formatting is Prettier defaults** — there is no config file. One consequence bites repeatedly: Prettier keeps short JSON arrays inline (`"args": ["-y", "@playwright/mcp@latest"]`). An editor that reflows them one-per-line will break `lint`. This has already happened once (commit `17a2b8b`, reverted by `15041a2`).

**Keep formatting fixes in their own commit.** Never sweep them into an unrelated change.

**Commits follow Conventional Commits** — `docs:`, `style:`, `fix:` are in use. Work happens directly on `main`; there is no branch convention and no PR process.

**Claims in `README.md` must be evidence-based.** Every architectural statement there was verified against a sibling repository. When updating it:

- Cite the file path a claim came from
- Do not restate a sibling's docs as fact without reading its code
- Two findings are easy to get wrong and are already corrected in the README: DevSpeak is **not** Next.js (Vite + React Router SPA with an Express 5 backend), and DevSpeak uses **Firestore, not Supabase**

**Link, don't duplicate.** This repo indexes other repos. Link to a sibling's docs rather than copying — copied content goes stale silently.

**Never link to `lib/documents/notes.md`.** It is gitignored, so the link 404s on GitHub. This broke the README once (fixed in `18908f2`). Reference siblings by repo name or relative path instead.

## Testing Expectations

There are no tests, no test runner, and nothing to mock. A change is done when:

1. `npm run lint` passes
2. Any factual claim added to `README.md` cites the sibling file it came from
3. Markdown links resolve — in particular, nothing links to a gitignored path

For `README.md` specifically, verify the table-of-contents anchors still match the headings after any heading change.

## Boundaries / Do-Not-Touch

| Path / item                                       | Why                                                                                                                                                          |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `node_modules/`, `.env`, `lib/documents/notes.md` | Gitignored. All three were committed once and had to be untracked (`18908f2`). Never re-add.                                                                 |
| `.env`                                            | Currently empty, but it exists in the history of `cfaa1d9`. Do not add values without discussing history scrubbing first.                                    |
| `package.json` name/version/packageManager        | Inherited scaffold noise (see Tech Stack). Changing them is a decision, not a cleanup.                                                                       |
| The Mermaid diagram in `README.md`                | `graph TB`. Avoid `&` edge chaining and em-dashes in node labels — both broke GitHub's renderer here. Parentheses are only safe in the `[(cylinder)]` shape. |
| Sibling repositories (`../devspeak`, etc.)        | Read-only from here. This repo documents them; it does not modify them.                                                                                      |

## Reference Documents

- **Program state, architecture, and roadmap:** [README.md](README.md) — read this first, before any task touching the consolidation plan. Contains the four-repo current-state assessment, the target architecture decisions and their rationale, a six-phase roadmap, and nine known blockers.
- **Agent instructions:** [AGENTS.md](AGENTS.md) — the same guidance as this file, for non-Claude agents. If you change a command or convention here, change it there too.

No `agent_docs/` directory exists, and none is warranted: the repo has six tracked files and no schema, API, or build system to document. Detail belongs in `README.md`, which is already the deep reference.

## Maintenance

Update this file when commands, structure, or conventions change — especially if a test runner, CI workflow, or Prettier config is ever added, since this file currently asserts that none exist. `AGENTS.md` carries the same content for other agents; keep the two in sync.
