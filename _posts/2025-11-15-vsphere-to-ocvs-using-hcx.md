---
layout: post
title: "VMware vSphere to Oracle Cloud VMware Solution (OCVS) using VMware HCX"
date: 2026-11-15
tags: [migration, vmware, ocvs, oci, hcx]
guide: true
summary: "A practical guide to migrating from on-prem vSphere to OCVS with HCX — methods, constraints, L2 extension, MON, and field-proven best practices."
---

## Overview
This guide covers migrating workloads from on-premises VMware vSphere environments to Oracle Cloud VMware Solution (OCVS) using VMware HCX. This is a seamless lift-and-shift approach that preserves the full vSphere stack (ESXi, vCenter, vSAN, and NSX), minimizing operational disruption by avoiding re-platforming.

## Introduction
Oracle Cloud Infrastructure (OCI) is a global cloud services platform offering a comprehensive portfolio of IaaS, PaaS, SaaS, and DaaS capabilities across distributed datacenters. Oracle Cloud VMware Solution (OCVS) is purpose-built for lift-and-shift migrations of existing VMware environments, preserving the full VMware software-defined datacenter (vSphere, vSAN, NSX, vCenter), enabling organizations to maintain existing tools, processes, and operational models with minimal disruption.

Migrating VMware workloads to OCVS using HCX enables advanced capabilities such as Layer 2 network extension, IP address retention, and live workload mobility.

---

## Target Platform: Oracle Cloud VMware Solution (OCVS)

**Key Characteristics:**
- **Hypervisor:** VMware ESXi
- **Management Tools:** vCenter, HCX, NSX, vSAN
- **Best Use Case:** Lift-and-shift of VMware estates
- **Compute Shapes:** Dedicated Bare Metal DenseIO, Standard and GPU shapes
- **Networking:** NSX
- **Primary Storage:** vSAN or OCI Block Storage
- **Supported OS:** All OS supported by vSphere

---

## VMware HCX: Seamless VMware-to-VMware Mobility

**VMware HCX** is the preferred enterprise-grade mobility solution for vSphere-to-vSphere migrations. Integrated natively with OCVS, HCX enables live and bulk migrations, network extension, and non-disruptive workload transitions.

**Licensing:** HCX is bundled with OCVS deployments. Depending on the chosen Bare Metal shape, customers receive HCX Advanced or HCX Enterprise licenses.

### Migration Methods
- **Bulk Migration:** Parallel batch migration of VMs with scheduled cutovers. (Advanced)
- **vMotion:** Live migration of a single powered-on workload with zero downtime (Advanced)
- **Cold Migration:** Migration of a powered off VM (Advanced)
- **Replication Assisted vMotion (RAV):** Live migration of multiple workloads with minimal/no downtime (Enterprise)
- **OS-Assisted Migration (OSAM):** Guest-based migration of Hyper-V/KVM to OCVS (Enterprise)

**Decision tree**
![Deciding on the correct HCX migration type]({{ '/assets/images/hcx-decision-tree' | relative_url }})

---

## Migration Methods (in depth)

### Bulk Migration
Bulk Migration allows the parallel migration of multiple virtual machines in batches from a source environment to a target environment, such as from on-premises VMware vSphere to Oracle Cloud VMware Solution (OCVS).

Administrators can group VMs into migration waves and schedule cutover times to minimize business disruption.

The migration process involves an initial full synchronisation of the majority of a VM's data in advance, with only the delta changes being synchronized during the cutover window. During the pre-copy phase, HCX will maintain an RPO of 2 hours by default.

This approach is ideal for planned migrations where downtime can be scheduled in advance and large numbers of VMs must be moved efficiently. To the app user it will be seen as a guest OS reboot, as the source VM is powered down and the moved copy is powered on at the destination.

This is the preferred option for migrating VMs, as it allows you to do it at scale. A copy of the VM is left behind on prem, its name is amended to include a POSIX timestamp (this helps with auditing) and it is powered off with its vNIC disconnected from the network (to ensure if it is ever powered on again by accident there is no chance of a conflict on the network).

**Key Considerations:**
- RDMs in Physical Compatibility mode are not supported
- VMs with ISOs attached can't be migrated (HCX can force unmount if needed)
- Snapshots are not migrated
- Multi-Writer/Shared VMDKs are not supported
- VM hardware level must be at least vHW 7

