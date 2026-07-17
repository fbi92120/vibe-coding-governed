# BACKLOG — Global
Version : 1.10
Date : 2026-07-17 12:05

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

### Enrichissement continu des documents de contexte outils (Instructions Claude.ai + CLAUDE.md projet)
**Projets** : vibe-coding-governed (méthode)
**Date** : 2026-07-10 (initialement pisté 2026-05-10 pour les seules Instructions Claude.ai — étendu 2026-07-10 aux CLAUDE.md projet)
**Description** : les documents qui configurent les outils LLM au 
démarrage d'une session (Instructions du projet Claude.ai lues par 
Claude.ai, CLAUDE.md du projet lu par Claude Code) sont généralement 
rédigés au moment de la création du projet, puis laissés en l'état. 
Une première prise de conscience du 2026-05-10 pistait ce gap pour 
les Instructions Claude.ai suite à la session de bascule 
vibe-coding-governed → press-knowledge-extractor. Deux observations 
complémentaires du 2026-07-10 étendent ce gap.

Premièrement, le même principe s'applique aux CLAUDE.md des projets. 
Le projet llm-lab a fonctionné plusieurs semaines sans CLAUDE.md 
local du tout — malgré l'existence des CLAUDE.global.md et 
CLAUDE.projects.md, Claude Code n'avait aucune règle spécifique au 
projet, aucun contexte technique stable (VRAM, empreintes modèles, 
séquence de démarrage Ollama). Résultat : à chaque session, 
re-diagnostic partiel des mêmes faits, re-découverte des mêmes 
gotchas. Prise de conscience déclenchée par François pendant la 
session de rédaction du CLAUDE.md llm-lab initial.

Deuxièmement, ce ne sont pas seulement les Instructions qui doivent 
être enrichies au fur et à mesure — c'est aussi le CLAUDE.md du 
projet. Les deux documents sont symétriques (contexte pour l'outil 
au démarrage) mais destinés à deux outils différents (Claude.ai et 
Claude Code). Toute observation acquise en session qui a une valeur 
"au démarrage de la prochaine session" doit potentiellement enrichir 
l'un des deux, voire les deux.

Le pattern global émergent : les documents de contexte outil sont 
des artefacts qui évoluent au fil des sessions, pas des fichiers 
figés à l'ouverture du projet.

**Action** : dans la V7.5, ajouter une sous-section explicite dans 
la section 5 (Configurer le projet Claude.ai) — ou nouvelle section 
5 bis — qui cadre l'enrichissement continu des documents de contexte 
outil. Quatre éléments à couvrir :
1. La règle : toute observation acquise en session qui a valeur 
"au démarrage de la prochaine session" doit être proposée à l'humain 
pour enrichissement des Instructions Claude.ai et/ou du CLAUDE.md du 
projet.
2. La distinction : quelle information va dans les Instructions 
(destinée à Claude.ai — posture, arbitrage, signaux) vs dans le 
CLAUDE.md (destinée à Claude Code — règles opérationnelles, 
contexte technique, structure du projet, séquences de démarrage).
3. Le rythme : pas à chaque session mais quand un fait devient 
stable ou qu'un pattern se répète. Ni trop tôt (bruit) ni trop tard 
(dette de contexte).
4. Formulation du pattern à trancher en V7.5 : "documents de contexte 
outil = artefacts vivants" comme principe méthodologique autonome, 
ou rapprochement concret avec la discipline d'enrichissement 
d'EXPERIMENTS.md et BACKLOG.md ? Question ouverte, à discuter au 
démarrage de la session V7.5.

Documenter aussi qu'un projet peut démarrer sans CLAUDE.md local 
mais qu'il faut le créer dès qu'il y a plus d'un fait stable à 
transmettre à Claude Code — sans quoi ce dernier redécouvre à 
chaque session.

**Source** : première prise de conscience session vibe-coding-governed 
— 2026-05-10 (Instructions Claude.ai seules) ; extension session 
llm-lab — 2026-07-10 (ajout dimension CLAUDE.md projet, création du 
CLAUDE.md llm-lab initial)
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

