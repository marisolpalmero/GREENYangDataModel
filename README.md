# Exercise Comparing EMAN, POWEFF, and GREEN Work Items – v00 (Working Document)

**IETF GREEN Working Group**  
**Date:** November 2025  
**Status:** Working draft for WG alignment  

---

## 1. Introduction and Rationale

The IETF **Energy Management (EMAN)** work established a strong foundation for monitoring and managing the energy use of networked devices (RFC 7460, RFC 7461).  
However, EMAN’s SNMP MIB-based approach limits its scalability, extensibility, and integration within cloud-native or telemetry-driven environments.

As networks and data centers evolve toward **energy transparency**, **sustainability**, and **NetZero** targets, a new generation of models is required.  
These models must combine real-time telemetry, metadata on provenance and accuracy, and lifecycle insights into an interoperable, machine-readable framework.

The **POWEFF** draft introduced a modular concept — distinguishing **embedded**, **monitored**, and **derived** metrics — representing a natural evolution of EMAN’s ideas.  
The **IETF GREEN WG** builds on both to define **YANG data models** that enable **real-time, vendor-neutral telemetry**, **energy-aware control**, and **reporting** across devices, components, controllers, and aggregators, from device-level telemetry to system-wide reporting and control.

The goal is not only to extend and modernize the **Energy Management (EMAN)** architecture, aligning it with the a more modular approach and the **GREEN WG Use Cases (draft-stephan-green-use-cases-03)**.

---

## 2. Background and Alignment

| Standard / Draft | Description | Key Limitation or Gap |
|------------------|--------------|------------------------|
| **EMAN (RFC 7460–7461)** | Defines MIBs for energy objects, contexts, and power states via SNMP. | Device-centric; no support for streaming telemetry, provenance, or lifecycle data. |
| **POWEFF (draft-opsawg-poweff)** | Introduces modular energy telemetry (embedded / monitored / derived) and collection methods. | Conceptual structure only; no control RPCs. |
| **GREEN Framework (draft-belmq-green-framework)** | Provides the architecture for energy aware and control as well as data exchange and energy observability. | Requires binding to specific YANG modules for practical implementation. |

---

## 3. Gap Analysis: EMAN vs POWEFF vs GREEN YANG Data Model vs Generic Management Framework

### 3.1. Addressed by EMAN
- Power state modeling (idle, on, off, sleep)  
- Device energy metering and identity  
- Hierarchical relationships (device → component)  

### 3.2. Addressed by POWEFF
- Modular approach for telemetry granularity  
- Recognition of datasheet vs live vs derived metrics  
- Hierarchical relationships (device → component)  

### 3.3. Gaps Identified for GREEN WG YANG
- Common schema for interoperability between modules
- Data provenance and metric trustworthiness 
- Real-time telemetry via YANG Push and gNMI  
- Integration of contextual factors (temperature, load, traffic)  
- Lifecycle and energy aware policies (control + audit)

---

## 4. Proposed YANG Architecture Framework

The **YANG-based Green Energy Management Framework** defines a layered model where metrics evolve from static (datasheet) to live (monitored) to computed (derived) to actionable (policy), with standardized reporting (audit).

| Layer | Description | Source | YANG Module |
|-------|--------------|---------|--------------|
| **Datasheet (Vendor)** | Rated specifications and nominal efficiency data. | Manufacturer | `ietf-energy-capability` |
| **Device (Monitored)** | Real-time operational telemetry. | Device sensors, agents | `ietf-energy-core` |
| **Controller (Aggregator)** | Consolidates monitored data, adds derived metrics. | Local / regional controller | `ietf-energy-derived` |
| **Aggregator / Manager** | Fleet or site-level consolidation. | Multi-node systems | `ietf-energy-collector` |
| **Reporter / Auditor** | Policy, lifecycle, and ESG reporting. | Enterprise system | `ietf-energy-audit`, `ietf-energy-policy` |

This layered model ensures vertical consistency — from hardware to energy aware dashboards — while enabling modular implementation.

---

## 5. Alignment with GREEN WG Use Cases

| Use Case (from draft-stephan-green-use-cases) | Required YANG Evolution |
|-----------------------------------------------|--------------------------|
| **2.1 Energy Monitoring and Measurement** | ✔ YANG Push telemetry |
| **2.2 Power State Management** | ✔ YANG RPCs for control |
| **2.3 Energy Efficiency Optimization** | ⚠️ Partial (manual config) | ✅ Derived metrics + policy | ✔ ietf-energy-policy |
| **2.4 Multi-Device Coordination** | ✔ ietf-energy-collector |
| **2.5 Lifecycle Awareness** | ✔ ietf-energy-audit |
| **2.6 Renewable Energy Usage** | ✔ ietf-energy-derived |
| **2.7 Carbon & Environmental Metrics** | ✔ ietf-energy-audit |
| **2.8 Dynamic Workload Scheduling** |✔ ietf-energy-policy |
| **2.9 SLA and Safety Models** | ✔ ietf-energy-policy |
| **2.10 Cross-Domain Federation** | ✔ ietf-energy-collector |
| **2.11 Accuracy and Attestation** | ✔ ietf-energy-capability |
| **2.12 Reporting and Verification** |✔ ietf-energy-audit |
| **2.13 Operational Integration** | ✔ all modules |
| **2.14 SLA & Policy Compliance** | ✔ ietf-energy-policy |
| **2.15 Market and Regulatory Alignment** | ✔ ietf-energy-audit |

