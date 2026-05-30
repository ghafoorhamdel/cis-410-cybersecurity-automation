# Week 9 Security Audit – cis410-deploy-sa

**Project:** cis410-abh
**Date:** 05/29/2026
**Auditor:** cis410-flask-app

---

## 1. IAM Audit Results

### Before – Week 8 Configuration (Over-Permissioned)

| Role                          | Scope      | Problem                                                    |
| ----------------------------- | ---------- | ---------------------------------------------------------- |
| roles/run.admin               | Project    | Overly broad – allowed full Cloud Run administration       |
| roles/storage.admin           | Project    | Overly broad – granted access to all Cloud Storage buckets |
| roles/artifactregistry.writer | Project    | Acceptable – required for pushing images                   |
| roles/viewer                  | Project    | Acceptable – read-only access                              |
| roles/iam.serviceAccountUser  | Compute SA | Required for deployment                                    |

### After – Week 9 Least Privilege Fix

| Role                          | Scope               | Why Sufficient                                   |
| ----------------------------- | ------------------- | ------------------------------------------------ |
| roles/run.developer           | Project             | Allows deployment without full admin permissions |
| roles/storage.admin           | tfstate bucket only | Reduced access from all buckets to one bucket    |
| roles/artifactregistry.writer | Project             | Needed for image pushes                          |
| roles/viewer                  | Project             | Read-only metadata access                        |
| roles/iam.serviceAccountUser  | Compute SA          | Required for Cloud Run deployments               |

---

## 2. Secret Manager Migration

* Secret created: `flask-app-secret`
* Replication: automatic
* Secret mounted into Cloud Run service
* Secret accessor permission granted to:

  * cis410-deploy-sa
  * Compute Engine default service account

---

## 3. Logging and Monitoring

* Cloud Logging configured for Cloud Run logs
* Log-based alert created: `cis410-flask-app-alert`
* Alert configured for Cloud Run warnings/errors
* Budget created: `cis410-monthly-budget`
* Budget thresholds: 50%, 90%, 100%

---

## Reflection Question 1

Reducing permissions follows least privilege principles because service accounts only receive permissions required for their tasks. This reduces attack surface and lowers the impact of compromised credentials.

## Reflection Question 2

Google Secret Manager is safer than storing secrets directly in GitHub because secrets remain centrally managed, audited, and accessed only when required at runtime.

## Reflection Question 3

Logging, monitoring, alerts, and billing budgets improve operational visibility because administrators can quickly identify failures, abnormal behavior, and unexpected spending before problems become larger incidents.

