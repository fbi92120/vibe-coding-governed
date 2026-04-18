# BACKLOG — Projects
Version : 1.2
Date : 2026-04-18 18:38

*Dépendances inter-projets et priorisation cross-projets.
Source de vérité pour l'arbitrage portfolio.*

---

## Dépendances inter-projets

### DEP-01 — YT Extractor V1.1 → Wiki LLM corrections
**Projets** : yt-extractor → wiki-llm
**Description** : les corrections WL-02 (reader.py) et WL-03 (template source)
dépendent du format de fiche YT Extractor. Si V1.1 modifie le format des
sections (chapitrage inféré, formulations notables, etc.), les corrections
wiki-llm doivent s'adapter.
**Séquençage** : YT Extractor V1.1 d'abord, puis wiki-llm WL-02/WL-03
**Source** : session BACKLOG wiki-llm — 2026-04-15
**Statut** : résolu — 2026-04-18 18:38 — format de fiche stable en V1.7,
  aucune modification structurelle. WL-02/WL-03 peuvent avancer sans rework.

### DEP-02 — Export Evernote → Wiki LLM Workflow B
**Projets** : export-evernote → wiki-llm
**Description** : le Workflow B (WL-20) ne peut pas être spécifié tant que
la migration Evernote n'est pas terminée — la structure des notes exportées
détermine le reader et le template source du Workflow B.
**Séquençage** : Export Evernote terminé, puis co-construction WL-20
**Source** : decisions.md V2-1
**Statut** : bloqué (migration non commencée)

### DEP-04 — Principe orchestrateur passif (yt-extractor + wiki-llm)
**Projets** : yt-extractor + wiki-llm
**Date** : 2026-04-18 18:38
**Description** : les deux projets appliquent le principe "aucune logique métier
  dans l'orchestrateur" (extract.py / main.py). Violation détectée dans
  yt-extractor : _extract_model_from_fiche() placée dans extract.py au lieu
  de src/writer.py. Le pattern doit être appliqué de façon cohérente dans
  les deux projets.
**Séquençage** : corriger dans yt-extractor (déjà noté BACKLOG),
  vérifier dans wiki-llm
**Source** : BACKLOG yt-knowledge-extractor — 2026-04-18
**Statut** : ouvert

### DEP-03 — Wiki LLM Phase 2 → LLMProvider partagé
**Projets** : wiki-llm + yt-extractor
**Description** : les deux projets utilisent le même pattern `LLMProvider`
avec les mêmes providers (Ollama, Gemini, Haiku). Toute évolution du
pattern dans un projet devrait être propagée à l'autre. Pas de librairie
partagée au MVP — duplication acceptée, synchronisation manuelle.
**Source** : session BACKLOG wiki-llm — 2026-04-15
**Statut** : surveillé

---

## Priorisation cross-projets

### Séquençage validé (avril 2026)

```
1. Wiki LLM — corrections WL-01 à WL-05
2. YT Extractor V1.1 (extraction/inference separation)
3. Wiki LLM — Phase 2 (synthèse LLM, WL-10 à WL-13)
```

**Justification** : WL-01 (bug R11) et WL-04 (questions) n'ont pas de
dépendance YT Extractor — ils peuvent être faits maintenant. WL-02 et
WL-03 dépendent du format V1.1 (DEP-01), mais le format actuel est déjà
diagnostiqué comme compatible (DIAGNOSTIC_2026-04-11). Le risque de
changement cassant en V1.1 est faible.

**Alternative** : si V1.1 change le format des fiches, revenir sur
WL-02/WL-03 après V1.1. Le coût de rework est faible (un module, un test).
