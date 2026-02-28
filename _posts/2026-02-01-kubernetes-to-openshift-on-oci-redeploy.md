---
layout: post
title: "Kubernetes to OpenShift on OCI using Redeployment"
date: 2026-02-1
tags: [migration, openshift, kubernetes, oci]
guide: true
summary: "Guide to migrating generic Kubernetes workloads to OpenShift Container Platform on OCI using a redeployment model — positioning, architecture, and governance considerations."
---

## Overview

This guide covers migrating workloads from generic Kubernetes environments to Red Hat OpenShift Container Platform running on Oracle Cloud Infrastructure (OCI) using a redeployment-based migration model.

This path is appropriate only when OpenShift is the declared enterprise container platform.

If no OpenShift standardization requirement exists, Oracle Kubernetes Engine (OKE) remains the preferred Kubernetes platform on OCI.

---

## Introduction

OCI provides multiple container platform options:

- **Oracle Kubernetes Engine (OKE)** – managed Kubernetes service
- **Red Hat OpenShift on OCI** – customer-managed OpenShift under BYOS

OKE is the default Kubernetes platform on OCI.

OpenShift on OCI exists specifically for organizations that:

- Have existing OpenShift estates  
- Require Red Hat ecosystem alignment  
- Depend on OpenShift-specific features  
- Have OpenShift subscription commitments  

Migrating Kubernetes workloads to OpenShift on OCI is a strategic standardization decision — not a technical necessity.

---

# Target Platform: OpenShift Container Platform on OCI

**Key Characteristics:**

- Platform: Red Hat OpenShift Container Platform  
- Licensing: Bring Your Own Subscription (BYOS)  
- Control Plane: Customer-managed (OCI Compute)  
- Worker Nodes: OCI Compute (VM / Bare Metal / GPU)  
- Networking: OCI VCN-native  
- Ingress: OpenShift Router  
- Storage: OCI Block Volume / File Storage via CSI  
- Best Use Case: Enterprises standardizing on OpenShift  

---

# Strategic Positioning

## When to Choose This Path

This migration approach is appropriate when:

- OpenShift is already deployed on-prem or multi-cloud  
- OpenShift is the declared enterprise standard  
- Red Hat Operators, Pipelines, GitOps are required  
- Subscription costs are already budgeted  

---

## When NOT to Choose This Path

Avoid this path when:

- Source environment is generic Kubernetes  
- Cost optimization is primary objective  
- Platform simplicity is goal  
- No OpenShift subscription exists  

In these cases, Kubernetes → OKE is architecturally cleaner.

---

# Migration Model: Redeployment

Cluster-level migration is not recommended.

The correct approach is **application redeployment**, which means:

- Rebuilding platform services  
- Redeploying workloads  
- Preserving application artifacts  
- Avoiding cluster-state transfer  

This ensures:

- Clean platform alignment  
- Reduced configuration drift  
- Long-term operational stability  

---

# Core Redeployment Components

## Container Images

- Existing images reusable  
- Validate compatibility with OpenShift SCCs  
- Mirror images to OCI Container Registry (OCIR)  
- Validate registry authentication  

OpenShift enforces stricter defaults than vanilla Kubernetes.

---

## Deployment Manifests

Most Kubernetes YAML and Helm charts remain valid.

Adjustments may be required for:

- Security contexts  
- Storage classes  
- Service exposure  
- Ingress → Routes conversion  

OpenShift introduces:

- Routes  
- Templates  
- Operator-based deployment models  

---

## CI/CD Pipelines

Minimal changes required:

- Update kubeconfig  
- Update registry endpoints  
- Validate RBAC permissions  

If using:

- OpenShift Pipelines (Tekton)  
- OpenShift GitOps (Argo CD)  

Existing workflows can typically be reused.

---

# Migration Workflow

## 1. Assessment

- Inventory namespaces  
- Identify OpenShift-specific dependencies  
- Validate subscription coverage  
- Review Operator usage  

---

## 2. Architecture Design (OCI)

Define:

- Control plane placement  
- Worker node sizing  
- Networking segmentation  
- Ingress model  
- Storage classes  

Infrastructure must be designed before redeployment.

---

## 3. Registry Preparation

- Validate image compatibility  
- Mirror to OCIR  
- Enable vulnerability scanning  
- Enforce policy  

---

## 4. Application Redeployment

- Deploy OpenShift platform services first  
- Deploy workloads in waves  
- Validate:

  - Pod admission  
  - Route exposure  
  - Operator functionality  

---

## 5. Cutover Strategy

Common approaches:

- DNS switch  
- Blue/green  
- Parallel run  

Parallel validation is recommended.

---

# Networking Considerations

- OpenShift Routes replace generic Ingress  
- No IP preservation  
- DNS updates required  
- Firewall rules must be translated  

OCI VCN design must align with OpenShift ingress model.

---

# Security & Governance

OpenShift enforces:

- Stricter Security Context Constraints  
- Enhanced RBAC model  
- Cluster-wide governance policies  

Identity provider integration must be validated early.

---

# Observability & Operations

- OpenShift Monitoring Stack redeployed  
- Logging reconfigured  
- Historical metrics not migrated  

Post-migration operational validation is required.

---

# Special Considerations

## Stateful Workloads

Stateful migration requires separate strategy:

- CSI-based migration  
- Backup/restore  
- Application-level replication  

This guide focuses on stateless redeployment.

---

## Operator-Based Applications

Operators must be:

- Reinstalled  
- Reconfigured  
- Revalidated  

Operators are not migrated automatically.

---

# Best Practices

- Only adopt OpenShift if strategically required  
- Avoid mixing OKE and OpenShift without governance model  
- Redeploy platform services cleanly  
- Align subscription procurement early  
- Perform staged migration waves  

---

# Decision Summary

Choose Kubernetes → OpenShift on OCI when:

- OpenShift standardization is mandatory  
- Red Hat tooling is required  
- Governance alignment is strategic  

Otherwise:

Kubernetes → OKE is the cleaner and simpler architecture.

---

# Strategic Insight

Migrating Kubernetes to OpenShift is not a migration necessity.

It is a platform governance decision.

When OpenShift is already embedded in enterprise processes, redeployment:

- Preserves operational consistency  
- Maintains Red Hat supportability  
- Enables hybrid multi-cloud alignment  

If OpenShift is not strategic, adding it increases complexity rather than reducing it.
