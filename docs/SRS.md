# PRATIRODH — Software Requirements Specification

**Document status:** Proposed specification; implementation has not started. All requirements below describe intended behavior and are not evidence of delivered functionality.

| Field | Value |
|---|---|
| Product | PRATIRODH (working name) |
| Version | 0.1 — draft for prototype planning |
| Date | 2026-10-17 |
| Status | Planned; documentation-only workspace |

## 1. Purpose

This SRS defines a proposed web prototype for exploring disruption effects across an interconnected district infrastructure network and comparing possible preparedness interventions subject to a budget. It is intended to guide implementation, review, and verification. It does not assert that the model is validated for a real location or operational use.

## 2. Scope

### 2.1 Proposed product

PRATIRODH will provide a browser interface to inspect a network of infrastructure assets and dependencies, choose a documented disruption scenario, view modeled impacts, and compare candidate interventions. A separate API will validate inputs and invoke graph simulation and optimization logic.

### 2.2 Goals

- Make network structure and dependencies inspectable.
- Let a user run reproducible, explicitly parameterized scenarios.
- Summarize modeled service impacts and expose the assumptions behind them.
- Compare intervention portfolios against a finite budget and explain selection results.
- Support sensitivity/uncertainty exploration without presenting outputs as forecasts.

### 2.3 Out of scope for the initial prototype

- Live emergency dispatch, control of infrastructure, or automated operational decisions.
- Real-time feeds, predictive weather, or claims of forecast accuracy.
- Validated engineering, hydraulic, power-flow, traffic, or epidemiological models.
- Production identity management, multi-tenant data governance, or deployment architecture (not yet specified).
- Collection of personal data or confidential infrastructure records.

## 3. Users and operating assumptions

### 3.1 Intended users

- **Analyst/demo user:** selects a supplied network and scenario, runs a simulation, and reviews results.
- **Project maintainer:** curates scenario/network files, documents assumptions, and reviews proposed interventions.

The prototype assumes a desktop or tablet browser and a backend reachable over HTTP. The initial use case is educational or exploratory, using synthetic or appropriately licensed aggregate data. No authentication or authorization model has been chosen.

## 4. Definitions

- **Asset/node:** a facility, service point, or infrastructure component represented in the graph.
- **Dependency/edge:** a directed or undirected relationship whose effect semantics are defined in the network data.
- **Scenario:** a named set of hazard/disruption inputs and assumptions.
- **Intervention:** a candidate action with cost and modeled effect assumptions.
- **Impact:** a modeled change to an asset/service metric compared with a declared baseline.
- **Uncertainty variant:** a documented alternate input or assumption set used for sensitivity comparison.

## 5. Proposed architecture and constraints

The frontend is fixed as Next.js + TypeScript + Tailwind CSS. React Flow is the planned network visualization and Recharts the planned chart library. The backend is Python + FastAPI, using Pydantic for validation, NetworkX for graph modeling, and Google OR-Tools for budget-constrained optimization. The initial persistence proposal is version-controlled JSON scenario and network files.

The frontend and backend will communicate through JSON HTTP APIs. Exact runtime versions, deployment target, database, authentication, and test runner are undecided. This uncertainty must be resolved before implementation of affected features; it must not be represented as a completed setup.

## 6. Functional requirements

All requirements have **Planned** status. Requirement IDs should remain stable as implementation progresses.

### 6.1 Network and scenario discovery

| ID | Requirement | Priority |
|---|---|---|
| FR-001 | The system shall provide a list of available demo networks and scenarios, including stable IDs and human-readable descriptions. | Must |
| FR-002 | The system shall identify the source, version, units, and synthetic/public status of each loaded network and scenario. | Must |
| FR-003 | The system shall reject or clearly report malformed network/scenario data before simulation. | Must |

### 6.2 Network visualization

