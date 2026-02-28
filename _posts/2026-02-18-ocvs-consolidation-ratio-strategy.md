---
layout: post
title: "OCVS Consolidation Ratios: How to Model 2:1, 4:1 and 6–8:1 Safely"
date: 2026-02-18
tags: [ocvs, sizing, vmware, architecture, performance]
guide: true
summary: "Deep dive into OCVS CPU consolidation ratios — mission critical (2:1), general workloads (4:1), dev/test (6–8:1), including failure-state modeling and architectural implications."
---

# OCVS Consolidation Ratios: How to Model 2:1, 4:1 and 6–8:1 Safely

## Overview

CPU consolidation ratio is one of the most misunderstood aspects of Oracle Cloud VMware Solution (OCVS) sizing.

Oversubscription is not a fixed number.

It is a workload-class-driven architectural decision balancing:

- Performance
- Cost
- Resiliency
- Risk tolerance

This guide provides a practical framework for applying consolidation ratios safely in enterprise OCVS environments.

---

# Recommended Baseline Ratios

Consolidation ratio must be driven by workload class — never globally applied.

## Mission Critical → 2:1

Use for:

- Databases
- ERP systems
- Financial platforms
- Latency-sensitive workloads
- Systems with strict SLAs

Why:

- Lower CPU Ready %
- Predictable performance
- Stable NUMA alignment
- Better host-failure tolerance

---

## General Production Workloads → 4:1

Use for:

- Application servers
- APIs
- Middleware
- Business platforms

This is the enterprise “sweet spot” for OCVS.

It provides:

- Strong consolidation efficiency
- Controlled CPU Ready levels
- Good balance of cost vs performance

Most production estates should model around 4:1.

---

## Dev/Test → 6:1 – 8:1

Use for:

- Development environments
- QA
- Lab workloads
- Bursty systems

Characteristics:

- High idle time
- No strict SLA
- Acceptable performance variability

However:

Never apply 8:1 blindly.

Always validate against peak utilization windows.

---

# Oversubscription Is Not a Target

Consolidation ratios are not goals.

They are economic tuning levers.

Sizing must always be validated against:

- CPU Ready %
- Co-Stop %
- NUMA boundaries
- Host failure scenarios (N+1)

If CPU Ready exceeds 5–7% consistently, ratio is too aggressive.

---

# Failure-State Modeling (Often Ignored)

The most common mistake in OCVS sizing is modeling ratio only in steady-state.

But clusters must survive host failure.

Example:

4-node cluster  
Modeled at 4:1 consolidation  

If one node fails:

Capacity drops by 25%.

Effective ratio increases:

4:1 → ~5.3:1

This may push CPU Ready into unsafe territory.

Therefore:

Consolidation must be modeled in failure-state, not only steady-state.

---

# Excel Modeling Formula

In sizing worksheets:
