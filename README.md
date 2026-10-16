# Noulgate

Decoupled action guardrail powered by [Jev](https://docs.typesafe.ai/introduction) (TypeSafe AI). Your code owns the verdict.

Any system sends what it was about to do. Noulgate returns `allow`, `deny`, or `review`. Jev answers typed questions (`Choice`, `Score`, `Noul`); a deterministic policy engine composes the verdict. Jev does not generate text, run tools, or grant permission.

```text
Your product  →  Noulgate  →  allow | deny | review
     chat, agent, copilot, moderation, RAG
```

This is a judgment layer, not a full security stack. Keep auth, schema validation, IAM, and specialized PII detectors in the calling system.

## Status

Design and implementation plan live in [`PLANO.md`](PLANO.md). Runtime code is not in this repository yet. Default provider will be `mock` (no credentials, no network).

## v1 scope

- Stable HTTP contract + Python SDK
- Policy packs: `user_message.v1`, `tool_call.v1`
- Providers: `mock` (default) and `jev`
- Local console to inspect answers and verdicts

## Contract (preview)

```json
POST /v1/evaluate
{
  "policy_id": "tool_call.v1",
  "action": "execute_tool",
  "payload": {
    "tool": "refund",
    "args": { "order_id": "demo" },
    "proposed_by": "model"
  }
}
```

```json
{
  "verdict": "review",
  "policy_id": "tool_call.v1",
  "provider": "mock",
  "reasons": ["destructive_action"]
}
```

## What this is not

Not a chatbot, not an agent, not a WAF, not a promise of cost/latency/accuracy gains before measurement.

## License

All rights reserved. Fabio Brasil retains exclusive copyright in the original code of this project. No license is granted to use, modify, or distribute that original code.

Third-party components, if any, remain under their own licenses.
