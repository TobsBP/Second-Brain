---
type: concept
updated: 2026-04-20
appears_in: [books/eng-soft-moderna, subjects/S03-ArqSoft]
---

# Functional vs Non-Functional Requirements

**Functional requirements** describe *what* a system must do — the actions and behaviors visible to users (e.g., "view account balance", "change subscription plan"). They define the feature set.

**Non-functional requirements** describe *how* the system must be — qualities that users don't directly see but strongly feel: performance, security, availability, scalability, maintainability. Failures here often show up as outages or data breaches rather than missing features.

The distinction matters because they drive different design decisions: functional requirements shape domain logic and APIs, while non-functional requirements shape architecture, infrastructure, and operational choices.

## In personal context

Not yet observed in journals.

## In books

- [[wiki/books/eng-soft-moderna/overview]] — introduced in the opening pages as a foundational lens for understanding what a software system needs to satisfy

## Cross-references

- [[wiki/subjects/S03-ArqSoft/overview]] — architectural decisions are largely driven by non-functional requirements (performance, availability, scalability)
