# cel-contracts

[![crates.io](https://img.shields.io/crates/v/cel-contracts.svg)](https://crates.io/crates/cel-contracts)
[![docs.rs](https://docs.rs/cel-contracts/badge.svg)](https://docs.rs/cel-contracts)
[![CI](https://github.com/dimpagk92/cel-contracts/actions/workflows/ci.yml/badge.svg)](https://github.com/dimpagk92/cel-contracts/actions/workflows/ci.yml)

Shared Rust contracts for AI agent actions, planning views, execution receipts,
and runtime capabilities.

`cel-contracts` contains pure data types for the boundary between a planner and
the runtime that carries out its actions. It is intentionally small: no LLM
clients, no runtime loop, no storage backend, no policy engine.

## Purpose

Use `cel-contracts` when one component decides what should happen and another
component executes it. The shared schema lets planners, runtimes, adapters,
logs, tests, and dashboards agree on actions, capabilities, observations, and
receipts without sharing implementation code.

## What's Included

- `PlannedAction` — what a planner asks a runtime to do.
- `PlanningView` — budgeted context shape a planner can consume.
- `ExecutionReceipt` — what the runtime actually dispatched and observed.
- `DispatchRoute` — adapter, CDP, accessibility, native input, focus, or other.
- `ObservedEffect` — whether the expected effect was observed, timed out, contradicted, or not checked.
- `RuntimeCaps` and related planning/run contracts.

## Why It Exists

Agent frameworks and runtimes need a shared language at the boundary:

```text
planner → PlannedAction → runtime → ExecutionReceipt
```

Any runtime can implement this boundary while keeping the schema open,
inspectable, and testable.

**Status:** v0.2.0 on [crates.io](https://crates.io/crates/cel-contracts) — depends on **`cel-context` 0.2.0**.

## Upgrading from 0.1.x

Bump `cel-context` to **0.2.0** alongside this crate. See the
**[0.2 migration guide](https://github.com/dimpagk92/cellar/blob/main/docs/migration-0.2.md)**.

## Receipt Boundary

`ExecutionReceipt` proves dispatch and observed effect. It does not prove the
whole user goal is complete. Completion still requires post-state evidence such
as adapter readback, CDP/AX state, a fresh context snapshot, screenshot, or
external system confirmation.

## Example

```sh
cargo run --example action_receipt
```

The example serializes a `PlannedAction` and the `ExecutionReceipt` a runtime
could emit after dispatch.

## License

Apache-2.0
