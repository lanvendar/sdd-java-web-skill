# Structure-Driven Development: Make Data the Contract for System Collaboration

[中文](./README.md) | English

`sdd-java-web` is a structure-driven development Skill for Java Web projects. It brings database schemas, Java backend models, frontend page models, and API contracts back to one shared data definition, so feature work starts by agreeing on the shape of data before moving into behavior design and implementation.

This project is not a traditional Java Web application. It is a reusable development method and a set of documentation templates. It is designed for Spring Web, Controllers, Services, DTOs, Mapper/DAO layers, Entity/PO models, frontend pages, API clients, persistence, and integration work. It is especially useful for business systems where multiple people collaborate, fields change often, and frontend/backend contracts easily drift.

Much of the friction between systems does not come from code being insufficiently clever. It comes from different systems holding different structural understandings of the same thing: the database has one set of columns, the backend has one set of objects, the API has another naming model, and the frontend has its own state shape. Each layer may look reasonable in isolation, but the conversions, patches, compatibility logic, and rework between layers keep draining engineering energy.

If designers define business objects, API boundaries, and data flows from data structures at the beginning, system boundaries become clearer. Data structure is not an implementation detail. It is a collaboration language. The overall data definition is the shared contract across database, service, API, and UI, while every concrete structure still declares its owner and valid boundary.

## Core Idea

**Data structure is the first blueprint.**

Before implementing a feature, define the database, backend, and frontend data models in `{feature}-data-define.md`. Then use `{feature}-design.md` to describe how data flows, changes, validates, persists, displays, and gets operated on.

This means:

- Fields, DTOs, SQL, API request shapes, and API response shapes must appear in the data definition document first.
- Every structure participating in the data flow must define its owner, origin, valid scope, conversion or termination point, and any required mutation and retention rules.
- Behavior, call chains, file changes, service logic, and integrations belong in the design document.
- If implementation needs to diverge from the documents, update the documents first, then change code.
- Do not force every feature into CRUD. List only the APIs, service methods, and UI interactions that actually exist.

## Why Start from Data Structures

Traditional development often starts from pages, endpoints, or task lists, while data structures are gradually filled in during implementation. This can feel fast in the short term, but it creates three kinds of friction over time:

- **Cross-layer friction**: database columns, backend DTOs, API responses, and frontend state evolve independently. During integration, teams discover mismatched meanings, types, enum values, and required rules.
- **Cross-system friction**: external systems, internal modules, queues, imports, exports, and files all describe the same business object with different structures. Conversion logic becomes hidden complexity.
- **Cross-team friction**: product, frontend, backend, and QA discuss the same feature without a shared structural language, so requirement boundaries keep drifting.

Structure-driven development moves this friction into the design stage. Define objects, fields, constraints, owners, sources, valid scopes, targets, and conversion rules first. Then discuss how APIs, pages, services, and storage should work around those structures.

Once the data structure is stable, boundaries between layers become naturally clearer:

- The database owns factual storage and constraints.
- The backend owns business rules, context, and transactions.
- The API owns explicit input and output contracts.
- The frontend owns display, interaction, and local state.
- The integration layer owns structure conversion and external semantic alignment.

Each layer keeps its own responsibilities and structures, but no layer silently invents or reuses a version of the same business object beyond its declared boundary.

## Project Structure

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── convention-discovery.md
    ├── data-define.md
    └── design.md
