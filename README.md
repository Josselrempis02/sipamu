# agent-Sipumā (シプマー)

Personal skill set for Claude Code and Codex. Built for full-stack work in C#/.NET, Aspire, Node, React/React Native, and web UI.

The repo is its own plugin marketplace. Install once and you get every stable skill, grouped by area.

## Quick start

```bash
claude plugin marketplace add Josselrempis02/sipamu
claude plugin install agent-sipuma@sipmer
```

Skills are namespaced under the plugin: `/agent-sipuma:tdd`, `/agent-sipuma:code-review`, and so on.

Pull updates:

```bash
claude plugin marketplace update sipmer
```

Not sure where to start? Run `/agent-sipuma:ask-matt`. It routes you to the skill or flow that fits your situation.

### Other ways to install

| Method | Command | Scope |
|---|---|---|
| Local checkout | `claude --plugin-dir ./sipuma` | Current session |
| Single skill, all projects | `cp -r skills/engineering/tdd ~/.claude/skills/` | Your user |
| Single skill, one project | `cp -r skills/engineering/tdd <repo>/.claude/skills/` | That repo |

## How skills trigger

| Type | Behavior | Set by |
|---|---|---|
| **model** | Claude loads it when the task matches its `description`. You can also call it by name. | Default |
| **user** | Runs only when you type it. | `disable-model-invocation: true` (Claude Code), `policy.allow_implicit_invocation: false` in `agents/openai.yaml` (Codex) |

## Typical flow

For work bigger than one sitting:

```
grill-with-docs  →  to-spec  →  to-tickets  →  implement / implement-spec  →  code-review  →  pr
   sharpen plan      write spec   slice work      build it                       check it        ship it
```

For work too big to plan in one session, start with `wayfinder`. For a bug, go straight to `diagnosing-bugs`.

## Skills

### Engineering

**Model-invoked**

| Skill | What it does |
|---|---|
| `tdd` | Red-green-refactor, one vertical slice at a time |
| `testing` | Adds, fixes, and runs tests; sets the repo's testing conventions |
| `diagnosing-bugs` | Reproduce, minimise, hypothesise, fix, regression-test. For hard bugs and perf regressions |
| `code-review` | Reviews a diff on two axes: repo standards and the originating spec |
| `codebase-design` | Shared vocabulary for deep modules, seams, and testable interfaces |
| `domain-modeling` | Sharpens domain terms; keeps `GLOSSARY.md` and ADRs current |
| `prototype` | Throwaway prototype to answer a logic or UI question |
| `research` | Reads primary sources and writes a cited Markdown file |
| `pr` | Writes a PR body: summary, evidence, merge danger |
| `git-commit` | Commit message conventions |
| `wizard` | Generates an interactive bash wizard for steps only a human can do |

**User-invoked**

| Skill | What it does |
|---|---|
| `ask-matt` | Router: which skill or flow fits |
| `grill-with-docs` | Interview that sharpens a plan, writing ADRs and glossary entries as it goes |
| `to-spec` | Turns the conversation into a spec on the issue tracker |
| `to-tickets` | Splits a plan into tracer-bullet tickets with blocking edges |
| `implement` | Builds the work from a spec or tickets |
| `implement-spec` | Implements a whole spec on one branch with parallel subagents |
| `wayfinder` | Maps oversized work as a set of decision tickets |
| `triage` | Moves issues and PRs through triage states; writes agent-ready briefs |
| `improve-codebase-architecture` | Finds deepening opportunities and reports them as HTML |
| `thermo-nuclear-code-quality-review` | Very strict maintainability review |
| `second-opinion` | Second opinion on the current decision or design |
| `retro` | Retrospective on a coding session |
| `setup-matt-pocock-skills` | One-time repo setup: issue tracker, triage labels, doc layout |

### .NET

All model-invoked.

| Skill | What it does |
|---|---|
| `dotnet-conventions` | C#/.NET conventions for writing, reviewing, and scaffolding |
| `dotnet-inspect` | Gathers evidence on packages, assemblies, and APIs instead of guessing |
| `analyzing-dotnet-performance` | Scans for ~50 performance anti-patterns, ranked by severity |

### Aspire

All model-invoked. Start with `aspire`; it routes to the rest.

| Skill | What it does |
|---|---|
| `aspire` | Router: detects AppHosts and hands off to the right sub-skill |
| `aspire-init` | Adds Aspire to a repo (`aspire new` / `aspire init`) |
| `aspireify` | Wires resources, ServiceDefaults, and OTel into an AppHost |
| `aspire-orchestration` | Start, stop, wait, restart, worktree isolation |
| `aspire-monitoring` | Logs, traces, metrics, dashboard |
| `aspire-deployment` | Deploys to Docker Compose, Kubernetes, Azure, AWS, or Radius |
| `aspire-project-v2-migration` | Migrates `ProjectResource` to `DotnetProjectResource` |

### Web

All model-invoked.

| Skill | What it does |
|---|---|
| `impeccable` | Design, critique, audit, and polish frontend UI; captures design systems in `DESIGN.md` |
| `interface-design` | Craft-first product UI: dashboards, admin panels, tools |
| `web-design-guidelines` | Audits UI code against the Web Interface Guidelines |
| `playwright-cli` | Drives a real browser to verify UI changes and run Playwright tests |

### Productivity

| Skill | Trigger | What it does |
|---|---|---|
| `grilling` | model | Interviews you hard on a plan or idea |
| `writing-for-agents` | model | Writes skills, `AGENTS.md`, and `CLAUDE.md` |
| `grill-me` | user | Interviews you until every branch of the design is resolved |
| `handoff` | user | Compacts the conversation into a handoff doc |
| `teach` | user | Teaches a concept over several sessions |
| `to-questionnaire` | user | Turns an open decision into a questionnaire for someone else |
| `wait-what` | user | Re-explains the last message when it didn't land |

### Not shipped in the plugin

| Folder | Contents |
|---|---|
| [`skills/misc/`](skills/misc/README.md) | Rarely used tools: git guardrails, pre-commit setup |
| [`skills/in-progress/`](skills/in-progress/README.md) | Beta skills; may change or disappear |
| [`skills/deprecated/`](skills/deprecated/README.md) | Retired skills |

## Repo layout

```
sipuma/
├── .claude-plugin/
│   ├── marketplace.json     # marketplace "sipmer"
│   └── plugin.json          # plugin "agent-sipuma" + the skill folders it ships
├── skills/
│   ├── aspire/
│   ├── dotnet/
│   ├── engineering/
│   ├── productivity/
│   ├── web/
│   ├── misc/                # not shipped
│   ├── in-progress/         # not shipped
│   └── deprecated/          # not shipped
├── THIRD_PARTY_NOTICES.md
└── README.md
```

Each skill is a folder with a `SKILL.md` plus any reference files it links to. Codex metadata, where present, lives in `agents/openai.yaml`.

## Adding a skill

1. Create `skills/<bucket>/<name>/SKILL.md`. Use lowercase letters, numbers, and hyphens for `<name>`.
2. In the frontmatter, set `name` to match the folder and write a `description`. The description decides when the skill triggers, so say *when* to use it, not just *what* it does.
3. Add `disable-model-invocation: true` if it should run only when typed.
4. Keep `SKILL.md` short. Move long reference material into sibling files and link to them.
5. New bucket? Add it under `skills` in `.claude-plugin/plugin.json`.

## Credits

Most of `skills/engineering/` and `skills/productivity/` come from [mattpocock/skills](https://github.com/mattpocock/skills) by Matt Pocock (MIT). See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

## Author

Jossel Alfred R. Rempis (Aj / Sipmer)
