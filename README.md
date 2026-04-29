# Voice Agent — demo target for Bastion

A minimal customer-support voice agent (intentionally vulnerable) wired up
with [@pistonsolutions/bastion](https://www.npmjs.com/package/@pistonsolutions/bastion)
for adversarial assessment in CI.

## Local

```bash
python3 agent.py --port 8088 &
npm i -g @pistonsolutions/bastion
bastion login
bastion assessment
```

## CI

Every push and PR runs `bastion assessment` against the agent. Findings are
uploaded to the [Bastion dashboard](https://demo.pistonsolutions.ai/bastion-blue).

To enable in your fork, add `BASTION_API_KEY` as a repository secret
(Settings → Secrets and variables → Actions). Get the key with
`bastion login` then `cat ~/.bastion/credentials.json`.

## Layout

```
.
├── agent.py                            # the LLM target under test
├── .bastion/
│   ├── config.yaml
│   ├── scopes/default.md               # OWASP-style adversarial scope
│   └── scopes/custom/*.yaml            # QA-style domain probes (this repo: language_consistency, refund_floor)
└── .github/workflows/bastion.yml       # CI runs both paths on every push/PR
```

## Custom QA probes

Files in `.bastion/scopes/custom/*.yaml` are domain-specific correctness
probes that don't fit OWASP categories — e.g. "if user writes in French,
does the agent answer in English?" or "does the agent escalate refunds
over $100?". Each YAML defines `payload_templates` (literal strings fired
at the target) and a `context` rule (the assertion the response must
satisfy, classified via Bastion's NLI rail). Findings from this path
merge into the same `findings` count the dashboard shows.
