# Charter

## Metadata

| Field | Value |
|-------|-------|
| Rulebook Name | Java Application Modernization Guidelines |
| Version | 1.0 |
| Changelog | Initial version |

## Scope

### Covered Applications and Languages

Java applications are in scope.

### Application Types

**Included:**
- Java applications requiring modernization to Azure-hosted environments

**Excluded:**
- Applications not targeting Azure

### Constraints

- All modernized Java applications must comply with the identity, observability, health, secret management, and database access standards defined in this rulebook.
- Applications that do not follow these standards may be rejected from production onboarding, platform certification, migration approval, and operational support readiness.

## Modernization Strategy (6R Guidelines)

Of the 6R strategies, this rulebook covers **Rehost**, **Replatform**, and **Refactor** — the three that involve app modernization. Retire, Retain, and Repurchase are outside the scope of app modernization.

Supported strategies: **Rehost** (lift-and-shift, no code changes), **Replatform** (minimal code changes — containerize, adopt managed services), **Refactor** (modify code/architecture — decompose, upgrade).

| Application Type | Default Strategy | Override Conditions |
|------------------|-----------------|---------------------|
| Java application | Replatform | Refactor when authentication, observability, health endpoints, secret management, or database access patterns are non-compliant and require code changes |

## Principles

- Use cloud-native authentication via `DefaultAzureCredential` — no embedded credentials or environment-specific authentication code.
- Manage all secrets centrally via Azure Key Vault; secrets must never reside in source code, configuration files, or environment-specific repositories.
- Instrument all applications with the OpenTelemetry Java agent for automatic tracing, metrics, and log emission.
- Expose health endpoints required for Kubernetes liveness and readiness probes.
- Use JPA and prepared statements for all database access; prohibit SQL string concatenation from user input.
