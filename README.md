# Enterprise Cloud Lakehouse & Governed GenAI Architecture (Azure Databricks + Claude)

## Executive Overview
A production-grade reference architecture and governance blueprint for modernizing legacy enterprise data warehouses onto **Azure Databricks** and **ADLS Gen2**, secured by **Unity Catalog**, and extended to support **Enterprise GenAI / RAG workflows powered by Anthropic Claude**.

Designed from **19+ years of Enterprise Data & Solutions Architecture** experience across **Data Mesh, Data Vault 2.0, Dimensional Modeling, Cloud FinOps, and Regulated Data Governance**.

---

## Reference Architecture Blueprint
![Enterprise Lakehouse & Claude RAG Architecture](docs/architecture.png)

### End-to-End Architectural Layers
1. **Storage Isolation Layer (ADLS Gen2):**
   - Physically isolated storage containers per environment (`dev`, `qa`, `prod`) and business domain.
   - Zero hardcoded keys or SAS tokens—authenticated exclusively via **Azure Databricks Access Connectors** (Managed Identity) and Microsoft Entra ID SCIM sync.
2. **Medallion Lakehouse & Multi-Paradigm Modeling (Delta Lake + Liquid Clustering):**
   - **Bronze (Raw Landing):** Schema-on-read batch and real-time event ingestion (Auto Loader / Event Hubs) with immutable audit trails.
   - **Silver (Enterprise Integration):** Cleansed, conformed entities modeled using **Data Vault 2.0** (Hubs, Links, Satellites) for auditable enterprise history and automated PII tagging.
   - **Gold (Domain Data Products):** Federated **Data Mesh** domain products and **Dimensional (Star/Galaxy)** semantic views optimized with **Liquid Clustering** for sub-second BI and AI consumption.
3. **Centralized Governance & FinOps (Unity Catalog):**
   - Three-level namespace (`domain_prod.schema.object`) with strict workspace bindings preventing cross-environment leakage.
   - Dynamic column masking and row-level security for GDPR, HIPAA, and India DPDP Act compliance.
   - Automated column-level lineage and `system.billing.usage` FinOps chargeback guardrails.
4. **Governed GenAI / Claude RAG Layer:**
   - Unstructured PDFs and supplier contracts governed inside **Unity Catalog Volumes**.
   - Semantic chunking indexed into **Databricks Vector Search** with mandatory Entra ID group ACL metadata.
   - **Anthropic Claude** (via AWS Bedrock / Vertex AI / API) routed through an **AI Gateway** enforcing pre-retrieval ACL filtering, PII redaction, token rate limits, and full audit logging.

---

## Repository Structure

| Folder / File | Purpose |
| :--- | :--- |
| [`docs/architecture.png`](docs/architecture.png) | End-to-end Azure Databricks, Unity Catalog & Claude RAG visual blueprint |
| [`docs/adr-catalog.md`](docs/adr-catalog.md) | Architecture Decision Records (ADRs) detailing key design trade-offs |
| [`governance-sql/01-unity-catalog-setup.sql`](governance-sql/01-unity-catalog-setup.sql) | Domain catalog, ADLS Gen2 external location, Volume & RBAC setup |
| [`governance-sql/02-pii-dynamic-masking.sql`](governance-sql/02-pii-dynamic-masking.sql) | Dynamic PII column masking and row-level security SQL policies |
| [`governance-sql/03-finops-billing-queries.sql`](governance-sql/03-finops-billing-queries.sql) | System table queries for DBU cost attribution & slow-query governance |
| [`genai-rag-blueprint/claude-rag-acl-flow.md`](genai-rag-blueprint/claude-rag-acl-flow.md) | Zero-leakage ACL filtering specification for Anthropic Claude RAG |
| [`genai-rag-blueprint/prompt-guardrails.json`](genai-rag-blueprint/prompt-guardrails.json) | Sample AI Gateway rate-limiting, PII scrubbing & grounding config |
| [`migration-playbook/legacy-edw-to-lakehouse.md`](migration-playbook/legacy-edw-to-lakehouse.md) | Phased playbook for migrating Teradata/Oracle EDWs to Databricks |

---

## Core Architectural Highlights
- **Zero-Trust Data Security:** Combines Unity Catalog ABAC/RBAC with pre-retrieval vector metadata filtering so LLMs never surface unauthorized records.
- **Predictable Cloud FinOps:** Enforces Serverless SQL Warehouses, cluster policies, and automated `system.billing.usage` chargebacks across business domains.
- **Enterprise Modeling Rigor:** Bridges traditional **Data Vault 2.0** auditability and **Star/Galaxy Dimensional** reporting with modern **Delta Lake Liquid Clustering**.
