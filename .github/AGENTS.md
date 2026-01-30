---
nav_exclude: true
---

# AGENTS.md - Codex Persistent Guidance

This file provides persistent guidance to AI coding assistants (like Codex CLI, GitHub Copilot, Cursor, etc.) about how to work with the Microsoft AI Decision Tree project.

---

## Project Context

**What this project does:**  
Provides a comprehensive decision framework for selecting the right Microsoft AI technology (M365 Copilot, Copilot Studio, Microsoft Foundry, etc.) based on business requirements, technical complexity, and organizational capabilities.

**Why it exists:**  
Microsoft's AI portfolio is vast and rapidly evolving. This framework helps technical decision-makers avoid costly mistakes by providing evidence-based, source-backed guidance for technology selection.

**Target audience:**  
Enterprise architects, technical leads, developers, and business stakeholders evaluating Microsoft AI technologies for production use cases.

---

## Core Principles (ALWAYS FOLLOW)

### 1. The Fact vs. Framework Distinction (Constitution Article I)

**"Cite the Specs, Own the Story."**

* **Technical Truths:** Claims about what a product *is*, limits, pricing, or status (GA/Preview) MUST be verified against Microsoft Learn and cited.
* **Conceptual Truths:** Mental models (e.g., "The Coin," "The Podcast") and analogies DO NOT require citations. They are the teaching lens.

### 2. The Teaching Triad (Writing Style)

**Do not just list products. Teach the concept first.**

Use this pattern: **Concept $\rightarrow$ Analogy $\rightarrow$ Product.**
* *Bad:* "Use Copilot Studio for triggers."
* *Good:* "Think of an Invisible Agent like a thermostat (Analogy). It waits for a change to trigger an action (Concept). In Microsoft's stack, Copilot Studio handles these triggers (Product)."

### 3. Prevent "Shoeboxing"

**Shoeboxing** = Claiming a technology can do something it cannot.

**Real shoeboxing examples we caught:**
- ❌ Fabric Data Agents in "Autonomous" path → ✅ Actually conversational Q&A, not autonomous
- ❌ M365 SDK "has built-in Agent Framework" → ✅ Actually "bring your own orchestrator" model
- ❌ Logic Apps for "multi-agent orchestration" → ✅ Actually triggers SINGLE agents via events

**How to avoid:**
- Research official Microsoft Learn docs before making capability claims
- Don't infer features from product naming

### 4. Mermaid Diagram Validation

**Every diagram in `docs/visual-framework.md` must have:**
1. Dark theme (default Mermaid config)
2. Color-coded nodes set **inline** per diagram (keep white text): Blue #004578, Purple #4b2070, Green #0b6a0b, Orange #8c5e00, Red #a52617
3. Status annotations: `<i>Preview</i>` for preview features
4. Validation summary with source URLs

---

## File Structure & Conventions

### Documentation Files (docs/*.md)

**Required front matter:**
```yaml
---
layout: default
title: [Title]
nav_order: [1-12]
description: "[SEO description]"
---