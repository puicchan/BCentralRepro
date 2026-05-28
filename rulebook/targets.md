# Targets

Approved target technologies for Java modernization. Defines *what is approved* — not implementation steps.

## Target Data Services

| Service | Use When |
|---------|----------|
| Azure Key Vault | Storing and retrieving secrets (database passwords, API keys, certificates, connection secrets) |

## Target Integration Services

| Service | Use When |
|---------|----------|
| Azure Monitor | Receiving exported telemetry (traces, metrics, logs) from applications |
| OpenTelemetry Collector | Receiving exported telemetry from applications (OTLP-compatible) |

## Target Libraries

Source → target mappings for Java.

| Category | Source | Target | Notes |
|----------|--------|--------|-------|
| Authentication | Custom / environment-specific auth | `com.azure:azure-identity` (`DefaultAzureCredentialBuilder`) | Required for all Azure-hosted, local dev, and CI/CD environments |
| Observability | Manual instrumentation | `io.opentelemetry.javaagent:opentelemetry-javaagent` | Attach as `-javaagent:opentelemetry-javaagent.jar` at startup |
| Health Monitoring | Custom health endpoints | `org.springframework.boot:spring-boot-starter-actuator` | Expose `/actuator/health`; configure `management.endpoints.web.exposure.include=health` |
| Secret Management | Secrets in config / source code | `com.azure:azure-security-keyvault-secrets` (`SecretClient`) | Use `SecretClientBuilder` with `DefaultAzureCredential` |
| ORM / Database Access | Raw JDBC string concatenation | JPA (`@Entity`) + `PreparedStatement` | Use parameterized queries; no dynamic SQL from user input |
