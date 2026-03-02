# Full Tooling Inventory

## Global Commands (`~/.claude/commands/`)

| Command | Model | Purpose |
|---|---|---|
| `/requirement-intake` | sonnet | NL requirement → REQ spec → route to Obsidian dept folder |
| `/obsidian-doc-sync` | sonnet | Post-feature: sync code changes → Obsidian vault |
| `/code-review` | sonnet | Review uncommitted changes against universal checklist |
| `/auto-commit` | haiku | Stage + conventional commit with AI message |
| `/quick-search` | haiku | Fast codebase search via Explore subagent |

## Global Agents (`~/.claude/agents/`)

| Agent | Model | Purpose |
|---|---|---|
| `code-reviewer` | sonnet | Project-aware code review (reads CLAUDE.md first) |
| `cross-project-scout` | sonnet | Cross-repo pattern search, utility detection, dep audit |

## Project-Local: `project_root` only (`.claude/commands/` + `.claude/skills/`)

The only project with a real delivery pipeline:

```
/plan  →  /build  →  /update-docs  →  /auto-commit
  ↕           ↕           ↕
planner    developer    updater      (skills backing each command)
sonnet      opus        sonnet
```

| Command/Skill | Model | Purpose |
|---|---|---|
| `/plan` + `planner` | sonnet | Read design docs → structured implementation plan |
| `/build` + `developer` | opus | Implement plan → run pytest → implementation summary |
| `/update-docs` + `updater` | sonnet | Diff code → update ERD/architecture/decisions docs |
| `/code-review` + `code-reviewer` | sonnet | ICP-specific review: PEP8, type hints, SQL safety |
| `/quick-search` | haiku | ICP-scoped fast search |
| `/auto-commit` | haiku | Commit |
| `/weekly-report` | sonnet | Git history → weekly summary |

## Plugin Skills (`~/.claude/plugins/`)

### superpowers (always active)

| Skill | Trigger |
|---|---|
| `brainstorming` | Before any creative/feature work |
| `writing-plans` | Create implementation plans |
| `executing-plans` | Execute a written plan |
| `subagent-driven-development` | Orchestrate spec/implement/review subagents |
| `dispatching-parallel-agents` | 2+ independent tasks in parallel |
| `using-git-worktrees` | Isolated branch work |
| `systematic-debugging` | Any bug/test failure |
| `test-driven-development` | Before writing implementation code |
| `verification-before-completion` | Before claiming work done |
| `finishing-a-development-branch` | Merge/PR decision |
| `requesting-code-review` | After feature completion |
| `receiving-code-review` | When handling review feedback |
| `writing-skills` | Creating new skills |

### claude-reflect (self-learning)

| Command | Purpose |
|---|---|
| `/reflect` | Process queued learnings → update CLAUDE.md |
| `/reflect-skills` | Discover skill candidates from session patterns |
| `/view-queue` | See pending learnings |

Hooks fire on every session: `UserPromptSubmit`, `PostToolUse`, `PreCompact` — captures corrections and patterns into `learnings-queue.json`.

### ralph-loop

`/ralph-loop` — continuous Claude loop for iterative tasks until completion.

### voltagent-infra

14 DevOps/cloud specialist subagents (Azure, K8s, Terraform, SRE, etc.) — not yet used in your dev workflow.

## Pattern Map: What You Actually Use

```
INTAKE                 PLANNING              DELIVERY              DOCS/LEARNING
─────────────────────────────────────────────────────────────────────────────────
/requirement-intake    brainstorming skill   /build (opus)         /update-docs
  → Obsidian spec      writing-plans skill   /code-review          /obsidian-doc-sync
                       /plan (sonnet)        /auto-commit          /reflect
                                             ralph-loop            cross-project-scout
```
