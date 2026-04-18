# BACKLOG — vibe-coding-governed
Version : 1.1
Date : 2026-04-18 17:46

---

## Idées brutes

*Une phrase. Pas encore qualifiées. L'arbitrage se fait en revue périodique.*

*Aucune pour l'instant.*

---

## Évolutions planifiées

### [V8] Repo GitHub public avec templates
**Projet** : vibe-coding-governed
**Version cible** : V8
**Problème** : Le lecteur n'a pas de point de départ actionnable — pas de templates prêts à cloner, pas de prompt installable
**Dépendances** : Annexe D finalisée
**Source** : Structure du Quick Start pour METHODE_SPECS_CO-CONSTRUCTION V7 — https://claude.ai/chat/de588463-74fe-4077-b41e-d2a3527a5511
**Statut** : problème défini

Contenu prévu :
- `METHODE_SPECS_CO-CONSTRUCTION.md`
- `CLAUDE.global.md` prêt à installer
- `templates/SPECS.md` — structure vide 5 blocs
- `templates/BACKLOG.md` — template Annexe C
- `templates/CLAUDE.md` — template projet
- `prompts/co-construction.md` — prompt Annexe D

### [V8] Séquences BACKLOG par type d'entrée
**Projet** : vibe-coding-governed
**Version cible** : V8
**Problème** : La séquence de 11 questions est conçue pour démarrer un projet. Quand on reprend une entrée BACKLOG (idée, bug, évolution, dépendance), la séquence n'est pas la même — chaque type a son propre flux de questions
**Dépendances** : aucune
**Source** : Structure du Quick Start pour METHODE_SPECS_CO-CONSTRUCTION V7 — https://claude.ai/chat/de588463-74fe-4077-b41e-d2a3527a5511
**Statut** : problème défini

À définir : questions spécifiques pour idée brute, évolution planifiée, bug, gap de spec, dépendance externe. À intégrer dans le prompt Annexe D et/ou section 7.

### [V8] Artefact compétences auteur
**Projet** : vibe-coding-governed
**Version cible** : V8
**Problème** : Les compétences démontrées par ce projet (co-construction specs, gouvernance LLM, vibe-coding gouverné) n'ont pas d'artefact dédié — pertinent pour GitHub profil et LinkedIn
**Dépendances** : METHODE V7 finalisée
**Source** : Structure du Quick Start pour METHODE_SPECS_CO-CONSTRUCTION V7 — https://claude.ai/chat/de588463-74fe-4077-b41e-d2a3527a5511
**Statut** : problème défini

---

## Dépendances externes

*Aucune pour l'instant. Voir BACKLOG.global.md.*

---

## Bugs connus

*Aucun pour l'instant.*

---

## Tests manquants

*Aucun pour l'instant.*

---

## Gaps de spec

### Règles batch — artefact à créer
**Projet** : yt-knowledge-extractor / vibe-coding-governed
**Source** : Structure du Quick Start pour METHODE_SPECS_CO-CONSTRUCTION V7 — https://claude.ai/chat/de588463-74fe-4077-b41e-d2a3527a5511
**Description** : Règles batch (dry-run, archivage, log structuré, limite, idempotence, rapport de fin) définies pour yt-knowledge-extractor V2 — transférables à tout projet batch mais hors périmètre de la méthode généraliste
**Action** : créer artefact dédié dans specs yt-knowledge-extractor V2 — session dédiée à planifier
**Statut** : ouvert

### REX à déplacer dans leurs repos respectifs
**Projet** : vibe-coding-governed / yt-knowledge-extractor / wiki-llm
**Source** : Structure du Quick Start pour METHODE_SPECS_CO-CONSTRUCTION V7 — https://claude.ai/chat/de588463-74fe-4077-b41e-d2a3527a5511
**Description** : YT_EXTRACTOR_RETOUR_EXPERIENCE.md → repo yt-knowledge-extractor. WIKI_LLM_RETOUR_EXPERIENCE.md → repo wiki-llm.
**Action** : déplacer les fichiers REX dans leurs repos respectifs
**Statut** : ouvert

### Section 8 supprimée — raison documentée dans README
**Projet** : vibe-coding-governed
**Source** : Structure du Quick Start pour METHODE_SPECS_CO-CONSTRUCTION V7 — https://claude.ai/chat/de588463-74fe-4077-b41e-d2a3527a5511
**Description** : Section 8 "Ce qui est systématiquement transférable" supprimée en V7 — contenu absorbé par Quick Start (section 2) et les 9 étapes (section 6).
**Action** : noter dans le README du repo
**Statut** : ouvert

### Prompt injection — entrée Glossaire ajoutée
**Projet** : vibe-coding-governed
**Source** : session exécution BACKLOG V1.0 — 2026-04-15
**Description** : "Prompt injection" ajouté au Glossaire (section 3). Renvoi depuis Annexe B section "Avant de coder".
**Action** : aucune — résolu
**Statut** : résolu

### Gouvernance des discussions — règle ajoutée Annexe D
**Projet** : vibe-coding-governed
**Source** : session exécution BACKLOG V1.0 — 2026-04-15
**Description** : Règle de gouvernance des discussions Claude.ai ajoutée en Annexe D — signal proactif, prompt de démarrage, gestion des versions au transfert.
**Action** : aucune — résolu
**Statut** : résolu
