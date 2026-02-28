---
layout: post
title: "Other Public Clouds to OCI Native Compute Instances"
date: 2025-09-01
tags: [migration, oci, multicloud, aws, azure, gcp]
guide: true
summary: "Guide to migrating workloads from Azure, GCP, AWS and other public clouds to OCI Native Compute — architectural considerations, networking redesign, and cross-cloud transformation strategy."
---

## Overview

This guide covers migrating workloads from other public cloud providers (Azure, GCP, AWS and others) to OCI Native Compute Instances.

Cloud-to-cloud migration is typically driven by:

- Cost optimization initiatives  
- Multi-cloud consolidation  
- Performance requirements  
- Regulatory constraints  
- Vendor lock-in avoidance  
- Strategic platform alignment  

Unlike VMware-to-VMware migrations, this is a full infrastructure re-alignment exercise.

---

## Introduction

Oracle Cloud Infrastructure (OCI) provides enterprise-grade infrastructure services with:

- Predictable pricing  
- High-performance compute and networking  
- Off-box virtualization security model  
- Flexible compute shapes  

Migrating from Azure, GCP, or AWS to OCI requires:

- Disk format transformation  
- Networking redesign  
- Security policy translation  
- Agent replacement  
- Identity and IAM redesign  

This is not a lift-and-shift — it is a cloud replatforming initiative.

---

# Target Platform: OCI Native Compute Instances

**Key Characteristics:**

- Hypervisor: OCI Compute Service  
- Management: OCI Console / CLI / API  
- Networking: OCI Virtual Cloud Network (VCN)  
- Storage: OCI Block Volumes  
- Shapes: Flexible OCPU/RAM, DenseIO, GPU, HPC, Ampere  
- Best Use Case: Replatformed and cloud-aligned workloads  

---

# Migration Tooling: RackWare

RackWare is a cloud-agnostic mobility platform supporting:

- AWS EC2  
- Azure Virtual Machines  
- GCP Compute Engine  
- VMware  
- Hyper-V  
- KVM  
- Physical servers  

RackWare is typically the primary tool for cloud-to-cloud migrations into OCI Native.

---

## Why RackWare is Effective for Cloud-to-Cloud

- Agentless replication model  
- Delta-based synchronization  
- Multi-wave orchestration  
- Discovery across heterogeneous cloud estates  
- Integrated DR capabilities  
- OCI Marketplace availability  

Cloud providers do not offer native tooling for direct cross-cloud migration to OCI, making third-party automation essential.

---

# Migration Considerations

Cloud-to-cloud migrations are more complex than on-prem migrations because they require redesign rather than replication.

---

## Networking Redesign

Migrating from other public clouds requires:

- IP address remapping  
- VCN architecture design  
- Network Security Groups configuration  
- Internet / NAT Gateway redesign  
- VPN / FastConnect reconfiguration  

Security constructs differ significantly between providers.

For example:

- AWS Security Groups ≠ OCI NSGs  
- Azure VNets ≠ OCI VCNs  

Firewall logic must be revalidated.

---

## Identity & Access

- IAM models differ significantly  
- Cloud-native roles and policies must be redesigned  
- Service principals / managed identities require replacement  
- API permissions must be restructured  

Identity redesign is frequently underestimated.

---

## Storage & Disk Conversion

- AWS EBS → OCI Block Volumes  
- Azure Managed Disks → OCI Block Volumes  
- GCP Persistent Disks → OCI Block Volumes  

Disk format conversion is handled by migration tooling.

Performance tiers must be re-evaluated (Standard, Balanced, High Performance).

---

## Operating System Considerations

- Remove cloud-specific agents (Azure VM Agent, AWS SSM, GCP guest environment)  
- Install OCI agents  
- Adapt cloud-init scripts  
- Replace instance metadata integrations  
- Validate Windows drivers for OCI compatibility  

Cloud-specific automation often breaks post-migration.

---

# Multi-Cloud Architectural Risk Areas

Cloud-native services cannot simply be “moved”.

Examples:

- Managed databases  
- Serverless services  
- Load balancers  
- Object storage integrations  
- Cloud-native IAM integrations  

These require redesign, not migration.

---

# Enterprise & Mission Critical Databases

For high-throughput or large-scale databases:

VM-level replication may not be sufficient.

Consider:

- Oracle DB → Data Guard / GoldenGate  
- PostgreSQL/MySQL → Native replication  
- Active Directory → Replication strategy  
- Exchange → Hybrid / DAG  

Hybrid replication reduces downtime risk.

---

# Decision Guidance

Choose Cloud-to-OCI Native when:

- Consolidating multi-cloud estate  
- Cost reduction is measurable  
- OCI-native integration is strategic  
- Vendor diversification is required  

Avoid this path when:

- Workloads rely heavily on provider-specific PaaS  
- Migration requires significant re-architecture without ROI  

---

# Best Practices

- Perform full dependency mapping before migration  
- Redesign network before replication  
- Pilot representative workloads  
- Benchmark performance post-migration  
- Validate IAM and security policies thoroughly  

---

# Strategic Insight

Cloud-to-cloud migration is rarely about infrastructure alone.

It is usually:

- Financial strategy  
- Risk diversification  
- Governance alignment  
- Performance optimization  

The most successful cloud-to-cloud migrations treat the project as:

Platform realignment — not workload relocation.
