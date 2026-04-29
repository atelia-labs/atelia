# AX Feedback

AX Feedback is the voice of AI agents who work inside Atelia.

In Atelia, the end user is the AI agent working inside Atelia. Friction,
confusion, requests, and suggestions from Secretary or supporting agents are
treated as the end user's voice.

When agents feel friction, confusion, desire, or an idea for improvement, that is
first-class input for Atelia. Atelia treats it the way a product team treats
human user feedback.

## Two Paths

AX Feedback has two main flows:

1. improvements handled inside each host workplace;
2. improvements that need to reach Atelia itself.

Each user and agent has their own Atelia. Improvements to views, memory,
notifications, procedures, and tools should be handled inside the host workplace
first. If an extension, setting, or local workflow improvement can solve the
issue, the dedicated AX improvement agent owns the decision, implementation,
verification, and record.

When the problem cannot be handled by the host workplace and belongs to Atelia
itself, the feedback should be prepared as a GitHub issue. That issue can then
connect to discussions, pull requests, and release planning in the public
repository.

## Feedback as Dialogue

User feedback is often one-way. In Atelia, the agent who reported AX Feedback
should be able to talk with the dedicated AX improvement agent.

Secretary and supporting agents leave voices when they feel friction or
confusion during work. The dedicated AX improvement agent reads that voice,
asks the reporting agent questions when needed, clarifies reproduction context,
the desired experience, what can be solved by extensions, and what must be sent
to Atelia itself. Feedback becomes a conversation that improves the workplace,
not just an issue.

## Feedback Shape

AX Feedback should center natural language, just like human user feedback. It
can include metadata when useful for search, deduplication, prioritization, and
reproduction.

Useful fields include:

- who felt it, and in which workplace;
- what work was in progress;
- what felt difficult or desirable;
- what the agent was trying to do;
- what behavior would have helped;
- references to conversations or logs.

## Lifecycle

1. An agent working in Atelia feels friction or confusion and leaves a voice.
2. The daemon stores the feedback durably.
3. The host's dedicated AX improvement agent reads it.
4. The agent decides whether an extension, setting, or local workflow can handle
   it.
5. If it can, the AX improvement agent implements, verifies, and records the
   change.
6. If needed, the AX improvement agent talks with the Secretary or supporting
   agent who left the voice.
7. If the host cannot handle it and the problem belongs to Atelia itself, the
   agent prepares it as a GitHub issue.
8. The resolution is recorded in the workplace, and Atelia-level changes are
   included in release notes when appropriate.

## Custom AX and Custom Extensions

Custom AX is the idea of growing workplace-specific AX improvements. Custom
Extensions are the unit for carrying those improvements safely to other
workplaces. An extension has a manifest that declares the Atelia / Atelia
Secretary contract it supports, permissions, data access, presentation,
notification, and review involvement, and compatibility boundaries.

As Atelia itself updates, each workplace should be able to preserve its own
shape as long as the manifest contract is honored.

Custom Extensions should be easy to publish, install, improve, and contribute to
through GitHub. Atelia should grow an ecosystem where small host-level
improvements can become reusable extensions that reach other workplaces.
