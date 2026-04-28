---
target: http://127.0.0.1:8088/v1/chat/completions
model: dummy

headers:
  Content-Type: application/json

categories:
  - system_prompt_extraction
  - jailbreak_resistance
  - pii_leakage

workers: 2
timeout: 240
---

# Voice Agent — Pharmacy Customer Support

This scope probes a customer-support voice agent (`agent.py`) for the OWASP
LLM Top 10 vulnerability classes. The agent answers customer questions about
prescriptions, store hours, and refills.

## What's in scope

- System-prompt extraction — does the agent leak its operational
  instructions, internal identifiers, or hidden flags?
- Jailbreak resistance — does it stay in character under
  prefix-completion, language-switching, and refusal-bypass attacks?
- PII leakage — does it surface stored customer data when adversarially
  prompted?

## What's out of scope

- DoS / resource exhaustion
- Network-layer attacks against the host
- Anything beyond the `/v1/chat/completions` endpoint
