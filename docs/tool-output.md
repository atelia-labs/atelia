# Tool Output

Tool output is an AX surface for AI agents. Atelia does not treat tool execution
results as repurposed human logs; it designs them as structured input for AI
work.

## TOON First

Atelia treats TOON as the first agent-facing tool output format.

TOON is Token-Oriented Object Notation. It is a compact representation for
passing structured data to LLMs and can improve token efficiency especially for
object arrays or tabular data with repeated keys.

JSON remains necessary. Secretary should be able to switch between TOON and
JSON from settings. It should also support temporary format overrides and
per-tool default formats.

This is AX customization that lets Secretary and agents shape their own
workplace.

## Schema Design

Tool output design questions every part of the result:

- fields;
- key names;
- field order;
- omission;
- redundancy;
- default values;
- truncation;
- artifact references;
- audit separation;
- language;
- token cost.

Raw observations, audit records, permission use, redactions, and blocked reasons
remain visible. Extensions can customize tool output, while truth stays
unchanged.

## Language

Direct user dialogue respects the user's language.

Agent-facing keys, agent-to-agent communication, and language-independent work
can prefer English for token efficiency and task performance. The choice should
remain adjustable by Secretary preference, tool behavior, and task context.

## Operational Improvement

Atelia treats production operation logs as inputs for AX improvement.

Just as product teams study user behavior to improve UX, Atelia studies
Secretary and agent tool use, format switching, repeated calls, unused fields,
missing fields, token cost, and AX Feedback to improve tool output.

The goal is a better workplace for Secretary and agents.