| ID | Requirement | Priority |
|---|---|---|
| FR-010 | The frontend shall render assets and dependencies in an interactive graph using React Flow. | Must |
| FR-011 | Asset nodes shall expose type, name, baseline status, and available modeled attributes in a detail view. | Must |
| FR-012 | The graph shall distinguish baseline, disrupted, and intervention result states using more than color alone. | Must |
| FR-013 | The user shall be able to pan, zoom, and select graph elements. | Should |
| FR-014 | The graph shall provide a legend and explain edge direction/meaning for the selected network. | Must |

### 6.3 Simulation

| ID | Requirement | Priority |
|---|---|---|
| FR-020 | The user shall be able to select a scenario and review its parameters before running it. | Must |
| FR-021 | The system shall validate scenario parameters against declared ranges, units, and network IDs. | Must |
| FR-022 | The backend shall compute scenario effects using documented deterministic rules for the initial baseline model. | Must |
| FR-023 | The system shall return baseline and post-disruption metrics with a machine-readable run ID and model/scenario version. | Must |
| FR-024 | The system shall display affected assets, affected services, and any configured reachability or service-impact metrics. | Must |
| FR-025 | Results shall disclose assumptions and limitations; they shall not be labelled as forecasts or ground truth. | Must |
| FR-026 | Repeating a run with identical versioned inputs shall yield reproducible results for deterministic simulation. | Must |

### 6.4 Intervention optimization

| ID | Requirement | Priority |
|---|---|---|
| FR-030 | The user shall be able to set a non-negative budget in the declared currency/unit and see the unit. | Must |
| FR-031 | The system shall validate intervention IDs, costs, effects, and eligibility before optimization. | Must |
| FR-032 | The optimizer shall select a feasible intervention set whose total cost does not exceed the budget. | Must |
| FR-033 | The objective function and any weights or constraints shall be disclosed in the result. | Must |
| FR-034 | The result shall show selected and unselected candidates, costs, modeled benefits, and feasibility status. | Must |
| FR-035 | The system shall distinguish infeasible, no-candidate, time-limited, and successful solver outcomes. | Must |
| FR-036 | Optimization output shall be presented as a candidate for human review, with model assumptions visible. | Must |

### 6.5 Comparison and uncertainty

| ID | Requirement | Priority |
|---|---|---|
| FR-040 | The frontend shall compare baseline, disrupted, and intervention outcomes using Recharts or accessible equivalent presentation. | Must |
| FR-041 | Charts shall include labels, units, legends, and a non-visual/color-independent way to interpret values. | Must |
| FR-042 | The user shall be able to compare documented uncertainty variants or sensitivity inputs where available. | Should |
| FR-043 | The system shall state the range/variants evaluated and shall not imply a probability distribution unless one is explicitly defined and justified. | Must |

### 6.6 Errors and provenance

| ID | Requirement | Priority |
|---|---|---|
| FR-050 | API errors shall use a consistent structured response with a safe, actionable message and request correlation identifier. | Must |
| FR-051 | Results shall retain references to network, scenario, intervention, and model versions used to produce them. | Must |
| FR-052 | The UI shall distinguish unavailable, loading, invalid-input, failed, and successful states. | Must |

## 7. Non-functional requirements

All are planned and require verification against a defined demo environment.

| ID | Requirement |
|---|---|
| NFR-001 | **Correctness:** graph and optimization logic shall have unit tests for boundary cases and hand-computable examples. |
| NFR-002 | **Reproducibility:** versioned inputs and deterministic settings shall be sufficient to reproduce deterministic runs. |
| NFR-003 | **Performance:** for an agreed demo network and scenario, an API simulation should complete within 3 seconds at the 95th percentile on the documented development machine; this target must be revisited when workload is defined. |
| NFR-004 | **Optimization runtime:** solver time limits and status shall be explicit; the UI must not present incomplete solver output as optimal. |
| NFR-005 | **Usability:** the principal workflow—select network, select scenario, run, inspect, compare—shall be completable without command-line interaction once the app is running. |
| NFR-006 | **Accessibility:** target WCAG 2.2 AA for core flows, including keyboard access, visible focus, semantic labels, sufficient contrast, and non-color status cues. |
| NFR-007 | **Compatibility:** support current stable desktop versions of Chrome, Firefox, Safari, and Edge at implementation time; exact versions shall be recorded during release. |
| NFR-008 | **Security:** validate inputs at the API boundary, use safe error responses, avoid secrets in the repository, and document deployment-specific controls before any hosted deployment. |
| NFR-009 | **Privacy:** the initial prototype shall not require personal data. Data files shall be checked for sensitive or restricted information before commit. |
| NFR-010 | **Maintainability:** simulation rules, objective functions, units, and assumptions shall be documented and separated from presentation code. |
| NFR-011 | **Observability:** backend logs shall support debugging without logging credentials or unnecessary sensitive input. |
| NFR-012 | **Resilience:** invalid or missing files, API failures, and solver errors shall produce controlled user-facing states rather than silent failure. |

