# RMA Nexus -- Roadmap

## Project Status

Architecture defined. Core principles locked. Implementation pending.

RMA Nexus is being developed architecture-first to prevent design drift
and maintain strict separation of concerns.

------------------------------------------------------------------------

# Phase 0 -- Foundation (Locked)

These principles are fixed and must not change:

-   RMA remains deterministic raw memory.
-   SQLite stores metadata only (no live values).
-   Alarm flags live in RMA.
-   Alarm lifecycle timestamps live in runtime memory.
-   SQL is SELECT-only for namespace resolution.
-   No historian responsibilities inside Nexus.

Status: Complete (Design Stage)

------------------------------------------------------------------------

# Phase 1 -- Core Runtime (MVP)

## 1. Registry Integration

-   Implement SQLite metadata schema
-   Tag namespace resolution
-   Allocation lookup
-   Validation on startup
-   Registry freeze mode (optional)

## 2. Query Engine

-   SELECT-only SQL interface
-   Metadata query execution
-   Allocation grouping by bank
-   Contiguous block merge algorithm
-   Optimized RMA reads
-   Structured result return

## 3. Basic API Layer

-   HTTP API
-   JSON response model
-   Query limits (row caps)
-   Health endpoint

Goal: Functional real-time tag resolution engine.

------------------------------------------------------------------------

# Phase 2 -- Alarm Manager

## 1. Alarm Definitions

-   Metadata-based alarm configuration
-   Severity levels
-   Requires acknowledgement flag

## 2. Runtime Alarm Engine

-   Condition monitoring
-   State machine transitions
-   Acknowledge handling
-   Duration calculation (derived)

## 3. Loki Integration

-   Log alarm transitions
-   Log acknowledgement actions
-   Structured event output

Goal: Deterministic alarm lifecycle without historian dependency.

------------------------------------------------------------------------

# Phase 3 -- Performance & Stability

## 1. Optimization

-   Allocation bitmap validation
-   Memory pooling
-   Reduced allocations
-   Concurrency testing

## 2. Load Testing

-   Large namespace tests (10k+ tags)
-   Burst query handling
-   Alarm stress tests

## 3. Failure Handling

-   Restart state rebuild
-   Optional alarm snapshot persistence
-   Registry corruption detection

Goal: Production-grade stability.

------------------------------------------------------------------------

# Phase 4 -- Advanced Features

## 1. Access Control

-   Read-only roles
-   Write permissions per tag
-   Query restrictions

## 2. Streaming Interface

-   WebSocket subscription
-   Tag change notifications
-   Alarm stream feed

## 3. Federation

-   Multi-RMA node aggregation
-   Distributed namespace model

Goal: Scalable ecosystem expansion.

------------------------------------------------------------------------

# Phase 5 -- Ecosystem Integration

-   AI query adapter
-   Structured query abstraction layer
-   Snapshot export
-   Documentation expansion
-   Benchmark publication

Goal: Mature ecosystem component.

------------------------------------------------------------------------

# Long-Term Vision

RMA Nexus becomes:

-   The semantic state convergence layer
-   The bridge between deterministic memory and intelligent systems
-   A modular, production-grade state engine

It remains:

-   Deterministic
-   Layered
-   Cognitive-aware
-   Metadata-driven
-   Historian-independent

------------------------------------------------------------------------

# Non-Goals (Permanent)

RMA Nexus will never become:

-   A time-series database
-   A log aggregation platform
-   A PLC runtime
-   A monolithic SCADA replacement

It is the state layer --- nothing more, nothing less.
