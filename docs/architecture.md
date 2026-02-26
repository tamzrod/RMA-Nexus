# RMA Nexus -- Architecture Document

## 1. Overview

RMA Nexus is the semantic state layer built on top of RMA's
deterministic memory fabric.

It provides:

-   Namespace resolution
-   Structured query execution
-   Alarm lifecycle management
-   Controlled real-time state access

RMA Nexus does not store live values in a database. All runtime values
are read directly from RMA memory.

------------------------------------------------------------------------

## 2. Layered Architecture

RMA Nexus follows strict separation of concerns.

    RMA (Memory Fabric)
            ↓
    RMA Nexus (Semantic Layer)
            ↓
    API / Query Engine
            ↓
    UI / AI / Automation

Supporting systems:

-   InfluxDB → numeric time-series history
-   Loki → event and transition history
-   SQLite → metadata registry only

------------------------------------------------------------------------

## 3. Core Components

### 3.1 RMA (Memory Fabric)

Responsibilities:

-   Deterministic raw memory banks
-   Fixed cell-size banks (digital, byte, 16bit, 32bit, 64bit)
-   Concurrency-safe read/write
-   No semantic awareness

RMA stores:

-   Tag values
-   Alarm condition bits
-   Acknowledge bits

RMA does not store:

-   Namespace
-   Metadata
-   Historical data

------------------------------------------------------------------------

### 3.2 Metadata Registry (SQLite)

SQLite stores configuration and mapping only.

Responsibilities:

-   Tag namespace hierarchy
-   Memory allocation mapping
-   Alarm definitions
-   Severity metadata
-   Query indexing

SQLite never stores:

-   Live values
-   Alarm runtime state
-   Duration counters

------------------------------------------------------------------------

### 3.3 Query Engine

The query engine resolves metadata and reads memory.

Execution flow:

1.  Receive SQL (SELECT-only) or structured API request
2.  Query SQLite for tag metadata
3.  Group allocations by memory bank
4.  Sort by start_cell
5.  Merge contiguous blocks
6.  Perform minimal RMA reads
7.  Attach values to result rows
8.  Return structured response

SQL is used as a namespace selection mechanism, not a storage engine.

------------------------------------------------------------------------

### 3.4 Alarm Manager

Alarm Manager is a runtime state machine.

Alarm flags live in RMA:

-   active bit
-   acknowledged bit

Alarm Manager tracks lifecycle timestamps in memory:

-   activated_at
-   acknowledged_at
-   cleared_at

Duration is derived:

If active: duration = now - activated_at

If cleared: duration = cleared_at - activated_at

Alarm transitions are logged to Loki. Alarm analytics may optionally be
exported to InfluxDB.

------------------------------------------------------------------------

## 4. Truth Layers

RMA Nexus distinguishes between three types of truth.

1.  Physical Truth (RMA)
    -   Raw memory state
2.  Semantic Present Truth (RMA Nexus)
    -   Interpreted tag state
    -   Alarm lifecycle state
3.  Temporal Truth (External Systems)
    -   Numeric history → InfluxDB
    -   Event history → Loki

These layers must never be merged.

------------------------------------------------------------------------

## 5. Design Principles

### 5.1 Deterministic Runtime

Runtime state must not depend on disk I/O. All live reads come from
memory.

### 5.2 Metadata-Only Registry

SQLite stores structure, not state.

### 5.3 No Runtime Value Persistence

Tag values are never written to SQLite.

### 5.4 Clear Boundaries

Each layer has a single responsibility.

### 5.5 Cognitive-Aware Access

RMA Nexus is optimized for focused, meaningful queries. It is not a bulk
data export engine.

------------------------------------------------------------------------

## 6. Failure & Restart Strategy

On restart:

-   Registry loads from SQLite
-   RMA initializes memory banks
-   Alarm Manager rebuilds state from active condition bits

Optional enhancement: Persist alarm state snapshot for recovery of
activation timestamps.

------------------------------------------------------------------------

## 7. Non-Goals

RMA Nexus is not:

-   A historian
-   A logging platform
-   A PLC runtime
-   A rules engine
-   A distributed database

It is the semantic state convergence layer over RMA.

------------------------------------------------------------------------

## 8. Future Extensions (Planned)

-   Federation across multiple RMA nodes
-   Access control policies
-   Streaming subscriptions
-   Snapshot export
-   Structured AI query interface

------------------------------------------------------------------------

## 9. Status

Architecture defined. Implementation pending.
