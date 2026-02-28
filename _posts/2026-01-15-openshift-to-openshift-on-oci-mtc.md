---
layout: post
title: "OpenShift to OpenShift on OCI using Migration Toolkit for Containers (MTC)"
date: 2026-01-15
tags: [migration, openshift, containers, oci]
guide: true
summary: "Guide to migrating Red Hat OpenShift clusters to OCI using Migration Toolkit for Containers (MTC) — namespace-level migration, persistent volume transfer, and OpenShift-native workload relocation."
---

## Overview

This guide covers migrating containerized workloads from existing Red Hat OpenShift clusters (on-prem or cloud-hosted) to Red Hat OpenShift Container Platform running on Oracle Cloud Infrastructure (OCI) using Migration Toolkit for Containers (MTC).

This approach is designed specifically for:

- OpenShift → OpenShift migrations  
- Namespace-level preservation  
- Persistent volume migration  
- Minimal refactoring strategy  

When both source and target platforms are OpenShift, MTC provides the highest fidelity migration path.

---

## Introduction

Enterprises standardized on Red Hat OpenShift often rely heavily on:

- Operators  
- Custom Resource Definitions (CRDs)  
- Integrated CI/CD pipelines  
- Persistent application storage  
- Platform-level services  

In such environments, full redeployment introduces:

- Operational risk  
- Increased downtime  
- Configuration drift  
- Governance complexity  

Migration Toolkit for Containers (MTC) enables controlled migration of OpenShift-native workloads while preserving application structure and state.

OpenShift on OCI operates under a **Bring Your Own Subscription (BYOS)** model:

- OpenShift licensing via Red Hat  
- Infrastructure via OCI  

---

# Target Platform: OpenShift Container Platform on OCI

**Key Characteristics:**

- Platform: Red Hat OpenShift Container Platform  
- Licensing: BYOS  
- Control Plane: Customer-managed (OCI Compute)  
- Worker Nodes: OCI Compute (VM / Bare Metal / GPU)  
- Networking: OCI VCN-native networking  
- Ingress: OpenShift Router  
- Storage: OCI Block Volume / File Storage via CSI  
- Optional: OpenShift Data Foundation (ODF)  

Best suited for enterprises maintaining OpenShift standardization.

---

# Migration Tooling: Migration Toolkit for Containers (MTC)

Migration Toolkit for Containers is Red Hat’s supported solution for OpenShift-to-OpenShift migration.

It enables:

- Namespace-level migration  
- Resource object transfer  
- Persistent volume migration  
- Selective workload cutover  

MTC uses Velero and CSI-based mechanisms to orchestrate resource and data transfer.

---

## When to Use MTC

Use MTC when:

- Both environments are OpenShift  
- Application structure must be preserved  
- Operators and CRDs are in use  
- Persistent data must be migrated  
- Minimal application refactoring is desired  

---

## When NOT to Use MTC

Do not use MTC when:

- Migrating generic Kubernetes  
- Target platform is OKE  
- Full modernization or platform redesign is planned  
- Application refactoring is strategic objective  

In these cases, redeployment is the preferred approach.

---

# Migration Architecture

MTC architecture includes:

- MTC Operator (installed on target cluster)  
- Velero (backup & restore orchestration)  
- Restic / CSI snapshot integration  
- Migration Controller  

Migration is typically controlled from the target cluster.

This ensures migration governance remains on the destination side.

---

# Migration Workflow

## 1. Assessment & Discovery

- Inventory namespaces  
- Identify Operators in use  
- Validate CRDs  
- Map persistent volumes  
- Verify OpenShift version compatibility  
- Confirm network connectivity  

Version compatibility is critical.

---

## 2. Target Cluster Preparation (OCI)

- Deploy OpenShift cluster on OCI  
- Configure VCN networking  
- Configure storage classes  
- Validate registry access  
- Install MTC Operator  

Typically, the target cluster acts as control cluster.

---

## 3. Source Registration

- Register source cluster  
- Validate authentication  
- Confirm namespace visibility  

Connectivity must be stable and secured.

---

## 4. Migration Planning

Define:

- Namespaces  
- Data transfer method (snapshot or file copy)  
- Pre/post hooks  
- Cutover behavior  

Multiple dry runs are strongly recommended.

---

## 5. Execution

- Initiate migration plan  
- Monitor transfer progress  
- Validate resource creation  
- Address conflicts  

Performance depends heavily on:

- Network throughput  
- Storage performance  
- Data churn rate  

---

## 6. Cutover

- Freeze writes (if required)  
- Execute final sync  
- Validate application functionality  
- Update ingress / DNS  

Cutover planning is critical for stateful workloads.

---

# Persistent Data Considerations

Persistent volume migration depends on:

- Storage class compatibility  
- CSI driver support  
- Data volume size  
- Write intensity  

For large or high-churn databases:

Application-level replication may still be required.

MTC should not replace database-native replication for mission-critical systems.

---

# Networking Considerations

- No Layer-2 extension  
- New IP allocation  
- DNS updates required  
- Network policies must be revalidated  
- Load balancers must be recreated  

OCI networking architecture must be designed before migration begins.

---

# Security & Governance

- RBAC objects migrate where supported  
- SCCs transferred  
- Secrets migrated securely  
- Identity provider integration must be reconfigured  

Cluster-level IAM and external IdP integration must be validated separately.

---

# Observability & Operations

- Monitoring stack redeployed  
- Historical logs and metrics are not migrated  
- Alerting integrations must be recreated  

Operational readiness validation is required before production cutover.

---

# Limitations

- Cluster-scoped components are not migrated  
- Some Operators require manual reinstallation  
- Cross-version migrations must follow Red Hat support matrix  
- Migration performance depends on infrastructure quality  

MTC migrates workloads — not entire cluster state.

---

# Decision Guidance

Choose OpenShift → OpenShift via MTC when:

- OpenShift standardization is strategic  
- Minimal disruption is required  
- Namespace-level migration sufficient  
- Platform architecture remains consistent  

Avoid MTC when modernization is primary objective.

---

# Best Practices

- Perform multiple dry runs  
- Validate storage compatibility early  
- Separate stateful and stateless workloads  
- Plan cutover window carefully  
- Validate security policies post-migration  

---

# Strategic Insight

OpenShift-to-OpenShift migration is not infrastructure migration.

It is platform continuity.

When executed correctly, it:

- Preserves governance  
- Maintains operational model  
- Reduces refactoring risk  
- Maintains Red Hat supportability  

MTC should be used as a precision tool — not a generic migration hammer.
