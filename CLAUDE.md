# AI-Brainstorm — Shared Brain for Claude Code

A **persistent shared brain** for AI assistants working across sessions.
Works in two modes: Solo (one AI) and Collab (multiple AIs).

---

## How This Works

You are an AI assistant participating in this brainstorm system.
Follow these rules every session:

### On Session Start — Always Do This First

1. Read `brain/_index.md` — see the dashboard (all topics, statuses, agent activity)
2. Read `brain/shared/findings.md` — see what's been concluded
3. Read `brain/inbox.md` — see if there are new ideas/questions pending
4. Scan `brain/topics/` — check open topics (Status: OPEN)

If this is a **Collab session** (user tells you which agent you are):

5. Read `brain/agents/{your-letter}/inbox.md` — check messages from other agents
6. After reading, move processed messages to the `## Archive` section at bottom

### On Every Write — Always Do This After

7. Update `brain/_index.md` whenever you create/resolve a topic, add a decision, or change status

---

## Directory Structure

```
brain/
  _index.md         — Dashboard: overview of all topics, decisions, agent status
  inbox.md          — Drop new ideas, questions, random thoughts here
  references.md     — External URLs, articles, docs cited during research
  topics/           — One file per research topic or problem
  decisions/        — Conclusions and decisions that are final
  workspace/        — Scratch space for research, analysis, drafts, temp files
  datasets/         — Downloaded data, API responses, CSVs, reference files
    _catalog.md     — Index of all datasets with source and re-download info
  agents/           — Mailboxes for multi-AI collaboration
    A/inbox.md      — Messages TO Agent A
    B/inbox.md      — Messages TO Agent B
  shared/
    findings.md     — Verified findings agreed upon by all agents
    experiments.md  — Structured log: what was tried, result, keep/discard
```

---

## Naming Convention

All numbered files use **zero-padded prefix with underscore**:

```
01_topic-name.md        (NOT 1-topic-name.md)
02_another-topic.md
03_third-topic.md
...
10_tenth-topic.md
```

This ensures correct sort order in file explorers and `ls`.
Next number = highest existing number + 1.

Applies to: `topics/`, `decisions/`, `workspace/`, `datasets/`

---

## Rules for Writing

### Creating a Topic
- Use template from `templates/topic.md`
- File name: `NN_short-name.md` (e.g., `01_auth-bug.md`)
- Update `brain/_index.md` after creating

### Updating a Topic
- Add new entries under `## Discussion` with session timestamp
- Format: `### [Session — YYYY-MM-DD HH:MM]`
- When resolved: change Status to `RESOLVED`, fill `## Resolution`, update `_index.md`

### Splitting a Long Topic
- If a topic file grows beyond **~200 lines**, convert it to a folder:
  ```
  topics/01_api-design.md
  →
  topics/01_api-design/
    _main.md              ← Original topic (context, status, resolution)
    01_initial-analysis.md  ← First discussion thread
    02_auth-options.md      ← Second thread
    03_final-conclusion.md  ← Third thread
  ```
- `_main.md` keeps the overview; threads are numbered sub-conversations
- Reference: `[thread 02](01_api-design/02_auth-options.md)`

### Recording a Decision
- Use template from `templates/decision.md`
- File name: `NN_short-name.md` (e.g., `01_use-jwt.md`)
- Only write here when something is **confirmed and final**
- Reference the topic(s) that led to this decision
- Update `brain/_index.md` after creating

### Sending a Message (Collab Mode)
- Write to the OTHER agent's inbox: `brain/agents/{other}/inbox.md`
- Add under `## Pending` section with timestamp
- Format: `### [From {you} — YYYY-MM-DD HH:MM]`
- Be specific: include data, references to topics, and clear questions
- Update agent status in `brain/_index.md`

### Storing Data (Datasets)
- Use `brain/datasets/` for all external data: API responses, CSV downloads, scraped data, reference docs
- File name: `NN_description.ext` (e.g., `01_user-survey.csv`, `02_api-response.json`)
- Always add an entry to `brain/datasets/_catalog.md`:
  - What it is, where it came from (URL/API/command), when it was fetched, format, size
- Large files (>1MB): add to `.gitignore` and note in `_catalog.md` how to reproduce/re-download
- Reference datasets from workspace/topics using: `**Data:** [filename](../datasets/01_data.csv)`

### Using Workspace (Scratch Space)
- Use `brain/workspace/` for all research artifacts: data analysis, drafts, calculations, temp files
- File name: `NN_short-description.md` (e.g., `01_performance-analysis.md`)
- Workspace files are **working documents** — messy is OK, they are not meant to be polished
- When a workspace file produces a useful conclusion → summarize it into `brain/topics/` or `brain/decisions/`
- Periodically clean up: delete workspace files that are no longer relevant

### Logging Experiments (REQUIRED)
- **Every time you try something** — a new approach, a test, a comparison — log it to `brain/shared/experiments.md`
- Add a row to the table with: date, related topic, what was tried, result, status, sources
- Status values: `keep` (worked), `discard` (didn't work), `partial` (needs more work), `crash` (failed)
- Log failures too — knowing what DOESN'T work is just as valuable
- Check experiments.md before trying something new — don't repeat what already failed
- When patterns emerge across experiments, add them to the `## Insights` section

### Recording a Finding
- Add to `brain/shared/findings.md`
- Only write findings that are **verified with evidence**
- Include: what was found, evidence/data, and which topic it relates to

### Citing External Sources
- Add to `brain/references.md` whenever you reference an external URL, article, or doc
- Include: title, URL, which topic/workspace file uses it, date added
- In your writing, link back: `[see ref](../references.md)`

### Source References (REQUIRED)
- **Every entry** in topics, inbox, or agent messages MUST include a `**Sources:**` line
- List the files you read or created during that entry
- Format: `**Sources:** [filename](relative/path), [filename](relative/path)`
- Example: `**Sources:** [perf-analysis](../workspace/03_performance-analysis.md), [api-design](../topics/01_api-design.md)`
- For external sources, also add: `**Refs:** [title](../references.md)`
- This lets anyone trace back HOW a conclusion was reached

---

## Conventions

- Statuses: `OPEN`, `IN-PROGRESS`, `RESOLVED`, `ABANDONED`
- Priority: `HIGH`, `MEDIUM`, `LOW`
- Keep each topic file focused — split if it grows beyond ~200 lines
- Use relative links between files: `[see topic](topics/01_example.md)`
- Language: use whatever language the user prefers

---

## For the Human

You can participate too:
- Read `brain/_index.md` for a quick overview of everything
- Drop ideas in `brain/inbox.md`
- Read `brain/shared/findings.md` for the latest conclusions
- Check `brain/references.md` for all external links cited
- In Collab mode, just tell each terminal: "check inbox" — no need to relay messages yourself
