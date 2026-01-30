---
layout: default
title: Evaluation Criteria
nav_order: 6
description: "Framework for evaluating complexity, skills, budget, and governance"
---

# Evaluation Criteria
{: .no_toc }

**"Trade-offs, not solutions."**

This module is the **scoring rubric** for your architecture. Before you commit to a technology, you must evaluate the project against four hard constraints: **Architectural Load** (Complexity), **Delivery Capabilities** (Skills), **Economic Model** (Budget), and **Risk Profile** (Governance).

Use this page to turn abstract requirements into defensible engineering decisions.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 1. Complexity Assessment (Architectural Load)

**The Trade-off:** The "Inverse Law of Control." As you adopt higher-level abstractions (SaaS), you gain velocity but lose granular control over the runtime and orchestration.

**The Analogy:** Think of this like **Construction**.
* **Tier 1 (Furnished Condo):** You live in someone else's building. You can change the decor (System Prompt), but you can't move the walls.
* **Tier 2 (Prefab Home):** You assemble modular rooms. You pick the appliances (Connectors) and the layout, but the foundation is pre-poured.
* **Tier 3 (Custom Build):** You pour the concrete. You choose the wiring, HVAC, and security. If the roof leaks, you fix it.
* **Tier 4 (Skyscraper):** Complex engineering. Requires steel frameworks, wind tunnel testing (Evals), and a specialized crew to operate.



**Question:** What is the *structural* complexity of the solution?

| Complexity Tier | System Characteristics | Architecture Pattern | Recommended Technology |
| :--- | :--- | :--- | :--- |
| **Tier 1: Informational**<br>*(Read-Only)* | Single knowledge domain. No state retention. Answers questions based on grounded data. | **RAG (Retrieval Augmented Generation)** | **M365 Copilot**. *Leverage the Graph; no custom orchestration.* |
| **Tier 2: Transactional**<br>*(Deterministic)* | Linear workflows ("If X, then Y"). Defined API calls. Human approval steps required. | **Orchestrated Workflow** | **Copilot Studio**. *Visual flow designer with Power Platform connectors.* |
| **Tier 3: Reasoning**<br>*(Probabilistic)* | Multi-step planning. The agent must decide *which* tool to use. Latency-critical. Custom vector stores. | **ReAct / Plan-and-Execute** | **M365 Agents SDK** (if M365-hosted) or **Microsoft Foundry** (if Azure-hosted) or Microsoft Foundry + Copilot Studio. |
| **Tier 4: Autonomous**<br>*(Nondeterministic)* | Multi-agent collaboration (Swarm). Recursive self-correction. Long-running asynchronous tasks. | **Multi-Agent Systems (MAS)** | **Microsoft Foundry, Agent Frameworkm, or Both**. *Full code-first control if needed* |

---

## 2. Skills & Resources (Delivery Team)

**The Trade-off:** **Abstraction vs. Rigor.** Low-code tools abstract away infrastructure but constrain the developer's workflow (ALM). Pro-code tools offer infinite flexibility but require you to manage the plumbing (Identity, Networking, State).

**Question:** What is the composition of the delivery team?

| Team Profile | Optimization | Recommended Path |
| :--- | :--- | :--- |
| **Makers / Fusion** | **Velocity.** Focus on subject matter expertise and business logic. | **Copilot Studio + AI Builder.** Rapid prototyping without infrastructure overhead. |
| **Pro Developers** | **Lifecycle.** Focus on CI/CD, unit testing, and version control. | **M365 Agents SDK, Foundry, Foundry Agent Service, Agent Framework**. Code-first approaches that fit into existing developer skills. |
| **Data Scientists** | **Precision.** Technical experts focused on data, grounding, and model behavior (not necessarily app dev). | **Foundry Agent Service or Copilot Studio.** Allows technical non-developers to deploy custom models as agents without managing full-stack infrastructure or writing application code. |

> [!TIP]
> **The Convergence Principle:** Do not treat "Pro Code" and "Low Code" as binary. A Principal Architect often uses **Copilot Studio** to handle authentication and UI state, while delegating complex reasoning to a **Foundry** Azure Function.

---

## 3. Budget Assessment (The Economic Model)

Architects must speak the language of finance. You need to capture Total Cost of Ownership (TCO)—licensing, consumption, and the engineering hours required to build it.

**Question:** Is your budget model based on **Capital Expenditure** (Pre-paid seats) or **Operating Expenditure** (Consumption)?

| Budget Band | Cost Model | Rough Estimate | Best Fit For... |
| :--- | :--- | :--- | :--- |
| **Included** | **Entitlement** | $0 (Included) | **M365 Copilot Chat.** If users have licenses, this is the highest ROI baseline. Zero incremental cost. |
| **Pilot** | **Per-User** | < $500/mo | **M365 Copilot Pilots.** Extending M365 Copilot with Graph Connectors involves no Azure infra spend, just maker time. |
| **Production** | **Predictable SaaS** | $2k - $10k/mo | **Copilot Studio.** Best for internal tools where you need predictable monthly billing (via Capacity Packs) rather than variable cloud bills.[^copilot-cost] |
| **Scale** | **Consumption** | $10k+/mo | **Microsoft Foundry / Agent Service.** Best for high-volume apps. You pay for the tokens you use. Requires managing quotas and Azure budgets.[^foundry-cost] |

