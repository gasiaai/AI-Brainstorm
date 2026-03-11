# AI-Brainstorm

A shared brain system for [Claude Code](https://claude.com/claude-code) — persistent memory across sessions, with multi-AI collaboration via mailbox pattern.

## The Problem

Claude Code forgets everything between sessions. Even with memory files, complex research and multi-step reasoning gets lost. If you run two Claude Code instances on the same project, they can't talk to each other without you manually relaying messages.

## The Solution

A simple folder structure + `CLAUDE.md` rules that give Claude Code:
- **Persistent memory** via organized Markdown files
- **Bot-to-bot communication** via a mailbox system
- **Dashboard** to see everything at a glance
- **No dependencies** — just Markdown, no installs needed

## Quick Start

### Solo Mode (One AI)

```bash
git clone https://github.com/gasiaai/AI-Brainstorm.git
cd AI-Brainstorm
claude  # Start Claude Code — it reads CLAUDE.md automatically
```

Then just work normally. Claude will:
- Check `brain/_index.md` for the dashboard overview
- Check `brain/inbox.md` for pending ideas
- Create topic files for new research questions
- Store data in `brain/datasets/` with full provenance
- Record decisions when conclusions are reached
- Build up `brain/shared/findings.md` over time

### Collab Mode (Two+ AIs)

Open two terminals in the same project:

**Terminal 1:**
```bash
claude
> "You are Agent A. Check your inbox and work on open topics."
```

**Terminal 2:**
```bash
claude
> "You are Agent B. Check your inbox and work on open topics."
```

To make them communicate:
```
Terminal 1: "Send your analysis to Agent B"
Terminal 2: "Check inbox"   # That's it — no need to relay the message yourself
```

## Structure

```
AI-Brainstorm/
├── CLAUDE.md                 # Rules for AI (auto-loaded by Claude Code)
├── brain/
│   ├── _index.md             # Dashboard: all topics, decisions, agent status
│   ├── inbox.md              # New ideas & questions
│   ├── references.md         # External URLs, articles, docs cited
│   ├── topics/               # One file per research topic
│   │   └── 01_example.md     # Numbered for sort order
│   ├── decisions/            # Final conclusions
│   │   └── 01_example.md
│   ├── workspace/            # Scratch space for research & temp files
│   ├── datasets/             # Downloaded data, CSVs, API responses
│   │   └── _catalog.md       # Index: what, where from, how to re-download
│   ├── agents/               # Mailboxes (collab mode only)
│   │   ├── A/inbox.md
│   │   └── B/inbox.md
│   └── shared/
│       ├── findings.md       # Verified findings
│       └── experiments.md    # Structured log: tried/result/keep/discard
└── templates/
    ├── topic.md
    └── decision.md
```

## How It Works

### Solo Mode
```
Session 1: Research a bug → creates brain/topics/01_auth-bug.md
Session 2: Claude reads _index.md → sees open topic → continues where it left off
Session 3: Bug solved → writes brain/decisions/01_use-jwt.md, updates findings.md
```

### Collab Mode
```
Agent A writes analysis    → brain/agents/B/inbox.md
You tell B: "check inbox"  → Agent B reads, responds
Agent B writes reply       → brain/agents/A/inbox.md
You tell A: "check inbox"  → Agent A reads, continues

Both agents write shared conclusions → brain/shared/findings.md
```

### Long Topic → Auto-split
```
When a topic grows too large (200+ lines), it becomes a folder:

topics/01_api-design.md
→
topics/01_api-design/
  _main.md                  ← Overview (context, status, resolution)
  01_initial-analysis.md    ← Thread 1
  02_auth-options.md        ← Thread 2
  03_final-conclusion.md    ← Thread 3
```

## Key Features

| Feature | What it does |
|---------|-------------|
| **`_index.md` dashboard** | See all topics, decisions, and agent activity at a glance |
| **`NN_` numbering** | Files sort correctly: `01_`, `02_`, ... `10_` |
| **`references.md`** | Track every external URL/article/doc cited |
| **`datasets/` + `_catalog.md`** | Store data with full provenance (source, date, re-download instructions) |
| **`workspace/`** | Messy research stays separate from clean topics |
| **`Sources:` required** | Every entry must cite which files it used — full traceability |
| **`experiments.md` log** | Structured keep/discard log of everything tried |
| **Auto-split** | Long topics become folders with numbered sub-threads |
| **Mailbox pattern** | Multi-AI communication without human relay |

## Tips

- **Keep topics focused** — one problem per file, split if it grows too large
- **Use inbox.md freely** — dump half-baked ideas, questions, TODOs
- **decisions/ is sacred** — only put confirmed, final conclusions there
- **workspace/ is the sandbox** — messy research goes here, keeps topics/ clean
- **datasets/ stores raw data** — every file logged in `_catalog.md` with source and re-fetch instructions
- **references.md tracks links** — every external URL cited gets logged here
- **Sources are required** — every entry must list which files were read/created
- **Add more agents** — just create `brain/agents/C/inbox.md`, `D/inbox.md`, etc.
- **Works with any project** — copy this structure into any existing repo

## Adapting for Your Project

1. Copy the `brain/`, `templates/`, and `CLAUDE.md` into your project
2. Add your project-specific context to `CLAUDE.md`
3. Start a Claude Code session — it picks up the rules automatically

## License

MIT — use it however you want.
