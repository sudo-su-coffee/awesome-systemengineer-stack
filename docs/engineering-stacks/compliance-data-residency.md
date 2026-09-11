# 🛡️ Universal SaaS: Compliance & Data Residency Engineering Stack

This document defines the **Legal Shield** of the platform—synthesizing **Physical Data Residency**, **Automated GDPR Deletion Architecture**, and **Cross-Jurisdiction Sovereignty**. It is the blueprint for a "Regulated-by-Design" SaaS that can serve enterprise clients in any jurisdiction (EU, India, US).

---

## 🏛️ 01. Data Residency Mapping (Physical Pinning)

We reject the "One Global Database" model for sensitive data.

- **Jurisdictional Clusters**: The platform is architected to deploy specialized clusters in specific geographical regions (e.g., AWS Mumbai for Indian DPDP, GCP Frankfurt for EU GDPR).
- **Physical Pinning logic**: 
    - When a tenant joins, they are assigned a `data_residency_region`.
    - **T-Level Isolation**: All Tier 6 (Truth) and Tier 10 (Media) data for that tenant is physically routed and stored *only* within the assigned regional storage buckets and DB instances.
- **Master Metadata Hub**: Only non-sensitive, high-level tenant identifiers reside in the global routing layer to facilitate global multi-region login.

---

## 📜 02. The "Right to be Forgotten" (GDPR Autodeletion)

We treat data deletion as a first-class engineering workflow, not a manual SQL script.

- **The Deletion DAG (Directed Acyclic Graph)**: When a "Delete Data" request is triggered:
    1.  **Stage 1: Identity Scrubbing**: Hashing and purging user identifiers from the Auth Tier (T2).
    2.  **Stage 2: Transaction Purge**: Atomic deletion of ledger records from Tier 6.
    3.  **Stage 3: Media Shredding**: Physical deletion of images/files from Tier 10 (MinIO).
    4.  **Stage 4: Analysis Scrubbing**: Hard-deletion of historical events in the Lakehouse (Iceberg).
- **Completion Certificate**: An automated, cryptographic "Proof of Deletion" is generated for the tenant's compliance files.

---

## ⚖️ 03. Cross-Jurisdiction Legal Compliance

We standardize on the world's most rigorous data laws.

- **India DPDP Alignment**: Mandatory storage of primary payment data within Indian borders (integrating with the **Economic Engine/Hyperswitch**).
- **EU / GDPR Standard**: Implementation of **DPA (Data Processing Agreements)** at the platform level, ensuring all sub-processors are vetted and documented.
- **U.S. SOC2 Type II Framework**: Constant automated auditing of internal access. No platform engineer can access raw tenant data without a "Just-in-Time" (JIT) authorized request logged in the Audit Tier (T7).

---

## 🔍 04. Audit Transparency & Immutable Logs

We provide "Nothing-to-Hide" visibility to the tenant.

- **Sovereign Audit Log**: Every login, configuration change, and payment event is logged as an **immutable entry** in a dedicated ClickHouse table.
- **Tenant-Accessible Audit Portal**: Tenants can download their own compliance logs in PDF/CSV format directly from the **4-Side Shell** interface, reducing support overhead.
- **Access Transparency**: If a "System Admin" impersonates or accesses a tenant's dashboard for support, it is logged as a `SUPER_USER_ACCESS` event and presented to the tenant in their audit trail.

---

## 🛡️ 05. The "Privacy-by-Default" API

- **PII Obfuscation**: By default, logs in the **Observability Tier (T7)** do not contain phone numbers or emails. They are replaced by **UUIDs** or **Masked Strings** (e.g., `pap***@gmail.com`).
- **Encrypted-at-Rest-Plus**: Support for **Customer-Managed Keys (CMK)**, where high-tier enterprise clients provide their own RSA keys to encrypt their dedicated data volumes.

---
*Last Updated: April 2026 | Following the Universal SaaS Engineering Standard.*
