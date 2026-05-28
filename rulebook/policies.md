# Policies

Enforceable standards and hard boundaries for Java modernization. Every policy here is validatable against generated artifacts.

## Security Requirements

### Authentication & Authorization

- Use `com.azure:azure-identity` with `DefaultAzureCredentialBuilder` for all authentication.
- `DefaultAzureCredential` must be used consistently across local development, CI/CD pipelines, and Azure-hosted environments.

### Secrets Management

- All secrets must be stored in and retrieved from Azure Key Vault using `SecretClient`.
- `SecretClientBuilder` must be initialized with `DefaultAzureCredential`.
- Secrets must never be stored in source code, configuration files, or environment-specific repositories.

## Guardrails (Hard Boundaries)

### Prohibited Technologies

| Technology | Approved Alternative |
|-----------|---------------------|
| Hardcoded secrets in source code | Azure Key Vault via `SecretClient` |
| Credentials stored in configuration files | Azure Key Vault via `SecretClient` |
| Connection strings when Azure Entra ID authentication is supported | `DefaultAzureCredential` from `azure-identity` |
| Embedded credentials in JDBC URLs | Azure Key Vault via `SecretClient` + `DefaultAzureCredential` |

### Prohibited Patterns

| Pattern | Approved Alternative |
|---------|---------------------|
| SQL string concatenation using user input | `PreparedStatement` with parameterized queries |
| Unsafe dynamic SQL generation | `PreparedStatement` with parameterized queries |

### Required Elements

Every modernized Java application must include:

#### Authentication

- `com.azure:azure-identity` dependency
- `DefaultAzureCredentialBuilder` used for all credential construction

#### Observability

- `opentelemetry-javaagent` attached at startup via `-javaagent:opentelemetry-javaagent.jar`
- Application must emit traces
- Application must emit metrics
- Application must emit logs when supported
- Telemetry must be exported to Azure Monitor, OpenTelemetry Collector, or an OTLP-compatible backend

#### Health Endpoint

- `spring-boot-starter-actuator` dependency
- `management.endpoints.web.exposure.include=health` configuration property
- `/actuator/health` endpoint exposed
- Recommended: `/actuator/health/liveness` and `/actuator/health/readiness` endpoints exposed

#### Secret Management

- `com.azure:azure-security-keyvault-secrets` dependency
- `SecretClient` used for all secret retrieval

#### Database Access

- JPA (`@Entity`) used for ORM
- `PreparedStatement` used for all parameterized SQL queries

## Validation & Quality Gates

### Pipeline Gates

- Applications that do not comply with all required standards are rejected from:
  - Production onboarding
  - Platform certification
  - Migration approval
  - Operational support readiness

## Coding Style Guidelines

### Java

- Use `@Entity` annotation on all JPA entity classes.
- Use `PreparedStatement` with `?` placeholders for all parameterized SQL queries.
- Never concatenate user input into SQL strings.
