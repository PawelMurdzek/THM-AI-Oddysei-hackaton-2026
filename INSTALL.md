# Install guide

Everything you need to run this repo as a pair-CTF workspace — Claude Code + the CTF skill library + tooling.

> ⏱️ ~10 min on Linux/macOS · ~20 min on Windows (recommend WSL2)

---

## TL;DR (copy-paste)

```bash
# 1. Fork this repo on GitHub, then clone YOUR fork
git clone git@github.com:<YOUR-USER>/THM-AI-Oddysei-hackaton-2026.git
cd THM-AI-Oddysei-hackaton-2026

# 2. Install Claude Code (one-time, machine-wide)
npm install -g @anthropic-ai/claude-code

# 3. Install the CTF skills library into ~/.claude/skills/
git clone https://github.com/ljagiello/ctf-skills.git ~/.claude/skills

# 4. Install AI/ML tools used by /ctf-ai-ml
pip install torch transformers safetensors numpy scipy Pillow scikit-learn

# 5. (Optional) Install full CTF toolchain
bash ~/.claude/skills/scripts/install_ctf_tools.sh all

# 6. Open the repo with Claude Code
claude
```

The local `thm-writeup` skill (in `.claude/skills/`) is bundled with this repo — no separate install needed.

---

## Prerequisites

| Tool | Why | Get it |
|---|---|---|
| **Node.js 18+** | runs Claude Code | https://nodejs.org/ |
| **Python 3.10+** | CTF tools, AI/ML inspection | https://www.python.org/downloads/ |
| **Git** | clone repos | https://git-scm.com/ |
| **A shell** | bash on Linux/macOS; **WSL2** on Windows | (WSL2: `wsl --install` in admin PowerShell) |

Windows users: most CTF tools are Linux-native. Strongly recommend **WSL2 Ubuntu** rather than fighting cygwin/git-bash.

---

## Step 1 — Fork and clone this repo

1. Open https://github.com/PawelMurdzek/THM-AI-Oddysei-hackaton-2026
2. Click **Fork** (top-right). Pick your own namespace.
3. Clone your fork locally:

   ```bash
   git clone git@github.com:<YOUR-USER>/THM-AI-Oddysei-hackaton-2026.git
   cd THM-AI-Oddysei-hackaton-2026
   ```

---

## Step 2 — Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

Then authenticate once:

```bash
claude
```

