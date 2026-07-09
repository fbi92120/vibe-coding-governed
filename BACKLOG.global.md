# BACKLOG — Global
Version : 1.4
Date : 2026-07-09 10:30

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
- Évaluer Opus 4.6 vs 4.7 sur une étape de co-construction réelle —
  hypothèse à tester avec PASSATION_ETAPE_1.md comme contexte identique

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

### Distinction "Instructions du projet" vs "Documents joints" absente de METHODE
**Projets** : vibe-coding-governed (méthode)
**Date** : 2026-05-10 19:15
**Description** : la section 5 de METHODE.md et l'Annexe D ne précisent pas
  explicitement la distinction critique entre le champ "Instructions du
  projet" (lu automatiquement par Claude.ai à chaque message) et les
  "documents joints au projet" (lus sur déclenchement uniquement).
  Conséquence : un utilisateur peut joindre METHODE.md comme document et
  croire que l'Annexe D s'applique automatiquement — ce n'est pas le cas.
  Sans le champ Instructions correctement configuré, le comportement du
  co-constructeur n'est pas garanti.
**Action** : ajouter dans la section 5 (sous-section "Configurer le projet
  Claude.ai") un encart explicite sur cette distinction, avec exemple
  concret. Clarifier dans l'Annexe D que son contenu est destiné aux
  Instructions, pas aux Documents.
**Source** : session vibe-coding-governed — 2026-05-10 — bascule vers
  press-knowledge-extractor
**Statut** : ouvert

### Qui rédige les instructions du projet Claude.ai — implicite
**Projets** : vibe-coding-governed (méthode)
**Date** : 2026-05-10 19:15
**Description** : la section 5 mentionne "Configurer le projet Claude.ai"
  avec le contenu à mettre dans les instructions, mais ne dit jamais
  explicitement que c'est l'humain qui rédige et colle les instructions
  au moment de la création du projet (Claude.ai ne peut pas écrire dans
  ce champ — c'est une configuration d'interface utilisateur).
  Implicite pour qui connaît l'outil, gap pour qui découvre la méthode.
**Action** : ajouter une ligne explicite dans la section 5 :
  "Les instructions du projet sont rédigées et collées par l'humain
  au moment de la création du projet Claude.ai. Elles ne sont pas
  modifiables par Claude.ai en cours de session."
**Source** : session vibe-coding-governed — 2026-05-10
**Statut** : ouvert

### Enrichir les instructions du projet avec les apprentissages de session
**Projets** : vibe-coding-governed (méthode)
**Date** : 2026-05-10 19:15
**Description** : bonne pratique observée pendant la bascule
  vibe-coding-governed → press-knowledge-extractor. Les instructions du
  nouveau projet ont été enrichies de 3 sections au-delà de l'Annexe D
  standard : (1) périmètre projet, (2) état du projet à l'ouverture avec
  les décisions de l'étape 1, (3) "Vigilance spécifique à ce projet"
  avec les patterns observés en cours de session.
  Cette pratique n'est pas documentée dans METHODE.md — elle devrait l'être
  car elle évite de perdre la mémoire opérationnelle entre les sessions
  successives d'un même projet.
**Action** : dans la section 5 (Configurer le projet Claude.ai), ajouter
  une sous-section "Enrichir les instructions au fur et à mesure" avec
  la liste des sections optionnelles à ajouter par-dessus l'Annexe D.
**Source** : session vibe-coding-governed — 2026-05-10
**Statut** : ouvert

### Pattern "saut prématuré vers l'architecture" — signal à reconnaître
**Projets** : vibe-coding-governed (méthode)
**Date** : 2026-05-10 19:15
**Description** : pattern récurrent observé 3 fois pendant l'étape 1 de
  press-knowledge-extractor — l'auteur répond par la solution technique
  (microservices, agentique, fonctions par site, multi-sites v1) alors
  que la question portait sur la dissociation des problèmes (étape 1)
  ou la hiérarchisation des usages (étape 2). Pattern probablement
  transverse à tout projet, surtout quand l'auteur a un instinct
  architectural fort.
**Action** : ajouter dans l'étape 7 (zones grises / audit pré-rédaction)
  ou dans une nouvelle sous-section de l'étape 1, une mention de ce
  signal et la posture à tenir : renvoyer systématiquement à la question
  de méthode, ne pas enregistrer la réponse technique comme acquise.
**Source** : session vibe-coding-governed — 2026-05-10
**Statut** : ouvert

### Reformulation des questions abstraites en scénarios observables
**Projets** : vibe-coding-governed (méthode)
**Date** : 2026-05-10 19:15
**Description** : pattern à capitaliser observé en étape 1 de
  press-knowledge-extractor. La question abstraite "consommation Obsidian
  vs corpus IA, lequel domine ?" (formulée en options A/B/C/D) n'a pas
  obtenu de réponse claire au 1er et 2ème tour. La même question
  reformulée en scénario observable ("imagine 3 mois plus tard,
  200 articles dans le vault, que fais-tu avec ?") a obtenu une réponse
  immédiate et nette. Pattern probablement transverse — quand une
  question revient en réponse technique, c'est souvent parce qu'elle
  était trop conceptuelle pour être ancrée.
**Action** : ajouter dans l'Annexe D du prompt de co-construction une
  consigne explicite : "Si une question reste sans réponse claire ou
  revient en réponse technique, la reformuler en scénario d'usage
  observable plutôt qu'en options catégorielles."
**Source** : session vibe-coding-governed — 2026-05-10
**Statut** : ouvert

### V7.5 METHODE_SPECS_CO-CONSTRUCTION — session à ouvrir
**Projets** : vibe-coding-governed (méthode)
**Date** : 2026-07-09
**Description** : le projet enex2obsidian a produit 7 observations
  méthodologiques cristallisées dans LEÇONS-METHODE-ENEX2OBSIDIAN.md V1.0
  (2026-06-30). Quatre items sont classés prêts à intégrer, trois sont
  classés à mûrir. Les quatre prêts : (1) récap structuré pré-commit
  obligatoire, (2) protocole "divergence code vs SPECS révélée par un
  test", (3) validation à 3 étapes pour modules à interface format
  externe, (4) inspection visuelle obligatoire pour outputs consommés
  visuellement. Les trois à mûrir : (5) représentativité de l'échantillon
  empirique, (6) consultation documentaire des formats sources en
  cadrage, (7) cycles X-AUDIT / X-FIX comme pattern accepté (audit LLM
  tiers type Codex CLI).
**Point ouvert** : Item 7 — l'audit LLM tiers doit-il devenir standard
  sur tout projet avec Claude Code, ou rester spécifique aux projets à
  fort enjeu ? Décision possible sans attendre un second projet si la
  portée universelle est acceptée par principe. Si oui, la règle doit
  être transférée dans CLAUDE.global.md (pour lecture automatique par
  Claude Code) et dans l'Annexe D (pour cadrage Claude.ai), pas
  seulement dans METHODE V7.5.
**Action** : ouvrir une session Claude.ai dédiée V7.5 dans le projet
  vibe-coding-governed avec LEÇONS-METHODE-ENEX2OBSIDIAN.md V1.0 joint.
  Intégrer aussi les 6 gaps de méthode ci-dessus (issus de
  press-knowledge-extractor et session vibe-coding-governed 2026-05-10)
  qui n'ont pas encore été traités — la V7.5 traite donc 13 items
  accumulés depuis avril, pas seulement les 7 items enex2obsidian.
**Déclencheur suggéré** : après exécution de la migration réelle
  enex2obsidian (aucune contrainte forte, mais évite d'ouvrir deux
  chantiers méta en parallèle).
**Source** : LEÇONS-METHODE-ENEX2OBSIDIAN.md V1.0 + conversation
  Claude.ai enex2obsidian 2026-07-09
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
