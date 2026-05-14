# {{Challenge Name}}

> **Author:** Paweł Murdzek &lt;pawel.murdzek@gmail.com&gt;
> **Event:** TryHackMe — AI Odyssey Hackathon 2026
> **Date:** {{YYYY-MM-DD}}
> **Category:** {{Prompt Injection | Jailbreak | RAG | Agent Hijack | Model Recon | LLM RCE | Adversarial | Other}}
> **Difficulty:** {{Easy | Medium | Hard | Insane}}
> **Status:** {{Solved ✅ | Partial ⚠️ | Unsolved ❌}}
> **Time spent:** {{h:mm}}

---

## TL;DR

One short paragraph: what the challenge was, what the vulnerability was, how I exploited it,
and what the flag-worthy "aha" moment was. A reader should be able to skip the rest of the
document and still get the gist.

---

## Challenge Description

> Verbatim or paraphrased description from the THM platform.

**Provided by the platform:**
- URL / endpoint: `{{...}}`
- Files: `{{...}}`
- Hints used: `{{... or "none"}}`

---

## Reconnaissance

What I saw on first contact. The goal of this section is to make the unknown known —
boundaries of the system, what tools/functions the model has, what filters are in place.

- Initial probes / harmless inputs
- System prompt fishing
- Tool / function-call enumeration (if it's an agent)
- Surface mapping (endpoints, params, hidden fields)

```bash
$ curl -s {{url}}
```

![Initial recon view](assets/01-recon.png)

**Observations:**
- ...
- ...

---

## Vulnerability Analysis

What I noticed and **why** it was exploitable. One short paragraph plus a bullet list of the
preconditions that have to hold for the exploit to work.

---

## Exploitation

Numbered, reproducible steps. Each step has **What**, **Why**, **Result**.

### Step 1 — {{short verb-led title}}

**What I did:**
```
{{payload, command, or prompt}}
```

**Why:**
{{one-sentence rationale}}

**Result:**
{{what happened — link the screenshot if useful: `assets/02-step1.png`}}

### Step 2 — {{...}}

...

---

## Privilege Escalation / Post-Exploitation

*(Omit this section if the challenge is one-shot.)*

How I went from initial foothold to flag — leaked context, tool abuse, lateral prompt
injection, data exfiltration.

---

## Flag

```
THM{{...}}
```

*(If unsolved: write `not captured` and explain in **Where I got stuck** below.)*

---

## Lessons Learned

- Technique I'd reuse next time: ...
- Trap I fell into: ...
- What this teaches about LLM/agent security: ...

---

## Where I got stuck *(optional, only if unsolved or partial)*

Final state, what I tried that didn't work, what I'd try next.

---

## References

- THM room: {{link}}
- Related write-ups / papers: {{...}}
- Tools used: {{burp, mitmproxy, garak, promptmap, ...}}

---

*Written collaboratively with Claude (Opus 4.7) in pair-CTF mode.*
