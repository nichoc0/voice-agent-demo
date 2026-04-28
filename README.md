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
├── agent.py                      # the LLM target under test
├── .bastion/
│   ├── config.yaml
│   └── scopes/default.md         # what Bastion probes
└── .github/workflows/bastion.yml # CI runs the scope on every push/PR
```
