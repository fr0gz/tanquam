+++
title = 'Eda Internship 02 Workstation'
date = 2026-05-02T20:40:26-04:00
draft = false
tags= ["EDA", "Tooling", "Infrastructure"]
categories = ["Engineering"]
series = ["EDA Internship"]
+++

## Context

During my internship at AC3E / Synopsys, I was exposed to EDA tools and workflows used in integrated circuit design, particularly Synopsys-based environments running on Unix-like systems.

During this period, two recurring patterns became evident in the working environment:

On one hand, the shared server infrastructure—used to access critical EDA tools—was frequently unstable. When outages occurred, work would stall completely, since execution was tightly coupled to centralized resources.

On the other hand, many tasks were executed in highly heterogeneous local environments, where tools “worked if they worked”, depending on individual machine configuration and setup. This led to significant variability across users and systems.

## Problem

The core issue was high operational friction in the EDA development environment, driven by a lack of reproducibility and system-level consistency.

This manifested in three main effects:

Low reproducibility of development environments, leading to inconsistent behavior across machines and users.
Slow onboarding and knowledge transfer, due to heavy reliance on implicit, non-documented operational knowledge.
Increased time-to-market (TTM) for experimentation and workflow iteration, caused by dependency on fragile infrastructure and manual setup processes.
## Approach

The work focused on reducing friction in EDA workflows by engineering a local, reproducible execution environment that can operate independently from unstable centralized infrastructure while still integrating with required EDA tools.

The approach was structured around three concrete technical directions:

### 1. Reproducible workstation environment
A minimal Linux-based system was built and progressively hardened to serve as a deterministic execution base.

Key elements include:

controlled package surface (apt-mark showmanual)
encrypted disk setup (LUKS2 + LVM)
simplified boot chain (systemd-boot)
removal of non-essential system components to reduce state variability

### 2. Workflow automation via CLI layer

Ad-hoc scripts were progressively refactored into structured command-line tools to standardize execution flows.

This includes:

Python-based layout automation scripts (gdstk-based flow)
transition from single-purpose scripts → composable CLI interface
consistent input/output conventions for reproducibility
local execution of pre/post-processing independent of EDA server availability

### 3.- Environment abstraction and decoupling

Instead of depending directly on interactive server sessions, the workflow was structured into layers:

Host layer: minimal OS + system configuration
Execution layer: containerized or isolated tooling (Podman)
Workflow layer: scripts, CLI tools, and automation logic

This separation reduces coupling between:

system configuration
tool execution
workflow logic

## Takeaways

EDA workflows are not only tool-dependent, but environment-dependent systems.

In practice, most operational friction does not originate from the tools themselves, but from:

inconsistent execution environments
lack of reproducibility across machines
manual and implicit setup procedures

This leads to the key technical insight:

Improving EDA productivity requires treating the development environment as an engineered system, not as a collection of independent tools.
