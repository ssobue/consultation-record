# consultation-record

consultation-record is a POS (problem oriented system) application for analyzing and recording organizational issues in development teams.

The goal is to help facilitators, engineering managers, reviewers, and team members preserve the reasoning process behind organizational diagnosis: observed facts, interpreted problems, hypotheses, interventions, and follow-up outcomes.

Japanese README: [README-ja.md](README-ja.md)

## Current Status

This repository is at the initial design stage. Dependencies, initial domain documentation, and a draft PostgreSQL schema have been added, but the first application vertical slice has not been implemented yet.

## Planned Stack

- Java 25
- Spring Boot 4
- Thymeleaf
- Spring Security
- PostgreSQL 18
- Testcontainers with PostgreSQL 18 for tests
- Gradle

Persistence mapping is still under evaluation:

- MyBatis is being considered for explicit SQL and analysis-oriented queries.
- JPA is being considered for the initial CRUD-heavy workflow and conventional parent-child aggregates.

## Core Domain

The application is organized around these concepts:

- Team: a development team or organizational unit being analyzed
- Consultation Case: a bounded consultation or analysis context for a team
- Problem: a problem-oriented record inside a case
- Observation: a factual note attached to a problem
- Assessment: a hypothesis or interpretation attached to a problem
- Intervention: an action or experiment intended to improve a problem
- Follow-up: an outcome check after an intervention

## Documentation

- [Initial Design](docs/initial-design.md)
- [Database Schema Notes](docs/database-schema.md)
- [Draft PostgreSQL DDL](docs/database-schema.sql)

## Database

The current schema targets PostgreSQL 18.

Initial tables:

- `app_user`
- `team`
- `consultation_case`
- `problem`
- `observation`
- `assessment`
- `intervention`
- `follow_up`

The draft DDL uses identity primary keys, string status fields with `CHECK` constraints, audit timestamps, and explicit foreign-key deletion rules.

## Running Tests

Use the Gradle wrapper:

```sh
./gradlew test
```

Tests use Testcontainers and require Docker.

On macOS with Rancher Desktop, this local environment may need:

```sh
DOCKER_HOST=unix:///Users/ssobue/.rd/docker.sock \
TESTCONTAINERS_DOCKER_SOCKET_OVERRIDE=/var/run/docker.sock \
TESTCONTAINERS_RYUK_DISABLED=true \
./gradlew test
```

## First Implementation Milestone

The first useful vertical slice should include:

1. Team registration
2. Consultation case registration
3. Problem registration
4. Problem detail page
5. Observation registration

This should prove routing, security, database access, validation, templates, and the core domain language before the application expands into assessment, intervention, and follow-up workflows.
