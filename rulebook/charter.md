# Charter

## Metadata

| Field | Value |
|-------|-------|
| Rulebook Name | Java Application Modernization Guidelines |
| Version | 1.0 |

## Scope

### Covered Applications and Languages

Java applications are in scope.

### Constraints

Applications must comply with all defined standards to be eligible for production onboarding, platform certification, migration approval, and operational support readiness.

## Principles

- All applications must use Azure-native identity and authentication mechanisms.
- Secrets must never be stored in source code, configuration files, or environment-specific repositories.
- All applications must emit traces, metrics, and logs via OpenTelemetry.
- All applications must expose a health endpoint for operational monitoring and automated recovery.
- Database access must use safe, parameterized query patterns to prevent SQL injection.
- Non-compliant applications may be rejected from production onboarding, platform certification, migration approval, and operational support readiness.
