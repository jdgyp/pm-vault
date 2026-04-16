# Setup Guide — PM Vault Automation

Follow these steps once to get the full system running.
Estimated time: 45–60 minutes.

---

## Step 1 — Set up your vault folder structure

In your existing Obsidian vault, create the following folders:

```
00-inbox/
01-pm-leadership/
  frameworks/
  team-practices/
  stakeholder-management/
  craft/
02-pm-portfolio/
  discovery/
  delivery/
  strategy/
  stakeholder/
03-artefacts/
  problem-briefs/
  prd-drafts/
  retros/
  decisions/
04-reference/
  books/
  talks/
  people/
99-meta/
```

Then copy these files from this package into your vault:

| File | Destination in vault |
|---|---|
| `Ideas Inbox.md` | `00-inbox/Ideas Inbox.md` |
| `CLAUDE-INSTRUCTIONS.md` | `99-meta/CLAUDE-INSTRUCTIONS.md` |

---

## Step 2 — Create a GitHub repo for your vault

Your vault needs its own GitHub repo (separate from pm-leadership and pm-portfolio).
Call it something like `pm-vault` or `obsidian-vault`.

1. Go to github.com → New repository
2. Name it `pm-vault` (private is fine — recommended)
3. Don't initialise with a README (your vault content will be the initial commit)

---

## Step 3 — Configure obsidian-git

Install the obsidian-git community plugin if you haven't already:

1. Obsidian → Settings → Community plugins → Browse → search "obsidian-git"
2. Install and enable it
3. Go to obsidian-git settings and configure:

| Setting | Recommended value |
|---|---|
| Vault backup interval | `5` (minutes) |
| Auto push on backup | ✅ Enabled |
| Commit message | `vault: auto-backup {{date}}` |
| Pull on startup | ✅ Enabled |
| Disable push | ❌ Off |

4. Run "Initialise git repo" from the command palette if not already done
5. Add your GitHub repo as the remote:
   ```
   git remote add origin https://github.com/YOUR-USERNAME/pm-vault.git
   git push -u origin main
   ```

From this point, any save in Obsidian will auto-commit and push within 5 minutes.

---

## Step 4 — Add the GitHub Actions workflow

1. In your vault repo, create the folder `.github/workflows/`
2. Copy `claude-curator.yml` from this package into that folder
3. Commit and push:
   ```
   git add .github/
   git commit -m "feat: add Claude curator workflow"
   git push
   ```

---

## Step 5 — Add your Anthropic API key as a secret

The workflow needs your Anthropic API key to call Claude.

1. Go to your vault repo on GitHub
2. Settings → Secrets and variables → Actions → New repository secret
3. Name: `ANTHROPIC_API_KEY`
4. Value: your key from console.anthropic.com

> The free tier of GitHub Actions gives you 2,000 minutes/month on private repos
> and unlimited on public repos. Each Claude run takes ~1–2 minutes.
> At 1–2 inbox updates per day, you'll use roughly 60–120 minutes/month —
> well within the free tier.

---

## Step 6 — Test the end-to-end flow

1. Open Obsidian and go to `00-inbox/Ideas Inbox.md`
2. Add a test idea below the `<!-- IDEAS GO HERE -->` marker:

```
Good PM practice I want to document: always write the success metric
before writing the solution. I've seen too many teams build features
where no one can agree afterwards whether it worked. The success metric
forces the conversation upfront.
```

3. Save the file — obsidian-git will commit and push within 5 minutes
4. Go to your GitHub repo → Actions tab
5. You should see a workflow run called "PM Vault — Claude curator" starting
6. Wait ~2 minutes for it to complete
7. Check the repo — you should see:
   - A new note filed somewhere under `01-pm-leadership/`
   - An updated `_index.md` in that folder
   - A new entry in `99-meta/CHANGELOG.md`
   - Your idea archived at the bottom of `Ideas Inbox.md`
8. Pull in Obsidian (obsidian-git will do this automatically on next startup,
   or run "Pull" from the command palette)

---

## Step 7 — Customise CLAUDE-INSTRUCTIONS.md

Once the basic flow is working, edit `99-meta/CLAUDE-INSTRUCTIONS.md` to
reflect your actual working context:

- What kind of PM work do you do?
- What domains or industries?
- What tone do you want in your notes?
- Are there specific frameworks you use regularly?
- What should Claude never do?

The better your instructions, the better the curation.

---

## Troubleshooting

**The workflow didn't trigger**
→ Check that the file path in `paths:` in the YAML exactly matches your
  inbox file path. It's case-sensitive.

**Claude returned invalid JSON**
→ Check the workflow logs — it prints the raw response. Usually means
  the context was too large. Reduce `related_notes` cap in the workflow
  (change `[:10]` to `[:5]`).

**Changes were committed but didn't appear in Obsidian**
→ Run "Pull" from the Obsidian command palette, or wait for the next
  auto-pull on startup.

**The workflow runs but nothing changed**
→ The inbox may have been empty (nothing below the marker). Check the
  "Check inbox has ideas" step in the workflow logs.

**API key error**
→ Double-check the secret name is exactly `ANTHROPIC_API_KEY` (all caps).

---

## Cost estimate

Claude Sonnet pricing (as of mid-2025):

| Item | Estimate |
|---|---|
| Input tokens per run | ~3,000–8,000 (depending on vault size) |
| Output tokens per run | ~1,000–3,000 |
| Cost per run | ~$0.01–0.04 |
| At 30 runs/month | ~$0.30–1.20/month |

This will increase as your vault grows (more context = more input tokens),
but it stays very cheap for personal use.
