# PM Knowledge Vault — Folder Structure

This is the recommended folder structure for your Obsidian vault.
Set this up before wiring in the GitHub Actions automation — Claude uses
the folder layout to decide where to file things.

---

## Folder tree

```
vault/
│
├── 00-inbox/
│   └── Ideas Inbox.md          ← YOU WRITE HERE. Append freely, no formatting needed.
│
├── 01-pm-leadership/
│   ├── _index.md               ← MoC: auto-maintained by Claude
│   ├── frameworks/             ← PM methodologies, templates, decision tools
│   ├── team-practices/         ← Ways of working, rituals, team agreements
│   ├── stakeholder-management/ ← Influence, comms, exec alignment
│   └── craft/                  ← Writing, prioritisation, strategy skills
│
├── 02-pm-portfolio/
│   ├── _index.md               ← MoC: auto-maintained by Claude
│   ├── discovery/              ← Research, insight, problem framing examples
│   ├── delivery/               ← Sprint, execution, unblocking examples
│   ├── strategy/               ← Roadmap, vision, bet-making examples
│   └── stakeholder/            ← Influence, alignment, difficult conversation examples
│
├── 03-artefacts/
│   ├── _index.md               ← MoC: auto-maintained by Claude
│   ├── problem-briefs/         ← Expanded from inbox ideas
│   ├── prd-drafts/             ← Product requirement drafts
│   ├── retros/                 ← Retrospective notes
│   └── decisions/              ← ADRs and decision logs
│
├── 04-reference/
│   ├── _index.md
│   ├── books/                  ← Reading notes, book summaries
│   ├── talks/                  ← Conference notes, talk summaries
│   └── people/                 ← Influential thinkers, their key ideas
│
└── 99-meta/
    ├── CHANGELOG.md            ← Auto-written by Claude after each run
    ├── CLAUDE-INSTRUCTIONS.md  ← System prompt / curator instructions (YOU edit this)
    └── vault-stats.md          ← Auto-updated: note count, last run, coverage
```

---

## How Claude uses this structure

When Claude processes your inbox, it reads the entire vault tree and:

1. **Files the idea** into the most relevant folder under `01`, `02`, or `03`
2. **Creates a new note** if the idea warrants one, or **amends an existing note** if it builds on prior content
3. **Updates the relevant `_index.md`** (MoC) to link to new/changed content
4. **Writes a CHANGELOG entry** describing what changed and why
5. **Archives inbox items** by moving them into a dated section at the bottom of the Inbox note, clearing the top

---

## Setting up the vault in Obsidian

1. Create the folders above in your existing vault
2. Create `00-inbox/Ideas Inbox.md` with this starter content:

```markdown
# Ideas Inbox

> Append ideas below. Claude will process and file them automatically.
> No formatting needed — just write.

---

<!-- IDEAS GO HERE -->

```

3. Create `99-meta/CLAUDE-INSTRUCTIONS.md` — see the CLAUDE-INSTRUCTIONS template file

---

## GitHub repo mapping

| Vault folder | GitHub repo |
|---|---|
| `01-pm-leadership/` | `pm-leadership` |
| `02-pm-portfolio/` | `pm-portfolio` |
| `03-artefacts/` | Subfolder in `pm-leadership` or `pm-portfolio` (your choice) |

Your vault itself lives in a third repo (e.g. `pm-vault`) — this is the one
obsidian-git pushes to, and where GitHub Actions runs.