---

## 6. Recommendations for YANG Evolution

| Priority | Module | Key Additions | Status / Notes |
|-----------|----------|----------------|----------------|
| 🟢 1 | `ietf-energy-core` | Core energy metrics (power, temp, load, utilization) | Ready for prototype |
| 🟡 2 | `ietf-energy-capability` | Datasheet and provenance | Draft under review |
| 🟡 3 | `ietf-energy-derived` | Formulas for efficiency and CO₂e | In design |
| 🟠 4 | `ietf-energy-policy` | RPCs for policy enforcement | To be defined |
| 🔵 5 | `ietf-energy-audit` | ESG reporting & compliance linkage | Planned Q1 2026 |

---

## 7. Example Architectural Flow

Vendor Datasheet --> Device Telemetry --> Controller Aggregation --> Derived Metrics --> Policy Enforcement --> Audit & Reporting


Each stage enhances data trust, granularity, and control scope, maintaining a consistent taxonomy across layers.

---

## 8. Next Steps and Work Plan

| Priority | Action | Deliverable | Target Date |
|-----------|----------|--------------|--------------|
| 🟢 1 | Finalize `ietf-energy-core` & `ietf-energy-collector` | Internet-Draft submission | TBC |
| 🟡 2 | Develop `ietf-energy-capability` module | Vendor datasheet integration | TBC |
| 🟡 3 | Prototype `ietf-energy-derived` with KPI formulas | Efficiency / CO₂e validation | TBC |
| 🟠 4 | Define `ietf-energy-policy` RPCs | Control and orchestration | TBC |
| 🔵 5 | Draft `ietf-energy-audit` for reporting alignment | ESG / regulatory compliance | TBC |

---

## 9. Conclusion

The combination of **EMAN semantics**, **POWEFF modularity**, and **YANG extensibility** provides a clear pathway to a comprehensive **GREEN Framework** under the IETF GREEN WG.  

By implementing this architecture:
- Real-time, interoperable, and verifiable energy aware metrics become achievable.  
- Network and cloud systems can contribute to measurable decarbonization outcomes.  
- The IETF positions itself as a technical enabler of **energy aware that can derive to sustainability and NetZero commitments** across digital infrastructure.

---



____

## 📂 YANG Data Model Structure

| File | Purpose | Origin |
|------|----------|--------|
| `ietf-energy-core.yang` | Core telemetry metrics (power, energy, voltage, current, temperature). | Derived from EMAN EnergyObject MIB |
| `ietf-energy-capability.yang` | Static/datasheet information: rated power, supported states, transition times. | Derived from EMAN Context & PowerState |
| `ietf-energy-collector.yang` | Data collection and sampling configuration (SNMP, RESTCONF, gNMI). | Aligned with GREEN collector module proposed by Jan to the GREEN WG|
| `ietf-energy-derived.yang` | Formulas and calculated KPIs (efficiency, PUE, CO₂e). | Derived from POWEFF `derived` metrics |
| `ietf-energy-policy.yang` | Energy policy definitions for controllers. | With the end goal to introduce Energy Policy Defintions: New in GREEN WG framework |
| `ietf-energy-audit.yang` | Reporting and Energy Aware, with extension to Sustainability, compliance information. | New in GREEN WG framework |

## ⚙️ Import and Dependency Order

```text
ietf-energy-core.yang
 ├── ietf-energy-capability.yang
 ├── ietf-energy-collector.yang
 ├── ietf-energy-derived.yang
 ├── ietf-energy-policy.yang
 └── ietf-energy-audit.yang


All modules should import ietf-energy-core.
To build and validate, load them in this order.

---

## 🔗 Conceptual Model

These modules follow a modular YANG architecture mapped to real-world entities:

| Entity | Description | Relevant YANG Modules |
|---------|--------------|------------------------|
| **Datasheet / Vendor** | Provides static rated and efficiency data. | `ietf-energy-capability` |
| **Device** | Measures and reports real-time telemetry. | `ietf-energy-core` |
| **Component** | Sub-element (port, PSU, fan, CPU). | `ietf-energy-core` |
| **Controller / Aggregator** | Collects, aggregates, and computes derived metrics. | `ietf-energy-collector`, `ietf-energy-derived` |
| **Reporter** | Generates energy and audit reports. | `ietf-energy-audit` |
| **Control / Orchestrator** | Applies power-saving policies, automation. | `ietf-energy-policy` |

