# AGENTS.md

## What this repository is

A **documentation-only hub** for the Utilities Applications program. There is no application code here, and none should be added.

The deliverable is [README.md](README.md) — the program's architectural decision record and operating manual. It tracks the consolidation of four developer-focused AI utilities onto a shared backend and a single payment processor. Read it before doing anything else; it carries the current-state assessment, target architecture, phased roadmap, and known blockers.

## The repositories this hub coordinates

None of them live here. All six are expected to be cloned as **siblings** of this directory (`../<repo-name>`):

| Sibling                                                                          | Role                                                        |
| -------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `agent-spec-smith`, `safe-prompt-engine`, `dev-text-forge`, `prompt-craft-smith` | The four utility products                                   |
| `devspeak`                                                                       | Primary reference — the only codebase with working payments |
| `talentassisto`                                                                  | Secondary reference — i18n runtime and test-config patterns |

Local absolute paths are in `lib/documents/notes.md`, which is **gitignored on purpose** (machine-specific). Never link to it from `README.md` — the link 404s on GitHub. Reference siblings by relative path or repo name instead.

## Commands

```bash
npm run lint      # prettier --check .
npm run format    # prettier --write .
```

`lint` must pass before you commit. Keep formatting fixes in their own commit; never sweep them into an unrelated change.

There is no prettier config file; defaults apply. Prettier keeps short JSON arrays inline (`"args": ["-y", "@playwright/mcp@latest"]`), so an editor that reflows them one-per-line will fight `lint` — this has already broken the gate once.

## Conventions

**Never commit these.** They are gitignored and were removed from tracking once already:

- `node_modules/`
- `.env`
- `lib/documents/notes.md`

**`package.json` is misnamed `devspeak` with version `1.1.4`** — inherited from a copied scaffold. It is not the DevSpeak project. Do not treat that name as meaningful, and do not "fix" it without asking; other tooling may key off it.

**Claims in `README.md` must be evidence-based.** Every architectural statement there was verified against a sibling repository. When you update it:

- Cite the file path the claim came from
- Do not restate a sibling repo's docs as fact without reading the code
- Two findings are easy to get wrong and are already corrected in the README: DevSpeak is **not** Next.js (Vite + React Router SPA with an Express 5 backend), and DevSpeak uses **Firestore, not Supabase**

**Link, don't duplicate.** This repo indexes other repos. When a sibling already documents something, link to it rather than copying — copied content goes stale silently.

## Editing README.md

It is a long structured document with a table of contents whose anchors must stay in sync with headings. It contains one Mermaid diagram (`graph TB`); avoid `&` edge chaining and em-dashes in node labels, both of which broke GitHub's renderer here. Parentheses are only safe in the `[(cylinder)]` shape.

Prefer targeted string edits over rewriting the file.

## Customization directories

`.github/{agents,prompts,skills}/` and `.agents/{agents,prompts,skills}/` exist but are empty. `.agents/` is a copy of `.github/`. If you add a customization, put it in `.github/` and mirror it only if asked.
