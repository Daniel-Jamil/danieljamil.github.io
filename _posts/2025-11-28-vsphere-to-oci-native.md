---
layout: post
title: "VMware vSphere to OCI Native Compute Instances"
date: 2025-11-28
tags: [migration, vmware, oci]
guide: true
summary: "Guide to replatforming VMware workloads into OCI Native Compute using OCM or RackWare — architecture considerations, networking changes, storage conversion and best practices."
---

## Overview

This guide covers migrating workloads from on-premises VMware vSphere environments to OCI Native Compute Instances. In this scenario, VMware virtual machines are replatformed into OCI Native Compute. The process involves VM format conversion, networking adaptation, storage transformation, and integration with OCI-native services.

Unlike lift-and-shift to OCVS, this path introduces architectural changes and is typically aligned with modernization or cost optimization initiatives.

---

## Introduction

Oracle Cloud Infrastructure (OCI) is a global cloud services platform offering a comprehensive portfolio of IaaS, PaaS, SaaS, and DaaS capabilities across distributed datacenters.

OCI Native Compute Instances are designed for replatformed or cloud-native workloads, offering secure, elastic, and high-performance virtual machines provisioned directly within OCI.

This option is ideal for:

- Application modernization
- Integration with OCI-native services
- Cost and performance optimization
- Platform standardization

---

## Target Platform: OCI Native Compute Instances

**Key Characteristics:**

- **Hypervisor:** OCI Compute Service
- **Management Tools:** OCI Console / CLI / API
- **Best Use Case:** Cloud-native apps, replatformed VMs, container platforms
- **Compute Shapes:** Flexible OCPU/RAM; Standard, DenseIO, GPU, HPC, Ampere
- **Networking:** OCI Virtual Cloud Network (VCN)
- **Primary Storage:** OCI Block Volumes
- **Supported OS:** Latest Linux and Windows distributions supported by OCI

---

# Migration Tooling

## Oracle Cloud Migrations (OCM)

Oracle Cloud Migrations is an OCI-native managed service that automates migration of VMware workloads into OCI.

It streamlines discovery, planning, replication, and deployment through the OCI Console, CLI, or API.

### Key Capabilities

**Automated Asset Discovery & Inventory**
- Deploy remote appliance to discover VMware VMs
- Collect metadata and performance metrics
- Store inventory in OCI-hosted repository
- Integrates with OCI Monitoring

**Migration Planning & Execution**
- Assets grouped into Migration Projects
- Service recommends compute shapes and placement
- Cost estimation built into planning phase
- Incremental replication supported

**Secure Access & Governance**
- Native integration with OCI IAM
- Supports compartments, dynamic groups, Vault
- Policy-based access control

**Supported Environments**
- VMware vSphere 6.5 – 8.0  
- vSphere 8.0 requires VDDK 7.0U2  
- Wide range of Linux and Windows guest OS versions

**Best Suited For**
- Low-to-medium complexity VMware estates
- Organizations preferring OCI-native tooling
- Environments requiring governance integration

---

## RackWare

RackWare is a cloud-agnostic workload mobility platform supporting migration, disaster recovery, and backup across heterogeneous environments.

Its core component, RackWare Management Module (RMM), enables policy-driven workload replication and orchestration.

### Key Benefits

- **Broad Compatibility** – VMware, Hyper-V, KVM, physical servers, Kubernetes
- **Agentless & Lightweight**
- **Multi-Use Platform** – Migration + DR + Backup
- **Delta-Based Replication**
- **Wave-Based Orchestration**
- **OCI Marketplace Availability**

### Best Suited For

- Heterogeneous estates
- Multi-cloud consolidation
- Migration + DR combined strategies
- Complex enterprise migrations

---

# Assessment and Discovery Mapping

A structured assessment phase is critical when replatforming into OCI Native.

### Core Activities

- **Workload Discovery & Classification**  
  Use RVTools, OCM discovery, vSphere inventory, or RackWare

- **Dependency Mapping**  
  DNS records, firewall rules, IP dependencies  
  ⚠️ IP remapping is required unless custom networking solutions are used

- **Right-Sizing & Capacity Planning**  
  Define OCI shape selection and storage performance tiers  
  OCM provides automated recommendations

- **Network & Security Planning**  
  Design VCNs, subnets, NSGs, routing tables  
  Plan FastConnect or IPSec VPN

- **Testing & Validation**  
  Conduct pilot migrations before scale execution

---

# Migration Considerations

## Format Conversion

VMware uses VMDK format.  
During migration, disks are converted to OCI-compatible formats automatically by OCM or RackWare.

---

## Networking Changes

Unlike VMware-to-VMware migrations, OCI Native introduces networking changes:

- IP address remapping required
- DNS updates required
- OCI Security Lists / NSGs must be configured
- Default gateway changes to OCI VCN gateway
- Consider load balancers and DRG routing

This is one of the most critical design areas in replatforming projects.

---

## Storage Migration

- VMware storage (VMFS, NFS, vSAN) → OCI Block Volumes
- Adjust disk performance tiers as needed:
  - Standard
  - Balanced
  - High Performance
- Evaluate IOPS requirements before migration

---

## Operating System Considerations

- Validate OS support matrix in OCI
- Remove VMware Tools post-migration
- Install OCI agents (monitoring, management)
- Adjust boot drivers if needed
- Validate licensing (Windows, RHEL, etc.)

---

# Enterprise & Mission Critical Databases

VM-level migration tools (OCM, RackWare) are suitable for many workloads, but not always for:

- Large databases (TB–PB scale)
- High transaction systems
- Near-zero downtime requirements

Consider application-aware replication:

- **Oracle DB** → Data Guard / GoldenGate
- **PostgreSQL/MySQL** → Native replication
- **Active Directory** → Add domain controllers in OCI
- **Exchange** → Hybrid / DAG approach

Hybrid VM-level + DB-level strategy provides better RPO/RTO outcomes.

---

# Best Practices & Strategic Guidance

- Adopt phased migration waves
- Create detailed IP/DNS/firewall mapping tables
- Perform performance baseline comparisons
- Validate backup strategy post-migration
- Modernize where possible using OCI-native services:
  - Autonomous Database
  - Object Storage
  - Load Balancer
  - Monitoring

---

# Strategic Positioning Insight

Choosing OCI Native Compute instead of OCVS represents a strategic shift from infrastructure preservation to platform transformation.

This path introduces architectural change but unlocks long-term optimization potential, tighter OCI integration, and modernization opportunities.

---

