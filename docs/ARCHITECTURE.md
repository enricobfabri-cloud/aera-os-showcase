# Aera OS: architecture and tradeoffs

Aera OS is an Android-based shell and app platform under development. This document is a high-level explanation for a public portfolio; it intentionally leaves out private source, deployment configuration, and internal operational details.

## Component responsibilities

| Component | Responsibility | Why it is separate |
| --- | --- | --- |
| Home shell | Launcher and shared system surface | Keeps global behavior consistent across apps |
| Window management | Common window and spatial-panel interactions | Avoids independent, conflicting window models in every app |
| App runtime | Hosts manifest-backed Aera apps | Provides a defined boundary between platform and application |
| SDK and platform contracts | Shared interfaces for apps and tooling | Makes integration expectations explicit |
| First-party React Native apps | App-specific content and workflows | Allows reusable UI development within the platform |
| Native Android bridge | Access to platform capabilities | Keeps native integration behind a deliberate boundary |
| AOSP product overlay | Emulator product integration | Connects the shell and bundled apps to the development image |

## Challenges and decisions

### One coherent system, multiple independent apps

The shell should behave consistently while apps remain independently editable. A shared runtime and SDK provide that common foundation. The tradeoff is that changes to shared contracts require care because they can affect more than one app.

### Spatial controls alongside familiar content

Notes, files, and weather have familiar information structures. The spatial interface adds window controls and adjacent panels without requiring every app to invent its own platform behavior. Clear ownership between shell and app makes the interaction model easier to reason about.

### Cross-platform UI inside a native platform

React Native and TypeScript support reusable application UI. Android and Kotlin provide the host and native integration. A bridge connects those layers, with explicit contracts to control their coupling.

### Iterating without disrupting unrelated work

The development workflow supports scoped package work and verification. This is especially useful when several apps share one emulator environment: a change to one app should not require unrelated app data or behavior to change.

## Evidence and limits

The private implementation workspace contains the shell, runtime, shared packages, first-party apps, Android tests, package tests, and AOSP integration. Their presence establishes implementation structure, not a public claim that every test currently passes or that all features are production-ready.

The screenshots in this repository are existing emulator captures. They are not generated mockups, benchmark results, or evidence of a hardware release.
