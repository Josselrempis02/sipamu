# agent-Sipumā (シプマー)

A personal Claude Code skill set for full-stack software engineering — C#/.NET, Node/Express, React/React Native (Expo), and T-SQL/SQL Server.

Named after **Sipmer** → シプマー (*Shipumā / Sipumā*), with each skill taking a Japanese role name.

## Skills

| Skill | Kanji | Role | Use when |
|---|---|---|---|
| `shogun` | 将軍 | Orchestrator | A task is large or multi-part and needs a plan before code |
| `takumi` | 匠 | Builder | Implementing features, endpoints, procs, screens |
| `sensei` | 先生 | Reviewer | Reviewing a diff, PR, file, or stored procedure |
| `kintsugi` | 金継ぎ | Debugger | Something is broken, throwing, slow, or wrong |

## Structure

```
agent-Sipumā/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── shogun/SKILL.md
│   ├── takumi/SKILL.md
│   ├── sensei/SKILL.md
│   └── kintsugi/SKILL.md
├── .gitignore
└── README.md
```

## Install

**Option A — personal skills (all projects):** copy each folder under `skills/` into `~/.claude/skills/`.

```bash
cp -r skills/* ~/.claude/skills/
```

**Option B — project skills (one repo):** copy into that project's `.claude/skills/`.

**Option C — as a plugin:** point Claude Code at this folder with `--plugin-dir`, or publish the repo to GitHub and add it through a plugin marketplace.

## Adding a skill

1. Create `skills/<name>/SKILL.md` — `<name>` must be lowercase letters, numbers, and hyphens.
2. Frontmatter needs `name` (matching the folder) and `description` (this decides when the skill triggers — say *when*, not just *what*).
3. Keep `SKILL.md` focused; move long reference material into sibling files and link them.

## Author

Jossel Alfred R. Rempis (Aj / Sipmer)
