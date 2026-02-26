# RMA Nexus

**RMA Nexus** is the semantic state layer built on top of RMA's
deterministic memory fabric.

It provides namespace resolution, structured query access, and alarm
lifecycle management over raw memory --- without compromising real-time
determinism.

RMA Nexus does **not** replace RMA.\
It does **not** store live values in a database.\
It does **not** act as a historian.

It is the convergence layer between memory, meaning, and human
interaction.

------------------------------------------------------------------------

## Vision

RMA provides raw, deterministic memory banks.

RMA Nexus gives that memory structure, identity, and operational
semantics.

It connects:

-   Namespace ↔ Memory
-   SQL ↔ RMA
-   Alarms ↔ Tag state
-   Humans ↔ Real-time truth

Nexus is the bridge.

------------------------------------------------------------------------

## Core Principles

### 1. Deterministic Runtime

RMA remains the single source of runtime truth.

All live values are read directly from memory.

No value persistence inside the registry.

------------------------------------------------------------------------

### 2. Metadata-Only Registry

SQLite is used strictly for:

-   Tag namespace
-   Memory allocation mapping
-   Alarm definitions
-   Configuration metadata

It never stores live values.

------------------------------------------------------------------------

### 3. Clear Separation of Truth Layers

  Layer       Responsibility
  ----------- ------------------------------
  RMA         Physical memory truth
  RMA Nexus   Semantic real-time truth
  InfluxDB    Temporal numeric history
  Loki        Event and transition history

Each layer has a single responsibility.

------------------------------------------------------------------------

### 4. Alarm State Machine

Alarm flags live in RMA:

-   `active` bit
-   `acknowledged` bit

Alarm lifecycle timestamps are managed in runtime memory.

Duration is derived:

If active: duration = now - activated_at

If cleared: duration = cleared_at - activated_at

No historian is required for duration.

------------------------------------------------------------------------

### 5. SQL as Namespace Query Engine

SQL is used to select tags from the registry.

Example:

    SELECT * FROM tags WHERE level1 = 'plant1';

Execution flow:

1.  Query metadata from SQLite
2.  Group allocations by memory bank
3.  Merge contiguous blocks
4.  Read from RMA
5.  Return enriched result

SQL never mutates runtime state.

------------------------------------------------------------------------

### 6. Cognitive-Aware Design

RMA Nexus is not a bulk data dump engine.

It is designed for:

-   Focused real-time inspection
-   Operational clarity
-   Controlled query limits
-   Meaningful state visibility

If large historical pulls are required, use the historian layer.

------------------------------------------------------------------------

## Architecture Overview

RMA (Memory Fabric) ↓ RMA Nexus (Semantic Layer) ↓ API / Query Interface
↓ UI / AI / Automation

Supporting systems:

-   InfluxDB → numeric history
-   Loki → event transitions

------------------------------------------------------------------------

## What RMA Nexus Is NOT

-   Not a time-series database
-   Not a log aggregation system
-   Not a PLC runtime
-   Not a historian
-   Not a replacement for RMA core

It is the semantic state layer.

------------------------------------------------------------------------

## Current Status

Architecture defined.\
Implementation pending.

The design is intentionally documented before code to preserve
architectural clarity and prevent drift.

------------------------------------------------------------------------

## License

Apache License 2.0
