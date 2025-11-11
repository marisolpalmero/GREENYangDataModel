# IETF GREEN WG — YANG Prototype Modules for Energy Management

This repository contains a set of YANG data models developed under the **IETF GREEN Working Group** initiative.  
They extend and modernize the **Energy Management (EMAN)** architecture, aligning it with the **POWEFF** modular approach and the **GREEN WG Use Cases (draft-stephan-green-use-cases-03)**.

The goal is to enable **real-time, vendor-neutral telemetry**, **energy-aware control**, and **reporting** across devices, components, controllers, and aggregators.

---

## 📂 Repository Structure

| File | Purpose | Origin |
|------|----------|--------|
| `ietf-energy-core.yang` | Core telemetry metrics (power, energy, voltage, current, temperature). | Derived from EMAN EnergyObject MIB |
| `ietf-energy-capability.yang` | Static/datasheet information: rated power, supported states, transition times. | Derived from EMAN Context & PowerState |
| `ietf-energy-collector.yang` | Data collection and sampling configuration (SNMP, RESTCONF, gNMI). | Aligned with POWEFF `collector` module |
| `ietf-energy-derived.yang` | Formulas and calculated KPIs (efficiency, PUE, CO₂e). | Derived from POWEFF `derived` metrics |
| `ietf-energy-policy.yang` | Energy policy definitions for controllers. | With the end goal to introduce Sustainability Policy Defintions: New in GREEN WG framework |
| `ietf-energy-audit.yang` | Reporting and sustainability compliance information. | New in GREEN WG framework |

---

## 🔗 Conceptual Model

These modules follow a modular YANG architecture mapped to real-world entities:

| Entity | Description | Relevant YANG Modules |
|---------|--------------|------------------------|
| **Datasheet / Vendor** | Provides static rated and efficiency data. | `ietf-energy-capability` |
| **Device** | Measures and reports real-time telemetry. | `ietf-energy-core` |
| **Component** | Sub-element (port, PSU, fan, CPU). | `ietf-energy-core` |
| **Controller / Aggregator** | Collects, aggregates, and computes derived metrics. | `ietf-energy-collector`, `ietf-energy-derived` |
| **Reporter** | Generates sustainability and audit reports. | `ietf-energy-audit` |
| **Control / Orchestrator** | Applies power-saving policies, automation. | `ietf-energy-policy` |

---

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
