# BACKLOG — Global
Version : 1.1
Date : 2026-04-15

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
