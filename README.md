# PRATIRODH

> **Project status: Planned.** PRATIRODH is a proposed decision-support prototype. No application feature should be considered implemented until it has been built and its acceptance criteria have passed. This repository currently contains documentation only.

PRATIRODH (working name) is intended to help users explore how disruptions to interconnected district infrastructure may affect essential services, and compare candidate preparedness interventions under a stated budget. It will model dependencies such as roads, electricity, water, health facilities, and supply depots; simulate explicitly defined disruption scenarios; and explain the assumptions and limits behind results.

The project is intended for a challenge/research prototype using synthetic or openly licensed, appropriately aggregated data. It is not an emergency-response system, operational infrastructure control system, or substitute for expert judgment.

## What is planned

All items below are **planned**, not implemented or validated:

- An interactive infrastructure dependency graph with React Flow.
- Scenario configuration and disruption simulation using NetworkX.
- Impact and recovery summaries, with charts rendered using Recharts.
- Budget-constrained intervention selection using Google OR-Tools.
- An uncertainty view that compares results across documented assumptions or scenario variants.
- A Next.js interface communicating with a Python FastAPI service.

The initial illustrative scenario may represent heavy rainfall and flooding affecting road access and cascading service dependencies. The scenario is a demonstration concept, not a forecast or claim about any real district.

## Proposed architecture

```text
User
  │
  ▼
Next.js + TypeScript + Tailwind CSS
  ├── React Flow: infrastructure graph
  └── Recharts: impacts, comparisons, uncertainty
  │  JSON over HTTP
  ▼
Python + FastAPI
  ├── Pydantic: request and response validation
  ├── NetworkX: dependency graph and scenario simulation
  └── Google OR-Tools: budget-constrained intervention selection
  │
  ▼
Versioned scenario and network data (initial proposal: JSON files)
```

The frontend and backend are proposed as separate components. The API is the boundary for validation and computation. A persistent database, authentication, deployment platform, and production data pipeline have not been selected; they are out of scope until the prototype's needs are clearer.

## Technology decisions

| Area | Agreed choice | Planned role |
|---|---|---|
| Frontend | Next.js + TypeScript | Web application and typed client |
| Styling | Tailwind CSS | Responsive interface |
| Network visualization | React Flow | Interactive infrastructure graph |
| Charts | Recharts | Impact and intervention comparisons |
| Backend | Python + FastAPI | Validated HTTP API and orchestration |
| Graph modeling | NetworkX | Infrastructure dependencies and simulations |
| Optimization | Google OR-Tools | Select interventions under budget constraints |
| Validation | Pydantic | Validate API payloads and domain constraints |
| Initial data format | JSON files | Versioned demo networks and scenarios |
| Backend tests | Pytest | Unit, API, and algorithm behavior |
| Frontend tests | Vitest or Jest, React Testing Library, and Playwright (to be selected during setup) | Component, interaction, and end-to-end behavior |

These choices are fixed for the planned prototype. Versions and exact frontend test runner remain to be selected during implementation.

## Repository layout

```text
.
├── README.md
└── docs/
    └── SRS.md
```

This is the current documentation-only layout. Future `frontend/`, `backend/`, test, and sample-data directories are proposals, not existing components.

## Setup status

There is currently no application code, dependency manifest, sample dataset, or runnable service in this repository. Therefore there are no application setup or run commands yet. Do not install dependencies or expect a local dashboard until the implementation phase adds those files and documents verified commands here.

## Development roadmap

Each stage remains planned until implemented and verified:

1. **Requirements and data contract** — review [the SRS](docs/SRS.md), define a small synthetic network, units, and scoring assumptions.
2. **Backend foundation** — establish Python/FastAPI project, Pydantic schemas, scenario loading, and Pytest checks.
3. **Simulation baseline** — implement deterministic disruption and dependency propagation with documented limitations.
4. **Optimization baseline** — model a small, explainable intervention selection problem with OR-Tools and compare against hand-checkable cases.
5. **Frontend foundation** — establish Next.js, TypeScript, Tailwind, and the agreed visualization libraries; connect to the API.
6. **Integrated prototype** — add network exploration, scenario controls, result comparisons, and uncertainty variants.
7. **Validation and demo readiness** — meet acceptance criteria, document reproducible examples, accessibility, known limitations, and evidence from tests.

## Responsible use and limitations

- Use synthetic, public, or properly licensed data. Do not commit personal, confidential, security-sensitive, or access-controlled infrastructure data.
- Present outputs as exploratory decision support based on explicit assumptions. Do not describe them as predictions, guarantees, or authoritative risk ratings.
- Do not use the prototype to direct emergency response, allocate real-world critical resources, or make high-impact decisions without qualified human review and independent validation.
- Show input provenance, scenario assumptions, units, model version, and uncertainty alongside results. Avoid false precision.
- Treat optimization output as a candidate for review. A mathematically feasible solution may be operationally unsuitable, inequitable, or based on incomplete information.
- Apply accessibility and fairness review to visual encodings and any prioritization criteria; document unresolved issues.

## Documentation

- [Software Requirements Specification](docs/SRS.md) — proposed system scope, requirements, data model, API, verification, and acceptance criteria.

## Contribution and status changes

When implementation begins, update status labels and this README only when there is evidence in the repository. Mark a capability implemented only after its relevant acceptance criteria pass. Keep assumptions and known limitations visible. No license has been selected yet; add a license before inviting reuse or contribution under specific terms.
