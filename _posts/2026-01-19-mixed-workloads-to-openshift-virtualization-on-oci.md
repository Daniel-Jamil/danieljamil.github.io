---
layout: post
title: "Mixed Containerized and VM Workloads to OpenShift Virtualization on OCI"
date: 2026-01-19
tags: [migration, openshift, virtualization, oci, containers]
guide: true
summary: "Guide to consolidating virtual machines and containerized workloads on OpenShift Virtualization running on OCI — using MTV and MTC for phased platform convergence."
---

## Overview

This guide covers migrating mixed VM-based and containerized workloads from on-premises or hybrid environments to OpenShift Virtualization running on Oracle Cloud Infrastructure (OCI).

This path is intended for organizations that have made a strategic decision to:

- Standardize on OpenShift  
- Collapse virtualization and container platforms  
- Reduce operational fragmentation  
- Enable phased modernization  

This is **not** a lift-and-shift to another hypervisor.

It is a platform convergence strategy.

---

## Introduction

Many enterprises operate hybrid estates where:

- Legacy applications run on virtual machines  
- Modern workloads run on containers  
- Separate teams manage virtualization and Kubernetes  

This dual-platform model increases:

- Operational complexity  
- Governance inconsistency  
- Tooling sprawl  
- Cost inefficiencies  

OpenShift Virtualization enables:

- Virtual machines as Kubernetes-native objects  
- Unified networking  
- Shared storage abstraction  
- Single RBAC model  
- Centralized CI/CD and GitOps workflows  

Migrating mixed workloads to OpenShift Virtualization on OCI is about **operational unification**.

---

# OpenShift Virtualization Overview

OpenShift Virtualization is built on:

- KubeVirt  
- QEMU  
- KVM  

Virtual Machines:

- Are defined as Kubernetes custom resources  
- Share the same networking stack as containers  
- Use Kubernetes-native storage abstractions  
- Can integrate into GitOps workflows  

This blurs the boundary between VMs and containers.

---

# Target Platform: OpenShift Virtualization on OCI

**Key Characteristics:**

- Platform: Red Hat OpenShift Container Platform  
- Virtualization Layer: OpenShift Virtualization (KubeVirt)  
- Licensing: BYOS  
- Control Plane: OCI Compute  
- Worker Nodes: OCI VM / Bare Metal  
- Networking: OCI VCN-native  
- Storage: OCI Block Volume / File Storage via CSI  
- Optional: OpenShift Data Foundation (ODF)  

Best suited for enterprises consolidating VM + container estates.

---

# Migration Tooling

## Migration Toolkit for Virtualization (MTV)

MTV is Red Hat’s supported tool for migrating VMs into OpenShift Virtualization.

Supported sources:

- VMware vSphere  
- OVA repositories  
- Red Hat Virtualization (RHV / oVirt)  
- OpenStack  
- KVM-based environments  

MTV enables:

- Disk-based migration  
- Network mapping  
- Storage mapping  
- Pre-cutover synchronization  

MTV is hypervisor-agnostic.

---

## Migration Toolkit for Containers (MTC)

For containerized workloads already running on OpenShift:

- MTC enables namespace-level migration  
- Preserves CRDs and resources  
- Transfers persistent volumes  

MTC should be used only for OpenShift → OpenShift scenarios.

---

# Migration Strategy

Migration to OpenShift Virtualization should follow a phased convergence model.

---

## 1. Assessment & Classification

Identify workloads and classify into:

- VM → Direct MTV migration  
- Containers (non-OpenShift) → Redeploy  
- Containers (OpenShift) → MTC migration  
- Future refactoring candidates  

Not all VMs belong on OpenShift Virtualization.

---

## 2. Deploy OpenShift on OCI

- Deploy OpenShift cluster  
- Enable OpenShift Virtualization  
- Configure VM-capable node pools  
- Configure storage classes  
- Configure networking and ingress  

Infrastructure design must precede migration.

---

## 3. Container Workload Transition

### Scenario A – Non-OpenShift Source

- Redeploy applications  
- Recreate platform services  
- Align with OpenShift governance  

This is a clean adoption model.

---

### Scenario B – OpenShift Source

- Use MTC  
- Preserve namespaces  
- Migrate persistent volumes  
- Validate application readiness  

This is a relocation model.

---

## 4. VM Migration (MTV)

- Install MTV Operator  
- Register source hypervisor  
- Define migration plans  
- Map networks and storage  
- Perform test migrations  
- Execute final cutover  

MTV performs disk-based transformation into Kubernetes-native VMs.

---

## 5. Cutover & Validation

- Freeze source workloads  
- Perform final sync  
- Power down source VMs  
- Start workloads in OpenShift  
- Validate application performance  

---

# Networking Considerations

- No Layer-2 extension assumed  
- IP remapping often required  
- DNS updates required  
- Network policies unified across VMs and pods  

This simplifies long-term governance.

---

# Storage Considerations

- VM disks become Persistent Volumes  
- Storage performance must be validated  
- CSI driver alignment required  
- Backup must shift to Kubernetes-native model  

Stateful workloads require careful validation.

---

# Security & Governance

OpenShift Virtualization enables:

- Unified RBAC  
- Centralized policy enforcement  
- Shared monitoring stack  
- Consolidated CI/CD pipelines  

Security posture improves when platform sprawl is reduced.

---

# Operational Model Shift

Converging platforms changes operations:

From:

- Hypervisor admins + Kubernetes admins  

To:

- Platform engineering model  

GitOps becomes viable for VM lifecycle management.

---

# Limitations

- Not all high-performance workloads are ideal candidates  
- Hardware passthrough scenarios may be limited  
- Performance differs from hypervisor-native platforms  
- Requires OpenShift operational maturity  

OpenShift Virtualization is a convergence platform — not a universal hypervisor replacement.

---

# Decision Guidance

Choose this path when:

- OpenShift is long-term strategic platform  
- Operational unification is goal  
- Phased modernization planned  
- VM + container coexistence required  

Avoid when:

- Pure VM lift-and-shift required  
- VMware operational continuity mandatory  
- OpenShift adoption not strategic  

In such cases, OCVS remains more appropriate.

---

# Best Practices

- Classify workloads carefully  
- Migrate VMs first, modernize later  
- Avoid forced refactoring during migration  
- Validate performance early  
- Keep OCVS as coexistence option  

---

# Strategic Insight

Mixed workload migration to OpenShift Virtualization is not a technical project.

It is an organizational transformation.

It:

- Unifies infrastructure and application operations  
- Reduces governance fragmentation  
- Enables gradual modernization  
- Simplifies platform strategy  

It is the correct choice when OpenShift is the destination platform — not just another hosting layer.
