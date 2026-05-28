# Modernization Plan: ZavaPayGateway — Modernize and Deploy to Azure

**Project**: ZavaPayGateway

---

## Technical Framework

- **Language**: Java 8
- **Framework**: Apache Struts 1.3.10 (legacy MVC), deployed as WAR on Tomcat 9
- **Build Tool**: Gradle 7.6
- **Database**: Microsoft SQL Server (password-based JDBC via `mssql-jdbc:12.8.1.jre8`)
- **Key Dependencies**: `javax.servlet-api:3.1.0`, `struts-core:1.3.10`, `struts-taglib:1.3.10`

---

## Overview

> This migration modernizes the ZavaPayGateway payment gateway application and deploys it to Azure Container Apps. The application currently runs as a Java 8 / Apache Struts 1.x WAR on Tomcat, using password-based SQL Server authentication and a custom SSO session cookie mechanism. The new architecture will:
>
> - Replace password-based SQL Server credentials with Azure Managed Identity for secure, passwordless database authentication
> - Migrate plaintext database credentials (DB_USER, DB_PASSWORD) from configuration to Azure Key Vault for centralized secret management
> - Migrate console logging for cloud-native log collection on Azure
> - Containerize and deploy the application to Azure Container Apps using the existing Dockerfile
>
> The migration follows a phased approach: first securing credentials and aligning logging, then scanning for CVEs, and finally deploying to Azure Container Apps.

---

## Migration Impact Summary

| Application      | Original Service         | New Azure Service              | Authentication     | Comments                                      |
|------------------|--------------------------|--------------------------------|--------------------|-----------------------------------------------|
| ZavaPayGateway   | SQL Server (password)    | Azure SQL (Managed Identity)   | Managed Identity   | Replace DB_USER/DB_PASSWORD with MI auth      |
| ZavaPayGateway   | Plaintext config secrets | Azure Key Vault                | Managed Identity   | Secure DB credentials and other config values |
| ZavaPayGateway   | File/app logging         | Console logging                | N/A                | Cloud-native logging for Azure Container Apps |
| ZavaPayGateway   | Local container          | Azure Container Apps           | Managed Identity   | Deploy containerized WAR to ACA               |

---

## Open Questions & Questionnaire

- [x] Q: Should the plan include environment/infrastructure provisioning? → A: No — no separate infrastructure provisioning; deployment task will create new resources using Bicep.
- [x] Q: Should the plan include integration testing? → A: No — skip integration testing; focus on code migration and deployment.
- [x] Q: Should the plan include security/CVE remediation? → A: Yes — include security/CVE scan and remediation (default).
- [x] Q: Which Azure deployment target? → A: Azure Container Apps (default) — includes containerization; no separate containerization task needed.
