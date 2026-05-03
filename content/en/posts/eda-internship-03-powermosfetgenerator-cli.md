+++
title = 'EDA Internship Series #3 — Power MOSFET Layout Automation: from standalone script to CLI'
date = 2026-05-02T22:20:23-04:00
draft = false
tags= ["EDA", "Tooling", "Infrastructure"]
categories = ["Engineering"]
series = ["EDA Internship"]
+++

## Context

he initial Power MOSFET layout automation system was implemented as a standalone Python script operating over a set of GDS block inputs.

The script was functional, but tightly coupled to:

hardcoded file paths
internal configuration variables
manual execution steps
implicit assumptions about input structure

While suitable for experimentation, this approach limited reproducibility and made scaling or reuse across different designs difficult.

## Problem

The standalone script-based implementation introduced several structural limitations:

Execution required manual modification of source code or runtime variables
Input blocks and layout parameters were not formally defined as interfaces
There was no separation between configuration, logic, and execution
Re-running or modifying designs required editing the script itself
The workflow was not easily integrable into automated pipelines

As a result, the system was difficult to reproduce, extend, or integrate into a larger engineering workflow.

## Approach

The system was refactored into a structured CLI-driven automation tool, designed to formalize execution and decouple configuration from implementation.

### 1. CLI abstraction over layout generation logic

The core layout generation logic was preserved, but exposed through a command-line interface:

explicit parameter input (e.g. transistor count, layout configuration)
removal of hardcoded runtime configuration
standardized execution entrypoint

This shifted the system from script execution to tool execution.

### 2. Separation of concerns


The codebase was reorganized into distinct layers:

Core logic layer: MOSFET layout generation using GDS block composition
Interface layer: CLI argument parsing and execution control
Data layer: reusable GDS input blocks (left/mid/right segments)

This separation improved maintainability and reduced coupling between logic and configuration.

### 3. Deterministic execution model

The CLI tool enforces a deterministic workflow:

identical inputs produce identical layouts
all configuration is explicitly provided at runtime
no hidden state or internal assumptions required for execution

This enables reproducibility across environments and machines.

### 4. Repository-driven workflow

The system was structured around a version-controlled repository:

input GDS blocks are tracked in version control
layout logic evolves through explicit commits
tool behavior is reproducible from repository state

This enables traceability of design evolution over time.

### 5. Transition from script to tool

The key architectural shift was conceptual:

from: “modify and run script”
to: “execute a defined tool with parameters”

This changes the workflow from ad-hoc execution to structured engineering process.

## Key Decisions

Refactor standalone script into CLI-based tool
Separate logic, interface, and data layers
Eliminate hardcoded configuration from execution path
Enforce deterministic input/output behavior
Use repository structure as source of truth for workflow state