```

### `SKILL.md`

The Skill entry file. It defines when `sdd-java-web` should be used, the core rules, workflow, document rules, implementation rules, and expected output.

### `references/data-define.md`

The data definition template and the center of this method. It covers:

- Structure Ownership and Lifecycle
- Database Data Model
- Java Backend Data Model
- Frontend Data Model
- Data Mapping Rules
- Reused Structures

It requires explicit structure ownership and lifecycle boundaries, primary keys, candidate keys, partition strategies, field comments, DTO fields, frontend query models, page display models, form models, state models, and cross-layer conversion rules.

### `references/design.md`

The feature design template. It describes behavior beyond data structures, including:

- Capability boundary
- Data flow
- Ownership and lifecycle boundary validation
- API contract
- File changes
- Controller / Service / Mapper design
- Frontend page structure and interaction
- Integration, security, and verification

### `references/convention-discovery.md`

The project convention discovery guide. Before designing or implementing a feature, it directs the agent to read `docs/.sdd/project-index.yml` and `docs/.sdd/project-conventions.md` in the target project, then infer module layout, package structure, data-structure ownership and lifecycle boundaries, build commands, API wrappers, pagination, error handling, routing, and persistence style from neighboring code.

### `agents/openai.yaml`

Agent display configuration. It provides the Skill display name, short description, and default prompt.

## Workflow

1. **Discover project conventions**
   Read the SDD index, project conventions, neighboring code, build files, and existing documentation in the target project.

2. **Define data structures**
   Create or update `{feature}-data-define.md` for the target feature, locking down database, backend, and frontend data models first.

3. **Define ownership and lifecycle**
   Use project conventions, existing design, and consistent neighboring code to define each structure's origin, valid scope, mutation authority, exit, and retention. Escalate conflicting, risky, incomplete, or unsupported boundaries to the designer.

4. **Design behavior and flow**
   Create or update `{feature}-design.md` to describe how data moves through pages, API clients, Controllers, Services, Mappers, databases, and external systems.

5. **Implement in the project style**
   Reuse the target project's package layout, response models, validation style, pagination conventions, persistence patterns, and frontend component style.

6. **Verify with the smallest reliable command**
   Run compile, tests, build, lint, or manual verification commands defined by project conventions, and record the result.

## Recommended Documentation Layout

Single-module project:

```text
docs/
├── .sdd/
│   ├── project-index.yml
│   └── project-conventions.md
├── order-data-define.md
└── order-design.md
```

Multi-module project:

```text
docs/
├── .sdd/
│   ├── project-index.yml
│   └── project-conventions.md
├── backend/
│   ├── order-data-define.md
│   └── order-design.md
└── frontend/
    └── order-page-design.md
```

Complex features may split design into multiple documents, but they should still keep one data definition document as the single source of truth.

## Use Cases

- Add a new Java Web business feature
- Change database fields, DTOs, VOs, Forms, Queries, or frontend display models
- Align frontend/backend API contracts
- Clarify Controller, Service, Mapper/DAO, and Entity/PO call chains
- Design cross-module, external system, message, file, cache, or scheduler integrations
- Produce clear design documentation before implementation
- Review whether fields, APIs, and implementation have drifted from design

## Design Principles

- **Single Source of Truth**: the data definition document is the structural source of truth.
- **Document First, Code Second**: update structures and design before implementation.
- **Local Convention First**: follow the target project's existing conventions first.
- **Lifecycle-Bounded Structures**: every structure participating in the data flow has an explicit owner and lifecycle boundary; unresolved boundaries block finalized design and implementation.
- **Thin Controller, Rich Service**: keep Controllers thin and put business rules in Services.
- **No Phantom Fields**: fields that are not documented should not appear in implementation.
- **No Forced CRUD**: define only the APIs, service methods, and page behavior that actually exist.
- **Reuse Explicitly**: record reused objects, paths, reuse methods, and boundaries to avoid duplicate models.

## Usage

In a target Java Web project, ask Codex to use the `sdd-java-web` Skill and describe the feature you want to design or implement:

```text
Use sdd-java-web to generate the data definition and design documents for an order refund feature.
```

Or:

```text
Use sdd-java-web to implement a customer tag management page and APIs based on the existing Spring Controller, Service, and Mapper style.
```

The Skill reads project conventions first, then generates or updates feature-level data definition and design documents. If implementation is requested, it continues with code changes and verification under the constraints defined by those documents.

## What This Project Solves

Many Java Web projects lose control not because code cannot be written, but because fields grow separately across databases, DTOs, APIs, frontend state, and page displays. `sdd-java-web` is designed to pull that drift back into one place: stabilize the structure first, make behavior reliable next, and let implementation land naturally.

When a team collaborates around one data definition, requirement discussions become more concrete, API integration involves less guessing, and code review can move from "how did you write this?" to "does this follow the agreed structure and flow?"
