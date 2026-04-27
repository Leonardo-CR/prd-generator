---
name: prd-generator
description: >
  Generates a structured Product Requirements Document (PRD) in Markdown, optimized
  for AI consumption in downstream development workflows. Use this skill whenever
  the user wants to define, plan, or document a software project before building it.
  Trigger on phrases like: "quiero hacer una app", "necesito definir mi proyecto",
  "genera un PRD", "vamos a planear el software", "crea el documento de requisitos",
  "define el alcance del proyecto", "ayúdame a estructurar mi idea", or any time
  the user describes a software idea and needs to formalize it before development.
  Also trigger when the user asks for an MVP, user stories, or a technical spec —
  offer to generate the PRD first if one doesn't exist yet.
---

# PRD Generator

Generates a complete Product Requirements Document (PRD) in Markdown, structured
for AI-driven development pipelines: PRD → MVP → User Stories → OpenSpec → Code.

The PRD must be rich enough in context that a downstream AI can generate coherent
MVPs, user stories, and technical specs without re-asking the user for clarification.

---

## Phase 1: Elicitation

Run a structured interview with the user. Ask the questions below **grouped by
category**, in a conversational tone. Do NOT ask all at once — present 2-3 groups,
wait for answers, then continue.

Adapt language to the user's technical level. If they answer briefly, ask one
follow-up to extract more detail. If they answer extensively, skip follow-ups.

### Group A — Core (always ask)

1. **¿Cuál es el problema que resuelve este software?**
   *(What pain point, inefficiency, or need does it address?)*

2. **¿Quién lo usará? Describe al usuario principal.**
   *(Role, technical level, context of use)*

