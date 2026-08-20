# Project Priory: Neuro-Symbolic Architecture & Pipeline Interaction Reference

This document provides a comprehensive technical breakdown and visual graphs illustrating the end-to-end data flow, component interactions, and the precise roles of the **Semantic Web Layer (OWL 2 DL, SKOS, RDFLib, SPARQL)** in Project Priory, focusing on how `gw2-priory-ref` controlled vocabularies and taxonomies interface with the reasoning engine.

---

## 1. High-Level Architecture Overview

Project Priory executes a **Neuro-Symbolic Sandwich** pattern that combines the natural language strengths of Large Language Models with the strict determinism and verifiable truth of Semantic Web technologies and mathematical graph algorithms.

```mermaid
flowchart TD
    subgraph Layer1["1. Top Layer: Intent Extraction & Constraint Parsing"]
        User["User Natural Language Input"] --> Orchestrator["PrioryAgentOrchestrator\n(PrioryChatSession)"]
        Orchestrator --> IntentParser["IntentParser\n(Top LLM)"]
        IntentParser --> PydanticIntent["PlayerGoalIntent & ResolvedGoal\n(Typed Pydantic Schema)"]
    end

    subgraph Layer2["2. Core Layer: Semantic Knowledge Graph & Deterministic Math"]
        PydanticIntent --> SQS["SemanticQueryService\n(SPARQL Entity Resolution)"]
        SQS <--> GraphStore["PrioryGraphStore\n(In-Memory RDFLib Triples)"]
        GraphStore <--> TTL["OWL 2 DL Schemas & SKOS Vocabularies\n(priory_core.ttl, vocab/*.ttl, instances/*.ttl)"]
        
        SQS --> DiffEngine["AccountDiffEngine\n(DAG Traversal & Account Delta)"]
        DiffEngine <--> GraphStore
        DiffEngine <--> Account["Player AccountState\n(Materials, Bank, Wallet, Disciplines)"]
        
        DiffEngine --> Solver["PathSolver\n(Time-Gate, Gold & Route Optimization)"]
        Solver <--> GraphStore
        
        DiffEngine --> Ranker["AccountRanker\n(Leaderboards & Expansion Filters)"]
        Ranker <--> GraphStore
    end

    subgraph Layer3["3. Bottom Layer: Grounded Synthesis & User Presentation"]
        Solver --> SubgraphContext["Semantic Context Serialization\n(Waypoints, NPCs, Recipes)"]
        SubgraphContext --> GuideGen["GuideGenerator\n(Bottom LLM)"]
        DiffEngine --> GuideGen
        GuideGen --> PydanticGuide["PersonalizedGuide\n(Structured JSON Schema)"]
        PydanticGuide --> UserMarkdown["Rendered User Guide\n(Checklists, Chat Codes, Timetables)"]
    end

    classDef llm fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef semantic fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px;
    classDef engine fill:#e8f5e9,stroke:#388e3c,stroke-width:2px;
    classDef output fill:#fff3e0,stroke:#f57c00,stroke-width:2px;

    class IntentParser,GuideGen llm;
    class SQS,GraphStore,TTL,SubgraphContext semantic;
    class Orchestrator,DiffEngine,Solver,Ranker,Account engine;
    class User,PydanticIntent,PydanticGuide,UserMarkdown output;
```

---

## 2. End-to-End Sequence & Interaction Flow