> **Warning:** Consumption models require **Quota Management**. An ungoverned autonomous agent can burn through a monthly token budget in hours if it enters a loop. Always implement spending caps in Azure Cost Management.

---

## 4. Time to Production (The Runway)

**The Trade-off:** **Convenience vs. Customization.** You can launch a standard pattern in days, or a bespoke architecture in months.

| Timeline | Capability | Why |
| :--- | :--- | :--- |
| **Days** | **M365 Copilot / Declarative Agents** | No infrastructure to deploy. Content is already indexed by Graph. |
| **Weeks** | **Copilot Studio** | Visual canvas allows rapid iteration, but requires testing approval flows and connectors. |
| **Months** | **Foundry / Agents SDK** | Requires establishing Azure Landing Zones, VNets, and CI/CD pipelines before the first agent runs. |

---

## 5. Governance & Compliance (The Security Perimeter)

This is the "Go/No-Go" gate. You must define your **Data Boundary** and **Action Safety**.

### The Trust Boundary Decision
**Question:** Does the data stay in the SaaS tenant or move to your Azure subscription?



* **M365 Trust Boundary:** Data remains within the Microsoft 365 tenant boundary. Adheres to existing tenant certifications (ISO, SOC, HIPAA). No model training on customer data. -> *Use M365 Copilot.*
* **Power Platform Boundary:** Inherits compliance from the *Connectors* used. If you connect to a non-compliant third-party API, the agent inherits that risk. -> *Use Copilot Studio.*
* **Azure Landing Zone:** Data moves into your customer-controlled subscription. You own the VNet injection, Private Links, and Encryption Keys (CMK). -> *Use Microsoft Foundry.*

### Action Safety & Content Safety
**Question:** What is the "Blast Radius" of a mistake?

* **Read-Only Risk:** Hallucination/Grounding errors. (Mitigation: RAG + Citations).
* **Destructive Risk:** Data modification/Deletion. (Mitigation: Human-in-the-loop).

#### The Action Safety Guardrail Playbook
Use this rubric to design approval checkpoints before promoting an agent to production.

| Risk Level | Definition | Guardrail Requirement |
| :--- | :--- | :--- |
| **Low (Read)** | Search, lookup, summarize. | **Audit Log.** Log the query and response for post-hoc analysis. |
| **Medium (Write)** | Create draft, update status. | **User Confirmation.** The agent presents a draft/plan; User must explicitly click "Execute." |
| **High (Destructive)** | Delete, transfer funds, change permission. | **Middleware Interception.** The agent triggers a request; a Service Owner must approve via a separate channel.[^humanreview] |

**Implementation Example (Pro-Code):**
For high-risk actions in code-first agents, implement middleware that intercepts the specific tool call:

```typescript
async function executeToolWithApproval(toolName: string, params: any) {
  if (isDestructive(toolName)) {
    const approval = await requestHumanApproval(toolName, params);
    if (!approval.approved) { return { error: "Action rejected by reviewer" }; }
  }
  return await executeTool(toolName, params);
}
```

---

## 6. Scale & Performance (The Envelope)

**Question:** What are the latency and throughput requirements?

| Scale | Volume | Copilot Studio Fit | Microsoft Foundry Fit |
| --- | --- | --- | --- |
| **Departmental** | Low Volume | ✅ **Ideal.** Instant scaling, managed limits. | ⚠️ **Overhead.** High setup time for low volume. |
| **Enterprise** | Medium Volume | ✅ **Strong.** Requires Capacity Planning. | ✅ **Strong.** Standard Pay-As-You-Go models. |
| **High-Scale** | High Volume (1M+ req/day) | ⚠️ **Throttling Risk.** Check Environment RPM limits. | ✅ **Ideal.** Use PTU for guaranteed latency and throughput.[^1] |

**Key Limits to Watch:**

* **Copilot Studio:** Generative AI requests consume "Copilot Credits" at a different rate than standard messages. Limits are often around 8,000 requests per minute (RPM) per environment, but vary by license.[^2]
* **Azure OpenAI:** TPM (Tokens Per Minute) quotas are regional. For mission-critical scale, design for **multi-region failover** and model redundancy.

---

## Evaluation Checklist

Before moving to the [Implementation Patterns]({{ '/docs/implementation-patterns' | relative_url }}), confirm you have scored the scenario:

**Architecture:**

* [ ] Complexity Level (Config vs. Engineering) identified?
* [ ] Data boundaries mapped?

**Resources:**

* [ ] Team capability (Maker vs. Dev vs. DS) aligned to tool?
* [ ] ALM/DevOps requirements defined?

**Governance:**

* [ ] Trust boundary defined (M365 vs. Azure)?
* [ ] Action Safety Guardrails defined?

**Budget:**

* [ ] Cost model selected (License vs. Metered)?
* [ ] Estimated monthly spend band identified?

---

## Next Steps

**Feature comparison:**  
→ [Feature Comparison]({{ '/docs/feature-comparison' | relative_url }})

**Visual guidance:**  
→ [Visual Framework]({{ '/docs/visual-framework' | relative_url }})

**Real examples:**  
→ [Scenarios]({{ '/docs/scenarios' | relative_url }})

**Architecture patterns:**  
→ [Implementation Patterns]({{ '/docs/implementation-patterns' | relative_url }})

---

**Last Updated:** January 28, 2026

**Next:** [Implementation Patterns]({{ '/docs/implementation-patterns' | relative_url }}) - Apply the scoring outcomes to pick execution patterns
