# BACKLOG — Global
Version : 1.2
Date : 2026-04-18 18:38

---

## Idées brutes

*Une phrase. Pas encore qualifiées. L'arbitrage se fait en revue périodique.*

- yt-extractor + wiki-llm : support Batch API asynchrone
  (Gemini + Anthropic) pour volumes > 100 fiches
- LLMProvider comme module partagé (package Python interne ou sous-module git)
  au lieu de la duplication actuelle entre yt-extractor et wiki-llm
- Dashboard portfolio : un script qui agrège les backlogs des 3 projets
  et affiche l'état global (items ouverts, bloqués, dépendances)
- Template BACKLOG_projet.md dans vibe-coding-governed pour les futurs projets

---

## Évolutions planifiées

*Aucune pour l'instant.*

---

## Gaps de méthode

### Dette tests unitaires — modules en production non testés
**Projets** : yt-extractor (confirmé) + wiki-llm (à vérifier)
**Date** : 2026-04-18 18:38
**Description** : plusieurs modules en production sans tests unitaires
  (transcript.py, metadata.py, generator.py, validator.py, writer.py, llm/).
  Seul le smoke test couvre l'intégration. Pattern transverse à surveiller
  sur tout nouveau projet.
**Action** : session dédiée par projet — prioriser les modules à contrat
  fixe (validator, writer)
**Source** : BACKLOG yt-knowledge-extractor — 2026-04-18
**Statut** : ouvert

### Règles batch — convention transverse à formaliser
**Projets** : yt-extractor V2 + tout futur projet batch
**Date** : 2026-04-18 18:38
**Description** : règles batch (dry-run, archivage, log structuré, limite,
  idempotence, rapport de fin) définies empiriquement dans yt-extractor.
  Non formalisées dans la méthode — risque de réinvention à chaque projet.
**Action** : extraire les règles batch de yt-extractor V2 et les intégrer
  comme annexe dans METHODE_SPECS_CO-CONSTRUCTION
**Source** : BACKLOG vibe-coding-governed — 2026-04-15
**Statut** : ouvert

---

## Dépendances externes

### [JUIN 2026] Gemini 2.0 Flash-Lite déprécié
**Projet** : yt-extractor + wiki-llm
**Description** : Gemini 2.0 Flash-Lite déprécié — provider par défaut des deux projets
**Action requise** : migrer vers Gemini 2.5 Flash-Lite
**Impact wiki-llm** : WL-10 (LLMProvider) pointe déjà vers 2.5 Flash-Lite dans les specs — pas d'action si implémenté après juin
**Source** : session exécution BACKLOG V1.0 — 2026-04-15
**Statut** : surveillé

### [CONTINU] Python 3.9 sur Mac système
**Projet** : yt-extractor + wiki-llm
**Description** : syntaxe `str | None` (Python 3.10+) impose `from __future__ import annotations` partout
**Action requise** : migrer vers Python 3.12 (brew ou pyenv)
**Source** : retex wiki-llm + retex yt-extractor — même problème récurrent
**Statut** : accepté comme pattern en attendant migration
