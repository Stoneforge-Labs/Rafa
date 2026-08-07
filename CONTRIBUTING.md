# Contributing to Rafa

Rafa is maintained as a **controlled documentation and implementation baseline**. Contributions should improve clarity, correctness, traceability, or implementation readiness without silently changing approved intent.

## Before making a change

Identify the type of change you are proposing:

| Change class | Examples | Expected handling |
|---|---|---|
| **Editorial** | Spelling, grammar, formatting, broken links, non-operative clarification | Preserve meaning and document identifiers. |
| **Requirement** | New or changed behavior, acceptance criteria, scope, or policy | Explain rationale, impact, affected owners, tests, and traceability. |
| **Architecture** | System boundaries, data ownership, security model, orchestration, provider strategy | Document alternatives, consequences, migration/rollback, and validation. |
| **Policy / legal** | Rights, privacy, employment, retention, distribution, governance | Require the appropriate authorized business/legal owner. |
| **Production emergency** | Immediate risk or incident mitigation | Keep the change minimal, reversible where possible, and follow with a retrospective record. |

## Documentation standards

- Preserve existing **document IDs**, **requirement IDs**, **ADR IDs**, task IDs, and gate IDs.
- Do not turn assumptions, roadmap items, or illustrative scenarios into claims of implemented capability.
- Keep vendor-specific commands, product versions, pricing, performance figures, and legal positions explicitly subject to validation where the source document requires it.
- Use descriptive Markdown headings and tables that render cleanly on GitHub.
- Use fenced code blocks with an appropriate language identifier when adding executable examples.
- Keep diagrams readable in GitHub; Mermaid is preferred for simple architecture and flow diagrams.
- Prefer relative repository links for companion documents.
- Do not include credentials, private keys, access tokens, customer-confidential information, personal data, or restricted production values.

## Traceability

Material changes should answer four questions:

1. **What approved objective or requirement does this change support?**
2. **What downstream documents, systems, owners, or gates are affected?**
3. **What evidence will show that the change is correct?**
4. **How can the change be reversed, superseded, or retired if necessary?**

For implementation changes, link requirements to the relevant repository work, tests, and evidence location whenever those artifacts exist.

## Pull requests

A useful pull request should include:

- a concise summary of the change;
- the change class;
- affected documents or requirement IDs;
- rationale and expected impact;
- validation performed;
- known risks, assumptions, or follow-up work.

Avoid mixing unrelated editorial, requirement, architecture, and policy changes in one pull request when separating them would make review clearer.

## Controlled source principle

Approved source-controlled specifications take precedence over screenshots, chat excerpts, personal notes, and memory. If a proposed change conflicts with an approved requirement or architecture decision, update the governing source explicitly rather than creating an undocumented exception.
