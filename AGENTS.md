# Agent Rules & Contribution Guidelines - gw2-priory-ref

This repository hosts **Authoritative Controlled Vocabularies, Taxonomies, and Concept Schemes** for **Project Priory**.

---

## 1. Scope & Standards
* **SKOS Exclusivity:** All controlled vocabularies must use **W3C SKOS** (`skos:ConceptScheme`, `skos:Concept`, `skos:broader`, `skos:narrower`, `skos:prefLabel`, `skos:altLabel`, `skos:notation`).
* **Immutable URI Scheme:**
  * Namespace: `https://priory.gw2/ref/` (prefix: `priory-ref:`)
  * Sub-vocabularies: `https://priory.gw2/ref/rarity/`, `https://priory.gw2/ref/weapon/`, `https://priory.gw2/ref/discipline/`, `https://priory.gw2/ref/gamemode/`, `https://priory.gw2/ref/currency/`.
* **Cross-Source Alignment:** Where applicable, map concepts to external IDs using `skos:exactMatch` (GW2 Wiki URLs) and `skos:notation` (GW2 API IDs / Chat codes).
* **Maintain Changelog:** Record every vocabulary addition, modification, or deprecated concept in [`CHANGELOG.md`](./CHANGELOG.md).