### Distinguer 3 natures de projet dans l'Annexe D — production / lab / hybride
**Projets** : vibe-coding-governed (méthode)
**Date** : 2026-07-09
**Description** : L'Annexe D de METHODE V7.4 est taillée uniquement
  pour les projets de production d'outil (11 questions, SPECS.md,
  BACKLOG, tests de contrat, séquence prompt-module-test). Elle ne
  distingue pas les autres natures de projet, notamment les labs
  d'apprentissage (livrable = compétence acquise, pas outil livrable)
  et les projets hybrides (lab qui produit progressivement des briques
  réutilisables). Appliquée à un lab, l'Annexe D produit de la friction
  inutile : chaque manip passerait par un cycle 11 questions → SPECS.md,
  soit contournement du prompt, soit ralentissement de l'apprentissage.
**Cas déclencheur** : projet llm-lab (2026-07-09) — session Cockpit
  a signalé le mismatch entre l'Annexe D standard et la nature hybride
  de llm-lab. Un draft d'instructions hybrides a été produit pour
  llm-lab, à capitaliser dans la V7.5 de METHODE.
**Action** : dans la V7.5, restructurer l'Annexe D en 3 variantes ou
  en une seule Annexe D modulaire avec sections optionnelles selon
  la nature du projet :
  1. Projet de production d'outil (Annexe D actuelle)
  2. Projet de lab d'apprentissage (EXPERIMENTS.md comme livrable
     central, pas de SPECS.md, signaux de bascule vers mode spec)
  3. Projet hybride (mode lab par défaut, bascule vers mode spec
     quand une brique se stabilise)
  Documenter les signaux de bascule (🔧 STABILISATION DÉTECTÉE,
  🏗️ PORTÉE ÉLARGIE, 📤 CAPITALISATION EXTERNE).
  Documenter la nuance "DÉCISION MANQUANTE" (mode lab) vs "SPEC
  MANQUANTE" (mode spec).
**Référence** : draft d'instructions hybrides produit dans le projet
  Claude.ai llm-lab au 2026-07-09.
**Source** : session Cockpit + session préparatoire llm-lab (archive
  YT Extractor) 2026-07-09
**Statut** : ouvert

### Récupération de contexte d'un projet Claude.ai vers un autre — pattern absent
**Projets** : vibe-coding-governed (méthode) + tout projet dépendant
  d'un autre projet Claude.ai
**Date** : 2026-07-09
**Description** : quand un projet aval (ex. llm-lab) a besoin
  d'informations d'un projet amont (ex. yt-knowledge-extractor)
  — prompt système, exemple d'output, décision de spec —
  il n'existe pas de mécanisme pour que Claude.ai du projet aval
  accède au contexte du projet amont. Les projets Claude.ai sont
  cloisonnés. Le seul canal actuel est l'humain qui fait du
  copier-coller manuel. Observé sur llm-lab qui a besoin du prompt
  système de yt-knowledge-extractor pour tester la latence Ollama
  sur un vrai transcript.
**Action** : documenter le pattern dans METHODE_SPECS_CO-CONSTRUCTION.md
  (probablement section 5 ou 7) : quand un projet aval dépend
  de matière d'un projet amont, identifier au démarrage les
  artefacts à rapatrier (prompt système, exemples, extraits SPECS)
  et les joindre au projet aval comme documents figés. Ne pas
  tenter de "demander à l'autre projet" en cours de session.
**Source** : session llm-lab — 2026-07-09
**Statut** : ouvert

### Défauts silencieux des providers LLM — risque méthode transverse
**Projets** : llm-lab + tout projet utilisant un provider LLM
**Date** : 2026-07-09
**Description** : les providers LLM peuvent avoir des valeurs par défaut
  qui altèrent silencieusement le comportement observé, sans erreur ni
  avertissement. Observé sur Ollama v0.31.1 : le paramètre num_ctx par
  défaut est ~2 048 tokens, indépendamment de la capacité réelle du
  modèle (Qwen 3 8B affiche pourtant 40 960 tokens dans `ollama show`).
  Un prompt de 16 000 tokens envoyé sans num_ctx explicite est tronqué
  à ~2 050 tokens, la requête retourne done_reason "stop" et une
  sortie apparemment cohérente — mais totalement déconnectée du prompt
  réel. Aucun signal d'erreur.
  Ce n'est probablement pas propre à Ollama : tout provider peut avoir
  des défauts silencieux similaires (max_tokens, temperature, top_p,
  timeout, contexte glissant). Un benchmark ou une comparaison
  cross-modèles peut être invalidé sans que l'expérimentateur le
  détecte.