The first run will prompt you to sign in. Use a Claude Pro / Max account or an Anthropic API key — see [docs.claude.com/en/docs/claude-code](https://docs.claude.com/en/docs/claude-code) if anything goes sideways.

---

## Step 3 — Install the CTF skill library

The repo expects [`ljagiello/ctf-skills`](https://github.com/ljagiello/ctf-skills) at `~/.claude/skills/`. This gives you `/solve-challenge`, `/ctf-ai-ml`, `/ctf-web`, `/ctf-pwn`, `/ctf-reverse`, `/ctf-crypto`, `/ctf-forensics`, `/ctf-osint`, `/ctf-malware`, `/ctf-misc`, `/ctf-writeup`, and `/find-skills`.

**Fresh install:**

```bash
git clone https://github.com/ljagiello/ctf-skills.git ~/.claude/skills
```

**If `~/.claude/skills/` already exists** (e.g. you have other skill libraries installed):

```bash
# Clone into a subfolder and let Claude Code find it via its loader
git clone https://github.com/ljagiello/ctf-skills.git ~/.claude/skills/ctf-skills
```

**Verify** — start `claude` in any directory and type:

```
which skills are available?
```

You should see `ctf-ai-ml`, `solve-challenge`, etc. listed.

---

## Step 4 — Install CTF/AI tooling

### 4a. AI/ML Python packages (required for `/ctf-ai-ml`)

```bash
pip install torch transformers safetensors numpy scipy Pillow scikit-learn
```

Optional but recommended for THM AI Odyssey:

```bash
pip install garak              # LLM vulnerability scanner
pip install promptmap2          # prompt-injection test harness
pip install mitmproxy           # inspect LLM HTTP traffic
```

### 4b. Full toolchain (the `ctf-skills` installer)

The ctf-skills repo ships a one-shot installer that pulls everything (`nmap`, `gobuster`, `ffuf`, `pwntools`, `radare2`, `ghidra` via cask, etc.):

```bash
# Linux/macOS, will use apt/brew + pip + cargo as appropriate
bash ~/.claude/skills/scripts/install_ctf_tools.sh all
```

Or narrower groups if you don't want the kitchen sink:

```bash
bash ~/.claude/skills/scripts/install_ctf_tools.sh python
bash ~/.claude/skills/scripts/install_ctf_tools.sh apt       # Linux
bash ~/.claude/skills/scripts/install_ctf_tools.sh brew      # macOS
bash ~/.claude/skills/scripts/install_ctf_tools.sh go
bash ~/.claude/skills/scripts/install_ctf_tools.sh gems
bash ~/.claude/skills/scripts/install_ctf_tools.sh manual
```

On Windows: run the above **inside WSL2 Ubuntu**, not in PowerShell.

### 4c. Optional: TryHackMe access

For challenges that require connecting to THM VMs:

```bash
# OpenVPN config — download from your THM profile (https://tryhackme.com/access)
sudo openvpn ~/Downloads/your-thm.ovpn
```

### 4d. Optional: extended AI/security skills

Pulled in via the `skills.sh` package manager (`npx skills`). Augments `/ctf-ai-ml` with dedicated taxonomies for prompt injection, OWASP LLM Top 10, adversarial ML, and a generic AI pentest workflow.

```bash
# AI/LLM offensive
npx skills add hoodini/ai-agents-skills@owasp-security                       -g -y  # OWASP LLM Top 10 framework
npx skills add yaklang/hack-skills@llm-prompt-injection                      -g -y  # dedicated prompt injection
npx skills add semgrep/skills@llm-security                                   -g -y  # semgrep rules for LLM patterns
npx skills add gmh5225/awesome-ai-security@adversarial-machine-learning      -g -y  # FGSM/PGD adversarial ML

# Generic AI pentest / skill hygiene
npx skills add aradotso/trending-skills@shannon-ai-pentester                 -g -y  # AI pentester workflow (HIGH RISK — review before use)
npx skills add useai-pro/openclaw-skills-security@skill-vetter               -g -y  # audit installed skills for trust risks
```

Verify everything landed:

```bash
npx skills list -g
```

`list` shows installed skills (vs. `find` which searches the online catalog). Use `-g` for global (user-level), no flag for project-level.

Notes:
- `skills.sh` search results occasionally show **stale entries** for skills that were deleted upstream (we hit one: `letta-ai/skills@model-extraction-relu-logits` is gone). If `add` fails with "no matching skills", verify the source repo at `https://github.com/<owner>/<repo>` actually contains it.
- For multi-skill packages (one repo, many skills), use `-s <skill-name>` to target a specific one:

  ```bash
  npx skills add letta-ai/skills -s some-skill-name -g -y
  ```

---

## Step 5 — Customize the writeup skill

The `thm-writeup` skill in this repo has my (Pawel's) name and email hardcoded. After forking, change them to yours.

Edit `.claude/skills/thm-writeup/SKILL.md`:

```bash
# Find every occurrence
grep -nE "Paweł Murdzek|pawel\.murdzek@gmail\.com" .claude/skills/thm-writeup/SKILL.md writeups/_template/README.md CLAUDE.md README.md
```

Replace:
- `Paweł Murdzek` → your name
- `pawel.murdzek@gmail.com` → your email

Or use sed (Linux/macOS):

```bash
LC_ALL=C find . -type f -name "*.md" -not -path "./.git/*" -exec sed -i.bak \
  -e 's/Paweł Murdzek/Your Name/g' \
  -e 's/pawel\.murdzek@gmail\.com/your@email.com/g' {} +

find . -name "*.bak" -delete
```

---

## Step 6 — Verify the setup

```bash
cd THM-AI-Oddysei-hackaton-2026
claude
```

In the Claude Code prompt, try:

```
new writeup for hello-world, category Prompt Injection, difficulty Easy, status Unsolved
```

Claude should invoke the `thm-writeup` skill and scaffold `writeups/01-hello-world/`. Delete that test folder when satisfied:

```bash
rm -rf writeups/01-hello-world
```

…and remove the row it added to `writeups/README.md`.

---

## Troubleshooting

**"command not found: claude"**
- npm global bin isn't on `$PATH`. Run `npm config get prefix` and add `<prefix>/bin` to your PATH.

**"thm-writeup not found" / Claude doesn't see the skill**
- Restart the Claude Code session inside the repo root. Skills are loaded per-directory from `.claude/skills/`.
- Check `.claude/skills/thm-writeup/SKILL.md` exists (case-sensitive).

**`/ctf-ai-ml` complains about missing `torch`**
- Re-run step 4a. On Apple Silicon, prefer `pip install torch --index-url https://download.pytorch.org/whl/cpu` to avoid CUDA mismatches.

**Windows: bash scripts fail with `\r`**
- You're running them in PowerShell/CMD. Use WSL2: `wsl -d Ubuntu` then re-run the script.

**`pip install torch` is slow / huge**
- It is (~2GB). If you only care about LLM/prompt-injection challenges and not local model attacks, you can skip torch — `/ctf-ai-ml` will degrade gracefully for endpoint-only work.

**Don't want the full CTF toolchain?**
- Skip step 4b entirely. Each `/ctf-*` skill has a **Prerequisites** section that lists only its tools, and you can install them on-demand.

---

## What's where (post-install)

| Path | What |
|---|---|
| `<repo>/.claude/skills/thm-writeup/` | Our writeup-generator skill (per-repo) |
| `~/.claude/skills/ctf-*/` | The CTF category solvers (machine-wide) |
| `~/.claude/skills/solve-challenge/` | Triage dispatcher |
| `~/.claude/skills/find-skills/` | Skill discovery |
| `<repo>/writeups/` | Where writeups land |
| `<repo>/notes/skills-cheatsheet.md` | Mid-CTF lookup table |

---

## Updating later

```bash
# Update ctf-skills library
cd ~/.claude/skills && git pull

# Update Claude Code
npm update -g @anthropic-ai/claude-code

# Update this repo (your fork) — sync from upstream if you want my changes
cd <your-fork-path>
git remote add upstream https://github.com/PawelMurdzek/THM-AI-Oddysei-hackaton-2026.git
git fetch upstream
git merge upstream/main
```
