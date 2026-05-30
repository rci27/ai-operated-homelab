# Publishing & Promotion Runbook

Everything needed to take this from a folder on your laptop to a public, auto-updating website that's easy to find. After the one-time setup, **every `git push` rebuilds and republishes the site automatically** — no further manual steps.

---

## 0. Decisions to make first

| Decision | Default | Notes |
|---|---|---|
| Repo name | `ai-operated-homelab` | Keyword-rich and readable. Used in `_config.yml` `baseurl` and badge URLs. |
| Public or private | **Public** | GitHub Pages is free for public repos and required for discoverability. |
| Custom domain? | Optional | Default URL is `https://<rci27>.github.io/ai-operated-homelab/`. Custom domain via your own DNS is in §6. |
| Author name | `rci27` | Pre-filled. This is the credit name shown publicly (CC BY 4.0 attribution). Change it in `_config.yml` and `LICENSE` if you want your real name or `TheRonLab` instead. |

The repo is already filled in for the GitHub user `rci27` and repo name `ai-operated-homelab`. You only need to edit anything if you (a) rename the repo, (b) want a different public author name, or (c) add a custom domain (§6). Otherwise it's ready to push as-is.

---

## 1. Create the repo and push (the fast path with `gh`)

From inside the unpacked `site/` folder on your laptop:

```bash
git init -b main
git add .
git commit -m "Initial publish: AI-operated homelab guide"

# Create the public repo and push in one step
gh repo create ai-operated-homelab --public --source=. --remote=origin --push \
  --description "How to let an AI safely operate a self-hosted server — a broker, a control plane, and the discipline that makes it work."
```

No `gh`? Create an empty public repo on github.com, then:

```bash
git remote add origin https://github.com/rci27/ai-operated-homelab.git
git push -u origin main
```

> **Fits your existing workflow:** this is a clean job for Claude Code — hand it the folder and the commands above, and have it run the find-and-replace, commit, and push. Review the diff before it pushes (your standard gate-before-commit rule).

---

## 2. Turn on GitHub Pages (one click)

On github.com → your repo → **Settings → Pages → Build and deployment → Source → "GitHub Actions"**.

That's it. The included workflow (`.github/workflows/deploy.yml`) takes over. Watch it run under the **Actions** tab; when it's green, your site is live at the Pages URL. Update `url` / `baseurl` in `_config.yml` to match if you haven't already, and push again.

---

## 3. Make it discoverable *inside* GitHub

GitHub's own search ranks on the repo description and **topics** — these are high-leverage and take 30 seconds:

```bash
gh repo edit rci27/ai-operated-homelab \
  --description "Let an AI safely operate your self-hosted server: a deny-by-default broker, a human-in-the-loop control plane, and the discipline that makes it work." \
  --homepage "https://rci27.github.io/ai-operated-homelab/" \
  --add-topic homelab --add-topic self-hosted --add-topic proxmox \
  --add-topic ai --add-topic claude --add-topic automation \
  --add-topic devops --add-topic cloudflare-tunnel --add-topic tailscale \
  --add-topic ntfy --add-topic obsidian --add-topic llm --add-topic infrastructure
```

---

## 4. Set the social-share preview image

Two places, because they serve different surfaces:

1. **For the website** (Twitter/LinkedIn/Slack/Discord link previews): already wired — `_config.yml` points Open Graph + Twitter cards at `assets/social-preview.png`. Nothing to do.
2. **For the GitHub repo card** (when someone shares the *repo* link): Settings → **General → Social preview → Edit → upload** `assets/social-preview.png`. GitHub doesn't read it from the repo automatically; this upload is manual and one-time.

Verify the preview renders by pasting your live URL into the [opengraph.xyz](https://www.opengraph.xyz/) checker.

---

## 5. Search-engine optimization — what's already done, and the last steps

Baked in already: a real `<title>` and meta description, Open Graph + Twitter cards, a canonical URL, semantic headings, and an auto-generated `sitemap.xml` (via `jekyll-sitemap`) plus a `robots.txt` that points crawlers at it.

The remaining steps that actually move the needle:

- **Submit the sitemap to Google.** [Google Search Console](https://search.google.com/search-console) → add your property → Sitemaps → submit `sitemap.xml`. This is the single biggest lever for getting indexed quickly.
- **Get a few inbound links.** Search ranking leans heavily on links from other pages. The promotion in §7 *is* your link-building — a single front-page Reddit/HN post does more than weeks of on-page tweaking.
- **Keep the title aligned with how people search.** Phrases real people type: "AI homelab," "let AI manage my server," "safe AI agent infrastructure," "Claude Code homelab." The current title and description already lean into these.

---

## 6. Optional: serve it on your own domain

Two routes; pick one.

**A — GitHub Pages custom domain (recommended, zero maintenance).**
1. Add a file named `CNAME` at the repo root containing just your hostname, e.g. `homelab.example.com`.
2. At your DNS provider, add a `CNAME` record: `homelab` → `rci27.github.io`.
3. In Settings → Pages, set the custom domain and tick **Enforce HTTPS**.
4. Set `_config.yml` → `baseurl: ""` and `url: "https://homelab.example.com"`, then push.

> If your DNS is on Cloudflare, set that record to **DNS only (grey cloud)** initially so GitHub can issue its certificate; you can proxy it later once HTTPS is confirmed.

**B — Host it yourself** behind your existing Cloudflare Tunnel + reverse proxy, the same way you'd serve any static site container. More control, but now you're maintaining a server for something GitHub will host for free. Only worth it if you specifically want it inside your own infrastructure.

---

## 7. Promotion — launch checklist

A word first: **automate the build, keep the promotion human.** These communities have finely tuned radar for drive-by self-promotion, and automated posting backfires. Post as a person sharing something you made, engage with replies, and it'll travel.

Good homes for a guide like this, roughly in order of fit:

- **r/selfhosted** and **r/homelab** — the core audience. Lead with the *idea* ("I built a way to let an AI run my homelab safely — here's the design"), not the link. A short write-up in the post body with the link at the end does far better than a bare URL.
- **Hacker News** (`Show HN:`) — title it plainly and specifically. "Show HN: Letting an AI safely operate my homelab — the broker and control plane" reads better than anything with hype words.
- **lobste.rs** — if you have an invite; tag `devops` / `practices`.
- **dev.to / Hashnode** — cross-post the *full text* as an article, but set the **canonical URL** to your site so search engines credit the original and you don't compete with yourself.
- **Your own channels** — a Mastodon/Bluesky/LinkedIn post, any Discord/forum communities you're already part of.

Mechanics that help:
- Post mid-week, mornings US time, for the biggest awake audience.
- Reply to early comments quickly — engagement in the first hour shapes how far a post spreads.
- Crosspost the *same* link rather than re-uploading the content, so all the discussion and link-equity points at one canonical page.
- Don't blast everything at once; space posts over a few days and tailor the intro to each community.

---

## 8. Ongoing: updating the site

Edit `index.md` (the guide) or any file, commit, and push. The Actions workflow rebuilds and redeploys automatically — usually live within a minute or two. To force a rebuild without a content change, use the **workflow_dispatch** trigger: Actions tab → "Deploy site to GitHub Pages" → **Run workflow**.

That's the whole loop: write → commit → push → it's live.
