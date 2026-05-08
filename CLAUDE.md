# CLAUDE.md

Guidance for Claude (and other AI assistants) working in this repository.

## Repository purpose

This is a **GitHub Skills exercise template** for the course *"Experience with the GitHub Copilot."* It is not a runnable application. It is a learner-facing course that uses GitHub Actions to drive a 5-step interactive walkthrough of GitHub Copilot features in VS Code + Codespaces:

0. Welcome
1. Enable the Copilot extension (add `.devcontainer/devcontainer.json`)
2. Use Copilot autocomplete (add `skills.js`)
3. Use the Copilot completions panel (add `member.js`)
4. Use comment-driven generation (add `comments.js`)
5. Finish

Each learner action triggers a workflow that advances the course state.

## Repository structure

```
.
├── .github/
│   ├── dependabot.yml
│   ├── steps/
│   │   ├── -step.txt              # Current step number (0–4) or "X" when finished
│   │   ├── 0-welcome.md           # Step content rendered into README.md
│   │   ├── 1-copilot-extension.md
│   │   ├── 2-skills-javascript.md
│   │   ├── 3-copilot-hub.md
│   │   ├── 4-copilot-comment.md
│   │   └── X-finish.md
│   └── workflows/
│       ├── 0-welcome.yml          # Bootstrap: 0 → 1 on repo creation
│       ├── 1-copilot-extension.yml
│       ├── 2-skills-javascript.yml
│       ├── 3-copilot-hub.yml
│       ├── 4-copilot-comment.yml
│       └── classroom.yml          # GitHub Classroom autograder
├── .gitignore
├── LICENSE                        # MIT
└── README.md                      # Overwritten by step transitions — see below
```

There is **no source code in the template**. Files like `skills.js`, `member.js`, `comments.js`, and `.devcontainer/devcontainer.json` are intentionally absent — the learner creates them.

## How the course engine works

The course is a small state machine driven by three pieces:

1. **`.github/steps/-step.txt`** — Single-line file holding the current step number. Used by `classroom.yml` to grade and by step workflows to gate themselves.
2. **`.github/workflows/N-*.yml`** — Each workflow watches for the file the learner is supposed to add at step N. When that file is committed, the workflow calls `skills/action-update-step@v2` to:
   - Bump `-step.txt` to `N+1` (or `X` after step 4).
   - Replace `README.md` with `.github/steps/(N+1)-*.md`.
   - Commit those changes back to the branch.
3. **`.github/workflows/classroom.yml`** — Runs on every push. Uses `classroom-resources/autograding-command-grader@v1` to score the learner by checking `-step.txt` ≥ each step number.

Because step transitions overwrite `README.md`, **edits to `README.md` will be clobbered** the next time a step advances. Edit the corresponding `.github/steps/N-*.md` file instead.

## Tech stack and tooling

- **Languages:** Markdown for content; YAML for workflows. JavaScript files are added by the learner during the course.
- **Runtime/build/test/lint:** None. There is no package manager, no test runner, no lint config. Verification is "do the workflows parse and trigger?"
- **Actions used:** `actions/checkout@v4`, `skills/action-update-step@v2`, `classroom-resources/autograding-command-grader@v1`. Pinning is by major version — preserve that style when editing.

## Conventions for AI assistants

- **Don't pre-create the learner's files** (`skills.js`, `member.js`, `comments.js`, `.devcontainer/devcontainer.json`). Their absence is what makes the course progress.
- **Don't edit `README.md` directly** for course-content changes. Edit `.github/steps/N-*.md` — `README.md` is regenerated.
- **Keep workflow action versions pinned by major** (`@v1`, `@v2`) to match existing style.
- **The `.github/steps/-step.txt` file is sacred state.** Don't manually edit it; let the workflows update it.
- **No build or lint to run before commit.** If you have `actionlint` available you can sanity-check workflow YAML; otherwise visual review is the bar.

## Development workflow

- **Default branch:** `main`.
- **Active development branch (per current task):** `claude/create-claude-md-export-z4G04`.
- **Never push to `main`** without explicit permission.
- Use HEREDOC for commit message bodies; keep messages short and descriptive of the *why*.
- Don't `git push --force`, don't use `--no-verify`, don't amend published commits.

## Key files quick reference

| Path | Role |
| --- | --- |
| `.github/steps/-step.txt` | Current step pointer (`0`–`4` or `X`) |
| `.github/steps/N-*.md` | Source of truth for the README at step N |
| `.github/workflows/N-*.yml` | Advances the course when step N's file is added |
| `.github/workflows/classroom.yml` | Autograder |
| `README.md` | Rendered view of the current step (auto-generated) |

## Git remote

The repo is mirrored through a local proxy:
`http://local_proxy@127.0.0.1:36615/git/Exp-Primer-Copilot-Cohort-2/experience-primer-copilot-kairos7139`
