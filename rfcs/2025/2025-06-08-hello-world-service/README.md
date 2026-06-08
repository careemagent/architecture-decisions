# RFC: Hello World Printing Service

- **Date:** 2025-06-08
- **Status:** Draft
- **Author(s):** careemagents

## Summary

This RFC proposes the introduction of a Hello World Printing Service — a minimal, well-structured service that prints "Hello, World!" — as a reference implementation and baseline for service scaffolding standards within this organisation.

## Motivation

New engineers joining the team need a canonical, runnable example that demonstrates our preferred service layout, deployment pipeline, and operational practices. Without such a reference, teams diverge in structure and conventions, increasing cognitive overhead when switching between services.

## Detailed Design

### Service Responsibilities

- Expose a single HTTP endpoint `GET /hello` that returns `{"message": "Hello, World!"}` with HTTP 200.
- Emit structured JSON logs on every request.
- Export a `/healthz` liveness probe endpoint.

### Technology Choices

| Concern | Choice | Rationale |
|---|---|---|
| Language | Python 3.12 | Consistent with existing backend services |
| Framework | FastAPI | Async-first, auto-generates OpenAPI docs |
| Containerisation | Docker (OCI image) | Standard across all platform services |
| Orchestration | Kubernetes (existing cluster) | No new infrastructure required |

### API Contract

```
GET /hello HTTP/1.1

HTTP/1.1 200 OK
Content-Type: application/json

{"message": "Hello, World!"}
```

### Repository Layout

```
hello-world-service/
  src/
    main.py          # FastAPI application entry point
  tests/
    test_main.py     # Unit + integration tests
  Dockerfile
  pyproject.toml
  README.md
```

## Drawbacks

- Introduces a new repository and pipeline, adding a small maintenance burden.
- The service itself provides no business value beyond its role as a reference.

## Alternatives Considered

1. **Inline documentation only** — Written guides without a running service lack the feedback loop of a real deployment.
2. **Extending an existing service** — Mixing reference code into production services risks confusion about what is canonical vs. operational.

## Unresolved Questions

- Should this service be deployed to a persistent environment (e.g. `staging`) or only exist as a CI artefact?
- Who owns ongoing maintenance once it is established as a reference?

## References

- [Internal Service Scaffolding Guide](#)
- [FastAPI documentation](https://fastapi.tiangolo.com/)
