# 🌬️ AirPulse (Software Edition)
**Real-time Air Quality & Health Monitoring with EdgeFrame & Forgetful Forest**

> **Software-only** implementation that simulates edge behavior (no wearables). Uses public AQI datasets + synthetic health signals to evaluate a hybrid **EdgeFrame (rules)** + **Forgetful Forest (online ML)** approach under concept drift.

---

## Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Datasets](#datasets)
- [Existing Systems vs AirPulse](#existing-systems-vs-airpulse)
- [Key Features](#key-features)
- [Development Phases](#development-phases)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the System](#running-the-system)
- [Experiments & Expected Results](#experiments--expected-results)
- [Monitoring & Observability](#monitoring--observability)
- [Directory Structure](#directory-structure)
- [Contributing](#contributing)
- [References](#references)
- [License](#license)

---

## Overview
AirPulse (Software Edition) is a **hybrid real-time monitoring platform** that simulates environmental (AQI/PM2.5, CO₂, VOCs) and health signals (Heart Rate, Temperature) and applies **edge-inspired decision-making** with **adaptive streaming ML**.

Core ideas:
- **EdgeFrame Threshold Engine** → JSON-driven rules for instant local alert simulation and bandwidth-aware filtering.
- **Forgetful Forest (FF)** → Online, drift-aware model for classification (Safe/Unsafe) and/or regression (predict AQI/PM2.5).
- **Streaming Evaluation** → Prequential metrics over time, latency, bandwidth savings, and drift robustness.

This is **software-only** (no hardware). Data is sourced from **public AQI datasets** and **synthetic health signal generators**.

---

## Architecture

```mermaid
flowchart TD
    A[Data Sources\n(AQI CSVs/APIs + Synthetic vitals)] --> B[EdgeFrame Simulator\n(JSON thresholds, local alerts)]
    B -->|alerts/summaries| C[Redpanda/Kafka Broker]
    A -->|raw stream (optional)| C
    C --> D[Forgetful Forest Service\n(online ML, pretrain + prequential)]
    D --> E[(PostgreSQL)]
    D --> F[(Redis)]
    D --> G[API Gateway]
    G --> H[Grafana + Prometheus\nDashboards & Metrics]
