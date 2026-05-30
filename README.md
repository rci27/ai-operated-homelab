# Building an AI-Operated Homelab

**How do you let an AI help *operate* a server — deploy things, run jobs, fix problems — without handing it the keys to everything?**

There are thousands of guides for self-hosting a media server or setting up a reverse proxy. There are almost none for *that* question. This is one. It documents a working design where an AI assistant can safely run real operations on a home server, through a deny-by-default broker and a human-in-the-loop control plane — and the operating discipline that keeps it from going wrong.

### 📖 [Read the full guide →](https://rci27.github.io/ai-operated-homelab/)

<!-- badges: update the repo path, then these light up automatically -->
![Deploy status](https://github.com/rci27/ai-operated-homelab/actions/workflows/deploy.yml/badge.svg)
![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-5b21b6.svg)
![Made for homelabbers](https://img.shields.io/badge/for-self--hosters-0369a1.svg)

---

## What's actually new here

The *components* are ordinary — Proxmox, Cloudflare Tunnel, Tailscale, [ntfy](https://ntfy.sh), Obsidian, GitHub. A seasoned homelabber will recognize all of them. What's new lives one layer up, in the **control plane**:

- A **broker** that turns "AI with root access" into "AI with a panel of labelled, logged buttons" — deny-by-default, with a one-time phone approval on the dangerous actions.
- A **three-way separation of duties** — a design-and-judgment AI, a human who alone executes, and a local coding AI — where every layer is assumed fallible and checks the others.
- **Discovered patterns** that only make sense once you've hit the problem: routing file transfers through Git because the direct paths kept failing; splitting docs into machine-generated facts vs. human-written reasoning; insisting on evidence over an AI's confident claims.

None of these depend on the specific tools. **The transferable value is conceptual, not procedural** — read it for the way of thinking and discard every command.

## What you'll learn

- A safe model for letting AI act on infrastructure (the broker + the three-way workflow)
- Self-hosting with strong isolation on a single small server (Proxmox + containers)
- Three clean ways to expose services — public (Cloudflare Tunnel), private (Tailscale), local — and how to choose
- Self-hosted push notifications with [ntfy](https://ntfy.sh), including an approval channel
- A Git-backed Obsidian documentation system that doesn't rot
- Four worked examples, from a simple media server to an AI-powered data pipeline

## Who it's for

- Self-hosters and homelabbers who want to do more than chat with an AI about their server
- People interested in **safe AI agency** — practical guardrails, not theory
- Anyone who prefers **big ideas over copy-paste**: every section has a plain-English explanation and a diagram

## A note on the design

This guide describes one coherent way to do it, but the parts worth keeping no matter what tools you pick are: **isolation per service, no open inbound ports, secrets out of anything that syncs, and tested restores.** The AI control plane is the interesting frosting; those four are the cake.

## How this site is published

The site is the guide itself, rendered with Jekyll on GitHub Pages. A GitHub Actions workflow rebuilds and republishes it automatically on every push — see [`PUBLISHING.md`](PUBLISHING.md) for the one-time setup and a launch/promotion checklist. No server of your own is needed to host it.

## License

Documentation is released under [Creative Commons Attribution 4.0 (CC BY 4.0)](LICENSE) — reuse and adapt freely, just credit the source. All hostnames, addresses, and identifiers in the guide are illustrative placeholders; no secrets or credentials are included by design.

---

*Keywords: AI homelab, self-hosted AI operations, Proxmox automation, Claude Code homelab, AI agent guardrails, self-hosted infrastructure, Cloudflare Tunnel, Tailscale, ntfy, Obsidian docs, AI control plane, safe AI infrastructure automation.*