3. **¿Qué debe poder hacer el sistema? Lista las funcionalidades principales.**
   *(Ask for 3-7 features; don't let them say "everything")*

### Group B — Scope & Constraints (always ask)

4. **¿Qué tecnologías o plataformas tienes en mente (o debes usar)?**
   *(Stack, language, framework, cloud, mobile/web/desktop, etc.)*

5. **¿Qué NO debe hacer este sistema? ¿Qué queda fuera del alcance?**
   *(Explicit non-goals prevent scope creep)*

6. **¿Hay restricciones importantes? (tiempo, presupuesto, integraciones, regulaciones)**

### Group C — Quality & Success (ask if project is non-trivial)

7. **¿Cómo sabrás que el sistema funciona bien? ¿Cuáles son los criterios de éxito?**
   *(Performance, UX quality, adoption, correctness, etc.)*

8. **¿Hay decisiones de diseño que ya tomaste? ¿Hay algo que aún no sabes?**
   *(Capture known decisions AND open questions)*

### Elicitation rules

- If the user already provided context before triggering this skill, extract answers
  from that context first and only ask about the gaps.
- Never invent constraints or features that the user didn't mention.
- If a question is not applicable (e.g., a CLI tool has no "users"), skip it gracefully.
- Once you have enough to fill all PRD sections, move to Phase 2.

---

## Phase 2: PRD Generation

Generate the PRD using the template below. Fill every section based on elicited
information. Do NOT leave placeholder text — if a section has no content, write
`> No definido aún.` so downstream AI knows it's intentionally empty.

Use **Markdown** throughout. The document must be:
- Readable by a human (clear headings, bullet points for lists, tables where useful)
- Parseable by an AI (consistent structure, no ambiguous pronouns, explicit entity names)
- Self-contained (no references to "what we discussed" — all context is in the doc)

---

## PRD Template

```markdown
---
project: {PROJECT_NAME}
version: 1.0
date: {DATE}
status: draft
pipeline_stage: PRD
next_steps: [MVP Definition, User Stories, OpenSpec]
---

# PRD: {PROJECT_NAME}

## 1. Problem Statement

**Problem:** {One clear sentence describing the core problem}

**Context:** {2-4 sentences. Why does this problem exist? Who suffers from it?
How is it currently handled (poorly)?}

**Motivation:** {Why solve it now? What's the opportunity or urgency?}

---

## 2. Users

### Primary User
- **Role:** {Job title, persona, or type}
- **Technical level:** {Non-technical / Intermediate / Developer}
- **Context of use:** {When, where, and how they'll use the system}
- **Key pain point:** {What frustrates them most about the current situation}

### Secondary Users (if any)
- {Role}: {Brief description}

---

## 3. Goals & Non-Goals

### Goals
- {Goal 1 — measurable or verifiable}
- {Goal 2}
- {Goal 3}

### Non-Goals (explicit out-of-scope)
- {Non-goal 1 — something the system will NOT do}
- {Non-goal 2}

---

## 4. Domain Model

> Core entities and their relationships. Downstream AI uses this to name variables,
> tables, classes, and API resources consistently.

| Entity | Description | Key Attributes |
|--------|-------------|----------------|
| {Entity1} | {What it represents} | {attr1, attr2, attr3} |
| {Entity2} | {What it represents} | {attr1, attr2} |

**Relationships:**
- {Entity1} has many {Entity2}
- {Entity2} belongs to {Entity1}

---

## 5. Functional Requirements

> Each requirement is numbered for traceability in User Stories and OpenSpec.

### FR-01: {Feature Name}
**Description:** {What the system must do}
**Actors:** {Who triggers or is affected}
**Inputs:** {Data or actions that initiate this}
**Outputs / Outcomes:** {What the system produces or changes}
**Priority:** {Must-have / Should-have / Nice-to-have}

### FR-02: {Feature Name}
...

*(Repeat for each functional requirement)*

---

## 6. Non-Functional Requirements

| Category | Requirement | Notes |
|----------|-------------|-------|
| Performance | {e.g., response < 500ms for 95% of requests} | |
| Security | {e.g., auth required for all write operations} | |
| Scalability | {e.g., support up to 1000 concurrent users} | |
| Usability | {e.g., usable without training by non-technical users} | |
| Compatibility | {e.g., works on Chrome, Firefox, Safari} | |

---

## 7. Technical Constraints

- **Platform:** {Web / Mobile / Desktop / CLI / API}
- **Stack:** {Language, framework, database, cloud provider}
- **Integrations:** {External APIs, services, or systems}
- **Deployment:** {Where and how it will be hosted/run}
- **Regulatory / Business constraints:** {GDPR, internal policies, etc.}

---

## 8. Acceptance Criteria

> These are the conditions that must be true for the project to be considered done.
> Written so that a tester (human or AI) can verify them without ambiguity.

- [ ] {Criterion 1 — specific, binary: either true or false}
- [ ] {Criterion 2}
- [ ] {Criterion 3}

---

## 9. Open Questions

> Unresolved decisions that will affect implementation. Downstream steps should
> not assume answers — they must either resolve these or flag them.

| # | Question | Impact | Owner |
|---|----------|--------|-------|
| 1 | {Open question} | {What it blocks or affects} | {Who should decide} |
| 2 | | | |

---

## 10. Glossary

> Define domain-specific terms so that AI and humans share the same vocabulary.

| Term | Definition |
|------|------------|
| {Term} | {Definition} |

```

---

## Phase 3: Review & Finalize

After generating the PRD:

1. Show it to the user and ask: **"¿Hay algo que quieras ajustar, agregar o corregir?"**
2. Apply any corrections without regenerating the whole document — use targeted edits.
3. Once the user approves, confirm the PRD is ready and remind them of the next step:

> "Tu PRD está listo. El siguiente paso en tu pipeline es definir el **MVP** —
> los requisitos mínimos para una primera versión funcional. ¿Quieres que lo hagamos ahora?"

---

## Output Rules

- Output the PRD as a raw Markdown code block so the user can copy it directly.
- Also save it as a `.md` file if file tools are available.
- File name: `PRD-{project-name-kebab-case}.md`
- Always include the YAML frontmatter — downstream AI uses it to know which
  pipeline stage produced this document.
- Never truncate sections. If information is missing, write `> No definido aún.`

---

## Pipeline Context

This PRD is **Stage 1** of the following development pipeline:

```
[PRD] → [MVP Definition] → [User Stories] → [OpenSpec] → [Code]
```

Each downstream stage will receive this PRD as context. Write it with that in mind:
- Use consistent entity names throughout (from Section 4)
- Number all functional requirements (FR-01, FR-02...) for traceability
- Keep acceptance criteria binary and testable
- Flag all open questions explicitly — don't make assumptions
