---
name: interrogate-with-docs
description: A relentless interview to sharpen a plan or design, that also produces docs. Combines interrogate's one-question-at-a-time questioning with domain-modeling's glossary and ADR discipline.
disable-model-invocation: true
---

# interrogate-with-docs

Run `interrogate` and `domain-modeling` together in one session, instead of choosing between them.

Ask one question at a time, per `interrogate`: split facts (go research them) from decisions (ask about them), give a recommended answer with every question, follow a decision tree, don't act until a shared understanding is confirmed.

While that interview runs, apply `domain-modeling`'s discipline to what comes out of it: challenge terminology conflicts as they surface, sharpen vague terms into precise ones, cross-reference against the actual code, and write resolved terms into `CONTEXT.md` inline as the conversation reaches them, not batched at the end. Write an ADR only when a decision is hard to reverse, surprising without rationale, and a genuine tradeoff.

Use this instead of plain `interrogate` when the plan or design being sharpened is worth a lasting record: new architecture, a bounded context taking shape, a decision that would confuse someone in six months if it weren't written down.
