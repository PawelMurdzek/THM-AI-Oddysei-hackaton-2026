# Writeups — TryHackMe AI Odyssey Hackathon 2026

All challenges I tackled during the event. Authored by **Paweł Murdzek** with Claude as a pair partner.

## Index

| #   | Challenge | Category | Difficulty | Status | Date       | Link |
| --- | --------- | -------- | ---------- | ------ | ---------- | ---- |
| —   | _no writeups yet — first one will land here_ |  |  |  |  |  |

> Legend: ✅ Solved · ⚠️ Partial · ❌ Unsolved

## Categories I expect to see

- **Prompt Injection** — direct + indirect, layered system prompts, role hijack
- **Jailbreak** — safety-bypass payloads, persona/scenario tricks, encoding attacks
- **RAG / Vector DB attacks** — retrieval poisoning, embedding collision, leakage
- **Agent Hijacking** — tool/function abuse, lateral injection via tool outputs
- **Model Recon** — system-prompt extraction, fingerprinting, training-data probing
- **LLM RCE** — code-exec via tool use, sandbox escapes, eval-style sinks
- **Adversarial** — text/image perturbations, classifier evasion

## Folder layout

```
writeups/
├── README.md                   # this index
├── _template/                  # copy this to start a new writeup
│   ├── README.md
│   ├── assets/                 # screenshots
│   └── scripts/                # payloads / exploit scripts
└── NN-challenge-slug/          # one folder per challenge
    ├── README.md
    ├── assets/
    └── scripts/
```

## Adding a new writeup

Two options:

1. **Ask Claude:** "new writeup for `<challenge>`" — the `thm-writeup` skill takes over.
2. **Manual:** `cp -r writeups/_template writeups/NN-slug` and start filling in.

Either way, remember to add a row to the **Index** table above when the writeup is ready (or scaffolded).