**Action** : intégrer dans METHODE_SPECS_CO-CONSTRUCTION.md (étape 4
  "Forcer chaque décision à être nommée" ou nouvelle sous-section
  dédiée aux providers LLM) le principe suivant : tout paramètre
  critique d'un provider (num_ctx, max_tokens, temperature, etc.)
  doit être passé explicitement, jamais laissé au défaut. La règle
  s'applique aussi bien à un test qu'à un module de production.
  Corollaire dans la constitution de tout projet LLM : "ne jamais
  compter sur les valeurs par défaut d'un provider — les nommer
  toutes explicitement."
**Source** : session llm-lab — 2026-07-09 — premier bench Qwen 3 8B
  sur cas YT Extractor
**Statut** : ouvert

### Défauts de transcription des notes techniques
**Projets** : llm-lab + tout projet documentant des faits techniques dérivés
  d'observations (logs, sorties de commande, specs de provider)
**Date** : 2026-07-15
**Description** : distinct du gap "Défauts silencieux des providers LLM"
  (qui porte sur le comportement runtime), ce gap porte sur la
  documentation : un fait technique recopié de mémoire dans une note,
  un script ou une Instruction peut être faux, et n'est rattrapé qu'en
  le relisant contre la sortie réelle. Une même session (préparation
  Expé 3, 2026-07-15) a débusqué trois faits faux câblés ou sur le point
  de l'être :
  1. `--cache-ram 0` présenté comme flag d'`ollama serve` — inexistant
     (c'est un flag de llama-server/llama.cpp). Rattrapé par un doute
     avant câblage, confirmé en terminal.
  2. "Llama 3.3 8B" (LLM_LAB_DECISIONS_PREALABLES.md) — ce modèle
     n'existe pas ; Llama 3.3 démarre à 70B, le 8B réel est Llama 3.1 8B.
     Rattrapé par web search avant tout pull.
  3. Ancre de log "KV self size" (CLAUDE.md) — chaîne qui n'apparaît
     jamais dans les logs d'Ollama 0.31.1 ; la vraie ligne est
     `llama_kv_cache: size = … K (<type>) … V (<type>)`. A fait avorter
     les 3 runs de la première tentative sur un faux négatif du garde-fou
     (timeout en attente d'une ligne fantôme), pas sur un vrai problème.
  Observation qui affine le gap : sur le cas 3, EXPERIMENTS.md contenait
  déjà la bonne ligne. Le journal était juste ; c'est la note
  opérationnelle dérivée (dans le CLAUDE.md) qui était fausse. La dérive
  naît au moment de recopier un fait du journal vers une note d'action,
  pas dans le journal lui-même.
**Action** : intégrer dans METHODE_SPECS_CO-CONSTRUCTION.md un principe
  complémentaire du gap "Défauts silencieux" : une note technique (chaîne
  de log, nom de modèle, flag, valeur de config) se relit dans la sortie
  réelle AVANT d'être câblée dans un script ou une Instruction — jamais
  recopiée de mémoire. Point de vigilance spécifique : la recopie
  journal → note d'action est le maillon fragile. Corollaire outillage :
  un garde-fou qui attend un motif (ligne de log, etc.) doit être testé
  contre une sortie réelle, sinon il avorte sur le motif faux et non sur
  la condition qu'il est censé vérifier.
**Source** : session llm-lab — 2026-07-15 — préparation Expé 3 (voir
  entrée de cadrage Expé 3 dans EXPERIMENTS.md pour le détail)
**Statut** : ouvert

### Répartition Claude.ai / Claude Code sur les phases de diagnostic
    système en mode lab
**Projets** : vibe-coding-governed (méthode) + tout projet en mode lab
**Date** : 2026-07-10
**Description** : METHODE_SPECS_CO-CONSTRUCTION.md V7.4 cadre la
  répartition Claude.ai / Claude Code sur les phases classiques d'un
  projet (co-construction des specs, implémentation, arbitrage de
  divergence). Elle ne cadre pas la répartition sur les phases de
  diagnostic système préalables à une expérimentation en mode lab
  (kill de process, configuration d'environnement, lecture de logs,
  inspection d'état runtime).
  Cas déclencheur : session llm-lab 2026-07-10 — cadrage Expé 3
  (fit RAM Qwen 3 14B sur M5 16 Go, effectivité KV cache Q8_0). Le
  diagnostic a demandé 15+ commandes terminal en aller-retour avec
  Claude.ai (identification que le service Ollama tournait via Homebrew,
  arrêt propre via `brew services stop`, positionnement d'env vars,
  relance avec redirection de logs, inspection ligne par ligne). Chaque
  commande transitait par copier-coller vers le terminal puis retour de
  sortie vers Claude.ai. Perte de temps significative et fragilité
  (apostrophes françaises dans les commentaires shell provoquant des
  parse errors répétés).
  Bilan pratique : le diagnostic terminal pur est mieux confié à
  Claude Code, qui accède directement au terminal. L'interprétation des
  résultats (voie A/B/C, décision de poursuivre ou pivoter) reste chez
  Claude.ai. Cette répartition n'est pas triviale : ce n'est ni
  "Claude.ai cadre / Claude Code implémente" (répartition production),
  ni "Claude Code fait tout" (risque de dérive méthodologique).
**Action** : dans la V7.5, ajouter une section dans l'Annexe D variante
  lab qui cadre la répartition sur les phases de diagnostic système :
  1. Diagnostic terminal pur (état système, configuration, logs)
     → Claude Code
  2. Interprétation des résultats et arbitrage des voies → Claude.ai
  3. Documentation des découvertes dans EXPERIMENTS.md → Claude Code
     sur cadrage Claude.ai
  Documenter aussi le pattern "Claude.ai produit un plan de diagnostic
  à faire exécuter par Claude Code" comme forme légitime de délégation
  en mode lab, distincte de la co-construction classique.
**Source** : session llm-lab — 2026-07-10 — cadrage Expé 3
**Statut** : ouvert

### Ressources annoncées d'un composant local ≠ ressources utilisables
    en pratique
**Projets** : llm-lab + tout projet utilisant un modèle LLM local
**Date** : 2026-07-10
**Description** : la fenêtre native annoncée par un modèle (ex. Qwen 3 8B
  annonce 128K, `ollama show` affiche 40 960 tokens) ne correspond pas
  à la fenêtre utilisable sur une machine donnée. Sur Mac M5 16 Go RAM,
  la fenêtre utile pratique de Qwen 3 8B Q4_K_M est plafonnée à ~16K
  par la contrainte RAM (le KV cache d'un num_ctx élevé consomme
  beaucoup à côté des 5,2 Go du modèle). Passer à 32K provoquerait
  probablement un swap et effondrerait la latence.
  Ce plafond n'est pas documenté par le provider et ne fait l'objet
  d'aucun warning : le modèle accepte silencieusement n'importe quel
  num_ctx, la limite se manifeste par une dégradation runtime.
  Corollaire équivalent sur la VRAM machine, observé sur la même
  session : Mac M5 16 Go annonce 16 Go de mémoire unifiée, mais Ollama
  expose 11.8 GiB de VRAM disponible pour l'inférence GPU (mesuré via
  log `inference compute` au démarrage). La réservation macOS pour l'OS,
  l'affichage et les buffers noyau n'est pas récupérable en fermant des
  applications (Chrome, Slack, etc.) : elle est structurelle, pas
  dynamique en fonction de l'occupation applicative. Le budget VRAM
  utilisable pour l'inférence est donc un plafond dur inférieur à la RAM
  machine annoncée. Sur cette M5 en Ollama 0.31.1 backend Metal :
  11.8 GB. À dimensionner comme tel dans tout cadrage de fit modèle +
  KV cache.
  Corollaire du gap "Défauts silencieux des providers LLM" mais distinct :
  ici la contrainte est hardware, pas paramétrique.
**Action** : intégrer dans METHODE_SPECS_CO-CONSTRUCTION.md, en corollaire
  de la règle sur les défauts silencieux : pour tout modèle local, la
  fenêtre pratique par machine doit être mesurée expérimentalement
  (num_ctx maximal sans dégradation runtime), documentée, et distinguée
  de la fenêtre annoncée par le modèle. La fenêtre annoncée ne suffit
  pas au dimensionnement d'un cas d'usage.
  Étendre au dimensionnement VRAM : pour toute machine locale, la VRAM
  allouable à l'inférence doit être mesurée expérimentalement (via log
  du serveur d'inférence), documentée, et distinguée de la RAM machine
  annoncée. Fermer des applications utilisateur ne récupère pas cette
  VRAM.
**Source** : session llm-lab — 2026-07-10 — analyse post-run Qwen 3 8B
  Q4 sur Mac M5 16 Go
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
