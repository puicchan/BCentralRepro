# Targets

Approved target technologies for Java modernization. Defines *what is approved* — not implementation steps.

## Target Integration Services

| Service | Use When |
|---------|----------|
| Azure Monitor | Exporting telemetry (traces, metrics, logs) from Java applications |
| OpenTelemetry Collector | Aggregating and forwarding telemetry to OTLP-compatible backends |

## Target Libraries

Source → target mappings for Java.

| Category | Source | Target | Notes |
|----------|--------|--------|-------|
| Authentication | Custom / environment-specific auth | `com.azure:azure-identity` (`DefaultAzureCredentialBuilder`) | Works across local dev, CI/CD, and Azure-hosted environments |
| Observability | Manual instrumentation / custom agents | `opentelemetry-javaagent` | Attach via `-javaagent:opentelemetry-javaagent.jar` at startup |
| Health Endpoints | Custom health checks / none | `spring-boot-starter-actuator` | Exposes `/actuator/health`, `/actuator/health/liveness`, `/actuator/health/readiness` |
| Secret Management | Hardcoded secrets / config-file secrets | `com.azure:azure-security-keyvault-secrets` (`SecretClient`) | Backed by Azure Key Vault with `DefaultAzureCredential` |
| Database ORM | Raw JDBC / custom DAO | JPA (`@Entity`, `EntityManager`) | Improves maintainability, portability, and developer productivity |
| Database Queries | String-concatenated SQL | `PreparedStatement` | Eliminates SQL injection risk and reduces query parsing overhead |
