# 🛰️ THM AI Odyssey Hackathon 2026 — Writeups

> Personal writeup archive from the **TryHackMe AI Odyssey Hackathon 2026** — an event focused on AI/LLM security: prompt injection, jailbreaks, RAG poisoning, agent hijacking, model recon, LLM RCE, and adversarial inputs.
>
> Authored by **Paweł Murdzek** &lt;`pawel.murdzek@gmail.com`&gt; — solo competitor, paired with **Claude (Opus 4.7)** for note-taking, recon analysis, and writeup drafting.

## About the event

| | |
|---|---|
| **Event** | TryHackMe — AI Odyssey 2026 |
| **Dates** | May 14–17, 2026 |
| **Format** | Global CTF, solo or teams up to 4 |
| **Entry** | Free with a TryHackMe account |
| **Scope** | "From prompt injection to agentic AI" |
| **Prep path** | [TryHackMe AI Security Path](https://tryhackme.com) (run before the CTF) |
| **Named challenges (publicly teased)** | `Vectara`, `Token City`, `Injectus IX`, `Cypheron` |
| **Prizes** | Top-3 teams: cert attempt + 1y Premium + T-shirt; raffle pool for all participants |

My setup: **solo entry**, no team. Co-pilot is Claude (Opus 4.7) — see [CLAUDE.md](./CLAUDE.md) for the rules of engagement.

---

## Table of contents

- [Install](#install)
- [What's in here](#whats-in-here)
- [Repository layout](#repository-layout)
- [How writeups are written](#how-writeups-are-written)
- [Toolkit — skills we use](#toolkit--skills-we-use)
- [Workflow per challenge](#workflow-per-challenge)
- [Progress](#progress)
- [Quick links](#quick-links)
- [Conventions](#conventions)
- [Disclaimer](#disclaimer)

## Install

Forked the repo and want to run it? Full setup (Claude Code + CTF skill library + tooling) is in **[INSTALL.md](./INSTALL.md)**. TL;DR:

```bash
git clone git@github.com:<YOUR-USER>/THM-AI-Oddysei-hackaton-2026.git
npm install -g @anthropic-ai/claude-code
git clone https://github.com/ljagiello/ctf-skills.git ~/.claude/skills
pip install torch transformers safetensors numpy scipy Pillow scikit-learn
cd THM-AI-Oddysei-hackaton-2026 && claude
```

After cloning, **change the author info** in `thm-writeup` to yours — see [INSTALL.md § Step 5](./INSTALL.md#step-5--customize-the-writeup-skill).

---

## What's in here

A clean, per-challenge archive of everything I attacked during the hackathon: the recon I did, the payloads I shipped, the screenshots I took, and the lessons I'm taking forward. Each challenge lives in its own folder under `writeups/` with its assets and exploit scripts kept right next to the writeup itself — no orphan files, no hunting for that one screenshot.

The repo doubles as a **live workspace** during the event — Claude (acting as a pair-CTF partner) has access to the same skills and conventions I do, so we can keep momentum across sessions instead of re-explaining context every time.

## Repository layout

```
.
├── README.md                        # you are here
├── CLAUDE.md                        # rules Claude follows in this repo
├── .gitignore                       # CTF-aware: blocks secrets, models, captures, scratch
├── .claude/
│   └── skills/
│       └── thm-writeup/
│           └── SKILL.md             # our custom writeup-generator skill
├── writeups/
│   ├── README.md                    # index + status of every writeup
│   ├── _template/                   # canonical starting point — copy, don't edit
│   │   ├── README.md                # the writeup template
│   │   ├── assets/                  # screenshots, diagrams
│   │   └── scripts/                 # payloads, exploit code
│   └── NN-challenge-slug/           # one folder per challenge
│       ├── README.md                # the actual writeup
│       ├── assets/
│       └── scripts/
└── notes/
    ├── README.md                    # what goes in this folder
    └── skills-cheatsheet.md         # which slash-command in which phase
```

## How writeups are written

Pair-CTF flow:

1. **I solve** (or get partway), capturing payloads + screenshots as I go.
2. **I tell Claude**: `new writeup for <challenge>` (or paste raw notes + flag).
3. **The `thm-writeup` skill** scaffolds the folder, fills author/date/event, asks only what it can't infer.
4. **I review** — polish prose, drop screenshots into `assets/`, commit when ready.

Every writeup uses the same rigid structure: **TL;DR → Reconnaissance → Vulnerability Analysis → Exploitation (numbered steps, each with What/Why/Result) → PrivEsc → Flag → Lessons Learned**. Consistency is the point — it makes the archive skimmable and entries comparable.

## Toolkit — skills we use

The repo leans heavily on the [`ljagiello/ctf-skills`](https://github.com/ljagiello/ctf-skills) library installed at the user level, plus our local `thm-writeup` skill. Highlights:

| Skill | What it's for |
|---|---|
| **`thm-writeup`** *(local, this repo)* | Scaffold a per-challenge writeup with pre-baked author/event/date and the classic CTF format. |
| **`/solve-challenge`** | Triage entrypoint — give it a challenge file/URL, it picks the right category skill. |
| **`/ctf-ai-ml`** | The main hammer for this event. Bundles `llm-attacks.md`, `model-attacks.md`, `adversarial-ml.md`. |
| **`/ctf-web`** | Web frontend layer that often wraps the LLM. |
| **`/ctf-reverse`, `/ctf-pwn`, `/ctf-crypto`, `/ctf-forensics`, `/ctf-osint`, `/ctf-malware`, `/ctf-misc`** | Specialized category solvers for mixed challenges. |
| **`/find-skills`** | Discovery — search and install new skills from skills.sh. |

Full decision tree and per-skill notes: [`CLAUDE.md`](./CLAUDE.md). Quick mid-CTF lookup: [`notes/skills-cheatsheet.md`](./notes/skills-cheatsheet.md).

## Workflow per challenge

```
   ┌────────────────────────┐
   │ Pick a chal on THM     │
   └─────────┬──────────────┘
             │
             ▼
   ┌────────────────────────┐     unsure of category?
   │ Skim description / URL │ ─────────────────────────► /solve-challenge
   └─────────┬──────────────┘
             │
             ▼
   ┌────────────────────────┐
   │ Recon — fish the system│ ◄────── /ctf-ai-ml (llm-attacks.md)
   │ prompt, map tool surface│
   └─────────┬──────────────┘
             │
             ▼
   ┌────────────────────────┐
   │ Find the vuln          │
   └─────────┬──────────────┘
             │
             ▼
   ┌────────────────────────┐
   │ Build the payload      │ ◄────── capture screenshots → assets/
   │ Iterate until flag     │ ◄────── save payloads → scripts/
   └─────────┬──────────────┘
             │
             ▼
   ┌────────────────────────┐
   │ Flag captured 🏁       │
   └─────────┬──────────────┘
             │
             ▼
   ┌────────────────────────┐
   │ "new writeup for X"    │ ─────────► thm-writeup scaffolds folder
   │ → review, polish, commit│            + updates writeups/README.md
   └────────────────────────┘
```

## Progress

| Metric | Value |
|---|---|
| Challenges attempted | 0 |
| Solved | 0 |
| Partial | 0 |
| Unsolved | 0 |
| Categories covered | — |

Full index: [`writeups/README.md`](./writeups/README.md).

> *This table is updated by hand as the event progresses. Source of truth is the writeups index.*

## Quick links

- 🛠️ [Install guide](./INSTALL.md)
- 📁 [All writeups](./writeups/README.md)
- 🧰 [Writeup template](./writeups/_template/README.md)
- 🤖 [Claude's instructions](./CLAUDE.md)
- ⚙️ [The thm-writeup skill](./.claude/skills/thm-writeup/SKILL.md)
- 📝 [Skills cheatsheet](./notes/skills-cheatsheet.md)
- 📒 [Notes folder](./notes/README.md)

## Conventions

- **Languages:** writeups in **English**; chat between Pawel and Claude in Polish is fine.
- **Slug format:** `NN-kebab-case` (two-digit ordinal + kebab), e.g. `03-rag-poison-via-pdf`.
- **No flags committed** until the event is officially over, unless the room rules explicitly allow it.
- **No secrets in writeups** — API keys, other players' usernames, anything that wasn't part of the intended challenge gets redacted.
- **Screenshots over prose** for visual results — drop them in the writeup's `assets/` folder.
- **Quote the model verbatim** when documenting LLM behavior — paraphrasing kills reproducibility.

## Disclaimer

Everything documented here was performed against the official **TryHackMe AI Odyssey Hackathon 2026** rooms with explicit authorization from the platform. Techniques are for educational and defensive purposes — **do not** point them at systems you don't own or aren't authorized to test.
