# Changelog - gw2-priory-ref

All notable changes to **gw2-priory-ref** (Controlled Vocabularies & Taxonomies) will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Added
- **Multi-Agent Orchestration & Subagent Delegation Protocol in `AGENTS.md`:** Formalized the architecture rule instructing the primary agent to operate as Lead Architect/Orchestrator, decomposing complex taxonomy and vocab workstreams across specialized subagents.
- **Mount & Mobility Taxonomy in `vocab/mounts.ttl`:** Formalized `mount:MountScheme` SKOS Concept Scheme modeling all 10 mounts (Raptor, Springer, Skimmer, Jackal, Griffon, Roller Beetle, Skyscale, Warclaw, Siege Turtle, Jade Bot) with mobility profiles, traversal definitions, and expansion requirements.
- **Core Tyria Regions & Exploration Zones Taxonomy in `vocab/core_tyria_regions.ttl`:** Formalized `region:CoreTyriaRegionScheme` SKOS Concept Scheme covering all 5 Core Tyria regions (Kryta, Ascalon, Shiverpeaks, Maguuma Jungle, Ruins of Orr) and 29 exploration zones with level requirements, difficulty ratings, starting waypoints, and mount recommendations.
- **Project Scope & Architecture Definition in `README.md`:** Added standardized multi-repo architectural definition clarifying the division of labor between `gw2-priory-ref` (Controlled Vocabulary & Reference Taxonomy Layer: SKOS), `gw2-priory-def` (Domain Ontology & Schema Layer: OWL 2 DL TBox + SHACL), and the Engine (Deterministic Graph Solver & Neuro-Symbolic Agent Pipeline).
- Added comprehensive cross-repository architectural reference in [`docs/neuro_symbolic_architecture_and_pipeline.md`](./docs/neuro_symbolic_architecture_and_pipeline.md) detailing how SKOS concept schemes interface with the Project Priory neuro-symbolic reasoning engine.
- Integrated architecture interaction diagram and documentation link into [`README.md`](./README.md).
- Initialized repository with AGENTS.md, README.md, and changelog.
- Created SKOS Concept Schemes:
  - `vocab/rarities.ttl`: Item rarities (Junk to Legendary) with numerical tier ranks and color codes.
  - `vocab/disciplines.ttl`: Crafting disciplines (Weaponsmith, Armorsmith, Artificer, etc.) with max ratings.
  - `vocab/weapon_types.ttl`: Comprehensive weapon hierarchy (One-Handed, Two-Handed, Off-Hand, Aquatic).
  - `vocab/game_modes.ttl`: Game mode taxonomies (PvE Open World, Fractals, Raids, Strikes, WvW, PvP).
  - `vocab/currencies.ttl`: Game currencies and exchange tokens (Gold, Karma, Spirit Shards, Astral Acclaim, etc.).
