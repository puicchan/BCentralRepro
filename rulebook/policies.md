# Policies

Enforceable standards and hard boundaries for Java modernization. Every policy here is validatable against generated artifacts.

## Security Requirements

### Authentication & Authorization

- Use `com.azure:azure-identity` with `DefaultAzureCredentialBuilder` for all Azure authentication.
- Do not use hardcoded secrets or credentials in source code.
- Do not use connection strings when Azure Entra ID authentication is supported.

### Secrets Management

- All secrets must be retrieved from Azure Key Vault using `SecretClient` with `DefaultAzureCredential`.
- Secrets must not be stored in source code, configuration files, or environment-specific repositories.
- Applicable secrets include: database passwords, API keys, certificates, and connection secrets.

## Guardrails (Hard Boundaries)

### Prohibited Technologies

| Technology | Reason | Approved Alternative |
|-----------|--------|---------------------|
| Hardcoded credentials in source code | Security risk; secrets exposed in version control | Azure Key Vault via `SecretClient` |
| Credentials in configuration files | Secrets must be centrally managed and rotatable | Azure Key Vault via `SecretClient` |
| Connection strings when Entra ID is supported | Bypasses cloud-native authentication | `DefaultAzureCredentialBuilder` with Entra ID |
| Embedded credentials in JDBC URLs | Credentials exposed in configuration | Azure Key Vault + `DefaultAzureCredential` |

### Prohibited Patterns

| Pattern | Reason | Approved Alternative |
|---------|--------|---------------------|
| SQL string concatenation using user input | SQL injection risk | `PreparedStatement` with parameterized queries |
| Unsafe dynamic SQL generation | SQL injection risk | `PreparedStatement` with parameterized queries |

### Required Elements

Every modernized Java application must include:

#### Monitoring

- `opentelemetry-javaagent` attached via `-javaagent:opentelemetry-javaagent.jar` at startup.
- Emit traces.
- Emit metrics.
- Emit logs when supported.
- Export telemetry to Azure Monitor, OpenTelemetry Collector, or an OTLP-compatible backend.

#### Health Endpoints

- `spring-boot-starter-actuator` dependency included.
- `management.endpoints.web.exposure.include=health` set in application configuration.
- `/actuator/health` endpoint exposed.
- `/actuator/health/liveness` and `/actuator/health/readiness` endpoints recommended.

#### Authentication

- `azure-identity` library included.
- All Azure service clients authenticated using `DefaultAzureCredentialBuilder`.

#### Secret Management

- `azure-security-keyvault-secrets` library included.
- All secrets retrieved from Azure Key Vault using `SecretClient`.

#### Database Access

- JPA used for ORM (`@Entity` annotation pattern).
- All parameterized queries use `PreparedStatement`.

## Validation & Quality Gates

### Pipeline Gates

- Applications that do not comply with these standards may be rejected from:
  - Production onboarding
  - Platform certification
  - Migration approval
  - Operational support readiness

## Coding Style Guidelines

### Java

- Spring Boot Actuator configuration must set `management.endpoints.web.exposure.include=health`.
- `DefaultAzureCredential` must be instantiated via `new DefaultAzureCredentialBuilder().build()`.
- `SecretClient` must be built using `SecretClientBuilder` with `.vaultUrl()` and `.credential()`.
- JPA entities must use `@Entity` annotation.
- All SQL using user-supplied parameters must use `PreparedStatement` with `?` placeholders.