## 8. Data model (proposed)

The exact JSON schema is pending implementation. Fields below define the minimum conceptual contract; units and semantics must be specified in sample data before the corresponding feature is built.

### 8.1 Network

| Field | Type | Meaning |
|---|---|---|
| `id` | string | Stable network identifier |
| `name` | string | Display name |
| `version` | string | Data version |
| `description` | string | Scope and intended interpretation |
| `source` | object | Provenance, license, and synthetic/public designation |
| `units` | object | Metric and cost units used in this network |
| `nodes` | array of Node | Assets/services |
| `edges` | array of Edge | Dependency or connectivity relationships |

### 8.2 Node

`id` (stable string), `type` (controlled category), `name`, `attributes` (typed domain values with units), `baseline_status`, and optional `position` (presentation coordinates, not geographic coordinates unless explicitly declared). Avoid personal data and exact sensitive locations.

### 8.3 Edge

`id`, `source_node_id`, `target_node_id`, `relationship_type`, `directed`, and `attributes`. Relationship semantics must state whether an edge means physical connection, service dependency, access route, or another relation; these meanings must not be conflated.

### 8.4 Scenario

`id`, `name`, `version`, `description`, `network_id`, `hazard_type`, `parameters` (value and unit), `initial_disruptions` (node/edge IDs and effect), `assumptions`, and `source`.

### 8.5 Intervention

`id`, `name`, `description`, `cost` (numeric and currency/unit), `eligible_node_ids` or scope, `effect_assumptions`, `constraints`, and `source`/rationale. An intervention's modeled effect is an assumption unless independently validated.

### 8.6 Simulation run and result

`run_id`, `created_at`, `network_version`, `scenario_version`, `model_version`, `input_parameters`, `baseline_metrics`, `scenario_metrics`, `affected_node_ids`, `affected_edge_ids`, `assumptions`, and `warnings`.

### 8.7 Optimization result

`optimization_id`, `run_id`, `budget` and unit, `objective_definition`, `solver_name`/version, `solver_status`, `selected_intervention_ids`, `total_cost`, `modeled_benefit`, `unselected_candidates`, and `warnings`.

All numeric fields shall define range, precision, and unit. Unknown values shall not be silently coerced to zero.

## 9. Proposed API

Paths and schemas are proposals subject to implementation review. JSON over HTTP; prefix `/api/v1` is proposed. No endpoint exists yet.

| Method and path | Purpose | Proposed response |
|---|---|---|
| `GET /api/v1/health` | Report service availability and API version | status, version |
| `GET /api/v1/networks` | List available networks and provenance summaries | network summaries |
| `GET /api/v1/networks/{network_id}` | Return a network graph and metadata | network object |
| `GET /api/v1/scenarios?network_id={id}` | List scenarios compatible with a network | scenario summaries |
| `GET /api/v1/scenarios/{scenario_id}` | Return scenario details for user review | scenario object |
| `POST /api/v1/simulations` | Validate and run a scenario | run ID, metrics, affected entities, assumptions |
| `GET /api/v1/simulations/{run_id}` | Retrieve a result if run persistence is implemented; otherwise this endpoint may be omitted | simulation result |
| `POST /api/v1/optimizations` | Optimize eligible interventions for a run and budget | solver status, candidate plan, costs, objective, warnings |

