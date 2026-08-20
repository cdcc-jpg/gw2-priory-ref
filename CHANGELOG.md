# Changelog - gw2-priory-ref

All notable changes to **gw2-priory-ref** (Controlled Vocabularies & Taxonomies) will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Added
- Added comprehensive cross-repository architectural reference in [`docs/neuro_symbolic_architecture_and_pipeline.md`](./docs/neuro_symbolic_architecture_and_pipeline.md) detailing how SKOS concept schemes interface with the Project Priory neuro-symbolic reasoning engine.
- Integrated architecture interaction diagram and documentation link into [`README.md`](./README.md).
- Initialized repository with AGENTS.md, README.md, and changelog.
- Created SKOS Concept Schemes:
  - `vocab/rarities.ttl`: Item rarities (Junk to Legendary) with numerical tier ranks and color codes.
  - `vocab/disciplines.ttl`: Crafting disciplines (Weaponsmith, Armorsmith, Artificer, etc.) with max ratings.
  - `vocab/weapon_types.ttl`: Comprehensive weapon hierarchy (One-Handed, Two-Handed, Off-Hand, Aquatic).
  - `vocab/game_modes.ttl`: Game mode taxonomies (PvE Open World, Fractals, Raids, Strikes, WvW, PvP).
  - `vocab/currencies.ttl`: Game currencies and exchange tokens (Gold, Karma, Spirit Shards, Astral Acclaim, etc.).