### vMotion Migration
Enables the live migration of powered-on workloads from the source vSphere environment to OCVS without downtime. This is achieved by transferring memory, CPU state, and active network connections over the HCX Interconnect.

**Key Considerations:**
- Requires ~150 Mbps+ throughput (practical minimum)
- Any kind of RDM is not supported
- VM hardware level must be vHW 9+
- Multi-Writer/Shared VMDKs are not supported
- vMotion is serial (one at a time)
- HCX deactivates CBT before migration
- VM Encryption is not supported

### Cold Migration
Cold migration uses the same network path as HCX vMotion to transfer a powered-off virtual machine. During a cold migration, the VM IP address and MAC address are preserved. Cold migrations must satisfy vMotion requirements.

In practice, bulk migration is usually preferred. Cold is useful mainly for edge cases.

**Key Considerations:** Same as vMotion above.

### Replication Assisted vMotion (RAV)
Combines replication and vMotion to support the migration of larger workloads with minimal/no downtime. The majority of the VM's disk data is replicated in advance, followed by a short vMotion event to transfer remaining CPU/memory state.

Just like Bulk Migration there is an initial sync, then HCX maintains an RPO of 2 hours by default.

With RAV, multiple VMs are replicated simultaneously. When the replication phase reaches switchover, a delta vMotion cycle is initiated to perform a quick live cutover. **Live switchover happens serially.**

**Key Considerations:**
- Destination inventory: RAV creates 2 folders (infra + disks). Normal behavior; fix requires Storage vMotion post-migration.
- Requires ~150 Mbps+ throughput (practical minimum)
- Physical Compatibility mode RDMs not supported
- VM hardware level vHW 9+
- HCX deactivates CBT before migration

---

## Networking Capabilities

### Layer 2 Network Extension (L2E)
L2 Network Extension enables seamless extension of Layer 2 broadcast domains from an on-premises datacenter to OCVS. This allows virtual machines to retain their existing IP addresses, avoiding re-IP during migration.

HCX made L2 extension operationally reliable, especially with HA options.

### HCX L2E High Availability (HA)
In a standard HCX L2E setup, an HCX Network Extension appliance is deployed at both the source and destination sites to bridge Layer 2 networks.

HA mode ensures network extension remains operational even if the active appliance fails. This requires HCX Enterprise licensing.

**How it works**
- Active + Standby appliance per extended network
- Heartbeat monitoring
- Automatic failover

**Key Considerations**
- Doubles appliance resources (CPU/RAM/storage)
- Ensure bandwidth headroom for failover
- HA is Enterprise feature set
- Place HA pairs on separate hosts to avoid SPOF
- Some networks/types may not be supported for HA

### HCX Mobility Optimized Networking (MON)
MON ensures VMs migrated with L2 extension can route efficiently after migration — without traffic hairpinning back to the source site.

Without MON, north-south traffic can hairpin to the on-prem gateway, causing latency/bandwidth issues. MON enables a local egress at destination.

**Key Considerations**
- Requires HCX Enterprise
- Enabled per extended network + per VM
- Requires appropriate gateway at destination
- Can increase troubleshooting complexity
- MON has scaling limits (verify per HCX release)

![Mobility Optimized Networking]({{ '/assets/images/hcx-mon.png' | relative_url }})

---

## Assessment and Discovery Mapping
A structured assessment, planning, and testing phase is essential for validating the migration design, minimizing risk, and ensuring a smooth transition to OCVS:

- Workload discovery & classification (RVTools, vSphere inventory, HCX discovery)
- Dependency mapping (DNS, firewall rules, IP requirements)
- Right-sizing & capacity planning (compute, memory, storage)
- Network & security planning (NSX, FastConnect/IPSec, L2E where needed)
- Testing & validation (pilot wave before scale)

---

## Special Considerations for Enterprise and Mission Critical Databases
VM-level migration tools like HCX can handle most workloads, but mission-critical databases often require application-aware replication to achieve near-zero downtime.

Consider:
- **Oracle Databases:** Data Guard / GoldenGate
- **PostgreSQL/MySQL:** native replication / DB tools
- **Active Directory:** native AD replication (additional DCs in OCI/OCVS)
- **Exchange:** Hybrid / DAG approaches

---

## Best Practices & Guidance
- Adopt phased waves (start with non-prod)
- Plan IP/DNS/firewall updates early (even with L2E, document everything)
- Where possible, modernize post-migration using OCI-native services (monitoring, storage, DB services)
---

