# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the design repository for a **personalized learning management system**. The system ingests a knowledge base (knowledge nodes, dependencies, learning resources), tracks user learning records and feedback, and recommends next learning steps using an explore/exploit balance algorithm informed by forgetting curves.

The primary use case is managing a graph rendering learning repo (OpenGL + Vulkan), but the system is designed to be domain-agnostic and reusable across learning projects.

## Design Principles

- **Config-driven, not hardcoded** — all knowledge, paths, and rules live in configuration files
- **Privacy-first, local computation** — no cloud sync, all data stored locally
- **Non-intrusive feedback collection** — minimize friction, don't break learning flow
- **Multi-project via configuration isolation** — same codebase, different config per project

## Architecture

```
┌─────────────────────────────────────────────┐
│ Frontend (Learning UI)                      │
│  Today’s tasks / Notes / Feedback / Stats   │
├─────────────────────────────────────────────┤
│ Recommendation Engine                        │
│  Knowledge graph + user state → path        │
├─────────────────────────────────────────────┤
│ Backend Service                              │
│  Knowledge mgmt / Analytics / Feedback      │
├─────────────────────────────────────────────┤
│ Data Layer                                   │
│  Knowledge base / User state / Feedback log │
└─────────────────────────────────────────────┘
```

### Core Data Model

| Entity | Role |
|---|---|
| Knowledge Node | Atomic unit: concept, note, pitfall record |
| Learning Record | Time, duration, comprehension rating |
| Feedback Event | Explicit (rating) or implicit (time, skip) |
| Learning Path | Directed acyclic graph of knowledge nodes |

### Recommendation Strategy Rules

| Condition | Action |
|---|---|
| Prerequisite not mastered | Force regression |
| Multiple "didn't understand" | Lower difficulty or swap resource |
| Pitfall marked | Show pitfall record + practice |
| 3 days without review | Apply Ebbinghaus scheduling |
| Multiple consecutive completions | Accelerate pace |
| Skip a node | Analyze reason |

### Forgetting Curve Model

```
mastery(t) = initial_mastery × e^(-t/τ)
τ is personalized based on user performance
```

### Recommendation Score Formula

```
score = f(importance, mastery, forgetting_curve, goals)
```

## Implementation Roadmap

| Phase | Scope |
|---|---|
| Phase 1 | MVP: single HTML + YAML config + checkbox completion + localStorage |
| Phase 2 | Generalization: normalized config schema + multi-project testing |
| Phase 3 | Polish: modularization + themes + notes feature |
| Phase 4 | Distribution: GitHub Pages / npm / iframe widget |

## Implementation Notes

- Phase 1 should start with data model design first — define knowledge node schema and knowledge base structure
- Build the minimal feedback loop (record → recommend → verify) before adding features
- Knowledge graph can start manual/semi-automated before considering automated extraction
- All knowledge, paths, and strategy rules must be declarative config, not code

## Open Questions (unresolved)

- Knowledge graph construction: fully automated vs. semi-automated vs. manual
- Recommendation real-time: live compute vs. local cache
- Cross-project knowledge references