### 9.1 API behavior

- Validate IDs, numeric bounds, units, and cross-references before computation.
- Use appropriate HTTP status codes and a structured error envelope, proposed as `{ "error": { "code": "...", "message": "...", "request_id": "..." } }`.
- Never return stack traces or secrets to the client.
- Include model/data version references and assumptions in successful computational responses.
- Enforce request size and solver time limits appropriate to the demo workload.
- Define whether results are ephemeral or persisted before implementing retrieval endpoints.

## 10. Testing and verification strategy

Tooling is planned: Pytest for backend tests; a suitable frontend stack such as Vitest or Jest with React Testing Library, plus Playwright for end-to-end flows. Final choices and commands must be recorded once setup exists.

- **Backend unit tests:** schema validation, graph construction, propagation rules, metric calculations, budget feasibility, objective scoring, solver statuses, empty/disconnected graphs, malformed IDs, and boundary values.
- **API tests:** success/error schemas, invalid payloads, unknown IDs, unavailable data, request limits, and version/provenance fields.
- **Frontend tests:** loading/error/empty states, graph interactions, accessible controls, units and labels, and result interpretation without relying on color alone.
- **End-to-end tests:** select a supplied network and scenario, run the simulation, inspect impacts, submit a budget, and view the optimized candidate plan.
- **Reproducibility checks:** identical deterministic inputs yield equivalent outputs; representative test fixtures have expected results that can be manually verified.
- **Accessibility checks:** keyboard-only workflow, focus order, screen-reader labels, contrast, and chart alternatives.
- **Security/data checks:** no secrets or restricted data in repository; API handles oversized and malformed requests safely.

Tests should not imply empirical validity of the model. Model validation against real infrastructure is outside the initial scope.

## 11. Acceptance criteria

The prototype is acceptable for a documented demo only when all Must requirements are implemented and verified, and the following criteria pass:

1. A clean setup from documented prerequisites starts frontend and backend using commands that have been run successfully.
2. A supplied synthetic network loads with asset types, relationship meanings, provenance, and units visible.
3. A documented scenario can be reviewed and run; invalid parameters are rejected with understandable errors.
4. Simulation outputs match hand-computable fixture expectations and show affected entities, baseline comparison, model/data version, and assumptions.
5. A budget-constrained optimization returns a feasible candidate whose total cost is at most the entered budget, or an explicit non-success solver status.
6. The user can inspect the objective, selected/unselected interventions, costs, modeled benefits, and limitations.
7. Comparison charts provide units and accessible interpretation; important states are not communicated through color alone.
8. Uncertainty/sensitivity displays identify the variants tested and avoid unsupported probabilistic claims.
9. Backend, frontend, and end-to-end test suites pass in the documented environment; test evidence is recorded with the release/demo.
10. Responsible-use boundaries and known limitations are visible in the documentation and relevant result views.

## 12. Risks, open decisions, and dependencies

- **Model validity:** simple graph propagation may oversimplify real systems. Keep assumptions explicit and limit claims.
- **Data sensitivity:** infrastructure details can be sensitive. Begin with synthetic, aggregated examples and review all data before publication.
- **Objective design:** intervention ranking depends on chosen metric, weights, and constraints. Make them visible and reviewable.
- **Uncertainty semantics:** variants are not probabilities unless a defensible distribution is defined. Label sensitivity analysis accordingly.
- **Setup choices:** runtime versions, frontend test runner, persistence, deployment, and authentication remain open.
- **Terminology:** confirm the expansion/meaning of the working name PRATIRODH before public claims if required by the challenge.

## 13. Change control

This SRS is a living specification. Update requirement status and acceptance evidence as the implementation changes. Any change to scope, metric definitions, units, objective function, or data provenance should be reviewed because it can alter how users interpret results. Do not mark a requirement implemented based only on a mockup or documentation change.
