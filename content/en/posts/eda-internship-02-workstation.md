+++
title = 'EDA Internship Series #2 — Reproducible Linux Workstation for EDA Workflows'
date = 2026-05-02T20:40:26-04:00
draft = false
tags= ["EDA", "Tooling", "Infrastructure"]
categories = ["Engineering"]
series = ["EDA Internship"]
+++

## Context

Electronic Design Automation (EDA) workflows are highly sensitive to environment configuration. Toolchains are complex, dependencies are implicit, and execution often assumes a pre-established system state.

During my internship, I repeatedly observed that productivity was not primarily limited by design complexity, but by environment friction: inconsistent setups, server dependency, and lack of reproducible local execution environments.

During this period, two recurring patterns became evident in the working environment:

On one hand, the shared server infrastructure—used to access critical EDA tools—was frequently unstable. When outages occurred, work would stall completely, since execution was tightly coupled to centralized resources.

On the other hand, many tasks were executed in highly heterogeneous local environments, where tools “worked if they worked”, depending on individual machine configuration and setup. This led to significant variability across users and systems.

This post documents the design and implementation of a reproducible Linux workstation intended to serve as a deterministic execution environment for EDA workflows.

## Problem

EDA development environments are typically:

tightly coupled to remote servers or institutional infrastructure
configured manually with implicit knowledge transfer
dependent on non-documented system state
difficult to reproduce across machines or over time

In practice, this leads to:

long onboarding time for new users
fragile workflows that break outside specific environments
high cognitive overhead for tool usage
lack of portability between systems

The core issue is not tool complexity, but central environment fragility + local environment variability

The core issue was high operational friction in the EDA development environment, driven by a lack of reproducibility and system-level consistency.

This manifested in three main effects:

Low reproducibility of development environments, leading to inconsistent behavior across machines and users.
Slow onboarding and knowledge transfer, due to heavy reliance on implicit, non-documented operational knowledge.
Increased time-to-market (TTM) for experimentation and workflow iteration, caused by dependency on fragile infrastructure and manual setup processes.

## Approach

Instead of treating the workstation as a personal setup, the system was designed as a reproducible and minimal execution environment.

The guiding principle was:

optimize for determinism over convenience, and reproducibility over ad-hoc usability

The system was structured around reducing implicit state and enforcing explicit configuration at every layer.

## Key Design Decisions

### 1. Full-disk encryption with LUKS2 + LVM
A minimal Linux-based system was built and progressively hardened to serve as a deterministic execution base.

The system uses LUKS2 encryption combined with LVM to enforce a structured and secure storage model.

provides consistent disk abstraction
separates encryption layer from logical volume management
enables clean system recovery and migration

### 2. systemd-boot instead of GRUB

systemd-boot was selected over GRUB to reduce boot complexity.

minimal boot chain
easier debugging and configuration
reduced abstraction layers in early system initialization

### 3.- systemd-networkd + iwd for networking

Networking is handled using systemd-networkd and iwd.

eliminates runtime network management abstraction
ensures deterministic network configuration
aligns with systemd-based system initialization model

### 4.- podman for container runtime

Podman was selected as the primary container runtime.

rootless execution model
reduced security surface compared to daemon-based systems
better alignment with reproducible tool execution environments

### 5. Minimal base system (low package entropy)

A conscious effort was made to keep the base system minimal.

manual packages reduced (target: < 100 explicitly installed packages)
removal of non-essential desktop components
strict control over system dependencies

The goal is to minimize hidden system state that affects reproducibility.

### 6. Explicit system configuration over implicit behaviour

A key design constraint was to avoid “implicit configuration knowledge”.

every system component is explicitly configured
no reliance on GUI-driven or hidden defaults
configuration is treated as versionable artifacts

## Implementation Notes

The system was built on Ubuntu 24.04 as a pragmatic choice for initial iteration speed, with the intention of later portability toward Debian-based systems.

Core system configuration includes:

encrypted root filesystem (LUKS2)
LVM-based volume layout
systemd-based boot and network stack
rootless container runtime via podman

Work is ongoing to formalize parts of the system into reproducible scripts (e.g., future Ansible-based provisioning).

## Results

The workstation enables:

fast system recovery from scratch
reduced dependency on external servers for EDA workflows
more controlled execution environments for tooling experiments
a stable base for CLI-based automation development

Most importantly, it establishes a foundation for treating the development environment itself as a reproducible system rather than a static machine configuration.

## Takeaways

EDA workflows are not only constrained by tool complexity, but by the reproducibility of the environments in which those tools execute.

By treating the workstation as a deterministic system rather than a personal setup, it becomes possible to reduce onboarding friction, improve reliability, and create a stable base for tooling and automation development.