```mermaid
sequenceDiagram
    autonumber
    actor Player as 👤 Player
    participant Orch as 🎛️ Orchestrator<br/>(gw2-priory-def)
    participant TopLLM as 🧠 Top LLM<br/>(IntentParser)
    participant SQS as 🔮 SemanticQueryService<br/>(engine/semantic_query.py)
    participant KG as 📚 gw2-priory-ref & Triples<br/>(vocab/*.ttl & instances/*.ttl)
    participant Diff as ⚙️ AccountDiffEngine<br/>(engine/account_diff.py)
    participant Solver as ⏱️ PathSolver<br/>(engine/path_solver.py)
    participant BotLLM as ✍️ Bottom LLM<br/>(GuideGenerator)

    Player->>Orch: "I want to craft 2 legendary sigils tonight, have 90 mins, no WvW"
    
    %% Step 1: Top LLM Intent Parsing
    Orch->>TopLLM: user_prompt + Pydantic schema (PlayerGoalIntent)
    TopLLM-->>Orch: PlayerGoalIntent(target="legendary sigils", qty=2, time=90, excluded=["WvW"])

    %% Step 2: Semantic Entity Resolution
    Orch->>SQS: resolve_entity_by_text("legendary sigils")
    SQS->>KG: SPARQL: Match rdfs:label & skos:altLabel + stemming ("sigil")
    KG-->>SQS: Return item:91505 (Legendary Sigil)
    SQS-->>Orch: ResolvedGoal(item_id=91505, name="Legendary Sigil", qty=2)

    %% Step 3: Recipe DAG Traversal & Account Diffing
    Orch->>Diff: compute_diff(goal_item_id=91505, target_quantity=2, account_state)
    Diff->>KG: SPARQL: Recursive producedBy, hasIngredient, requiresItem, unpacksInto
    KG-->>Diff: Full ingredient DAG & discipline requirements
    Note over Diff: Deducts player's Material Storage (20 Clovers owned)<br/>Checks Wallet (440 Provisioner Tokens owned)<br/>Checks Bank for unpackable containers
    Diff-->>Orch: AccountDiffReport(missing: 40 Clovers, 1500 Lucent Crystals, Gift of Craftsmanship SATISFIED)

    %% Step 4: Multi-Criteria Route Optimization
    Orch->>Solver: solve_optimal_path(diff_report, time_budget=90, excluded=["WvW"])
    Solver->>KG: SPARQL: Discover substitute sources, daily time-gates & vendor currencies
    KG-->>Solver: Alternative routes (Wizard's Vault, Fractals, Vendor Exchanges)
    Note over Solver: Filters out WvW reward tracks<br/>Calculates currency conversions (Volatile Magic ➔ T6)<br/>Projects calendar time-gates
    Solver-->>Orch: OptimalCraftingPlan(checklist, step-by-step roadmap, time_gate_days=0)

    %% Step 5: Subgraph Context Extraction
    Orch->>SQS: get_item_semantic_context_for_llm(91505)
    SQS->>KG: SPARQL: Extract spatial waypoints, NPC names, and direct recipe facts
    KG-->>SQS: Subgraph triples (Waypoint: [&BPwCAAA=], NPC: Miyani)
    SQS-->>Orch: Grounded semantic markdown context

    %% Step 6: Bottom LLM Guide Generation
    Orch->>BotLLM: Grounded Facts + Optimal Plan + Pydantic schema (PersonalizedGuide)
    BotLLM-->>Orch: Structured JSON (PersonalizedGuide)
    
    %% Step 7: Output
    Orch->>Player: Rendered Markdown Checklist & Waypoint Navigation
```

---

## 3. The Three Touchpoints of the Semantic Web Layer

```mermaid
flowchart LR
    subgraph T1["Touchpoint 1: Semantic Resolution"]
        direction TB
        A1["Fuzzy Natural Language\n('leggy sigil', 'gen 2 staff')"] --> A2["SKOS Taxonomy & altLabel Match\n(priory_ref:weapon, priory_ref:rarity)"]
        A2 --> A3["Canonical Entity IRI\n(<https://priory.gw2/id/item/91505>)"]
    end

    subgraph T2["Touchpoint 2: Graph Reasoning & Math Grounding"]
        direction TB
        B1["Canonical Entity IRI"] --> B2["SPARQL Recipe DAG & Vendor Traversal\n(priory:producedBy, priory:hasSubstituteSource)"]
        B2 --> B3["Deterministic Math Engine\n(AccountDiffEngine & PathSolver)"]
    end

    subgraph T3["Touchpoint 3: Spatial & Factual Serialization"]
        direction TB
        C1["Engine Solution & Item IDs"] --> C2["SPARQL Spatial & NPC Extraction\n(priory:nearestWaypoint, priory:vendorNPC)"]
        C2 --> C3["Factual Prompt Context for Bottom LLM\n(Zero hallucinated waypoints or costs)"]
    end

    T1 --> T2 --> T3

    classDef box fill:#fafafa,stroke:#616161,stroke-width:1px;
    class T1,T2,T3 box;
```

---

## 4. Role of `gw2-priory-ref` in the Architecture

The `gw2-priory-ref` repository provides the foundational **SKOS Controlled Vocabularies** (`vocab/*.ttl`) that empower:
1. **Taxonomic Subsumption**: Enables hierarchical query resolution (e.g. `weapon:TwoHandedWeapon` subsuming `weapon:Greatsword`, `weapon:Staff`, `weapon:Spear`).
2. **Controlled Property Domains/Ranges**: Standardized URIs for `rarity:`, `discipline:`, `currency:`, `gamemode:`, and `weapon:` schemes.
3. **Synonym Matching**: `skos:altLabel` triples provide domain-accurate naming aliases without hardcoding synonyms in Python code.
