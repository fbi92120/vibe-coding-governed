# Méthode de co-construction des spécifications
## Vibe-coding gouverné : du code qui tient, sans patches

**Auteur** : François Biller
**Date** : 2026-04-15
**Version** : 7.1
**Repo** : https://github.com/fbi92120/vibe-coding-governed

---

## Sources & remerciements

Cette méthode n'est pas née dans le vide.

Les réflexions sur la gouvernance IA, la dette de contrôle, le rôle irremplaçable
de l'humain et la co-construction avec un LLM sont directement nourries par les
vidéos de la chaîne [Le SamourAI](https://www.youtube.com/@SamouraiDansant) et
par la formation accessible sur [Patreon](https://www.patreon.com/cw/SamouraiDansant)
et [surhumain.ai](https://surhumain.ai).

Cette méthode est ma façon de rendre visible ce que j'en ai appliqué.

---

## 1. Présentation

*Ce qu'un non-dev peut produire avec un LLM quand il gouverne au lieu de vibe-coder : du code qui tient, sans patches.*

Il existe deux façons de produire du code avec un LLM.

La première — le vibe-coding — consiste à donner une idée, obtenir du code qui tourne, et patcher quand ça ne marche pas comme prévu. C'est rapide au départ. C'est ingérable à mesure que le projet grossit.

La seconde — le vibe-coding gouverné — consiste à co-construire les specs avant d'écrire une ligne de code, à définir ce que le LLM ne doit pas faire, et à rester le décideur à chaque étape. Le code produit tient. Il n'accumule pas de dette silencieuse.

Ce document décrit la méthode qui distingue l'un de l'autre.

Elle s'adresse à deux profils :
- **Non-développeurs** qui produisent du code avec un LLM et veulent que ce code soit fiable, sans avoir à devenir développeurs.
- **Développeurs** qui utilisent un LLM dans leur workflow et veulent gouverner cet usage au lieu de le subir.

Dans les deux cas, le problème est le même : sans cadre, le LLM produit de l'apparence. Il génère du code techniquement correct pour le mauvais problème, des specs qui dérivent sans que personne ne mesure l'accumulation, des outputs qui *semblent* justes jusqu'à l'incident qui révèle qu'ils ne l'étaient pas.

La méthode repose sur un principe simple : deux outils, deux rôles distincts, un humain qui orchestre. Claude.ai pour co-construire et arbitrer. Claude Code pour implémenter. L'humain pour définir, juger, et décider — à chaque étape.

Ce document est le résultat de projets concrets développés entièrement selon cette méthode. Il ne décrit pas seulement ce qui a été décidé — il documente comment les décisions ont été prises, pourquoi certaines étapes auraient dû être faites différemment, et ce qui est directement transférable à d'autres projets.

---

## 2. Quick Start

Quatre artefacts à créer avant d'écrire une ligne de code.

**Projet Claude.ai** — à créer en premier. Configuré avec les instructions de co-construction et les documents du projet joints. Sans lui, chaque conversation repart de zéro et la co-construction perd sa continuité. Les instructions complètes sont en Annexe D.

**`SPECS.md`** — artefact central de la méthode. Co-construit dans Claude.ai en suivant la séquence ci-dessous. Contient la constitution, l'architecture, les comportements aux limites, la stratégie de test. C'est le document que Claude Code reçoit pour implémenter.

**`CLAUDE.md`** — fichier lu automatiquement par Claude Code au démarrage de chaque session. Contient les règles du projet et les principes de méthode. Sans lui, Claude Code démarre sans contexte.

**`BACKLOG.md`** — artefact de suivi. Trace les gaps, bugs, évolutions et idées brutes au fil du projet. Peut être vide au démarrage.

---

### Séquence de co-construction — 11 questions

À poser dans Claude.ai, dans l'ordre, avant de rédiger SPECS.md.

```
1.  Quel est le problème réel ? (dissocier si plusieurs)
2.  Quel est l'usage concret ?
3.  Quelle est la commande exacte que l'utilisateur tape pour lancer l'outil ?
4.  Quelle est l'hypothèse technique risquée ? (tester avant de spécifier)
5.  Quelles sont les décisions à prendre ? (les nommer toutes, sécurité incluse)
6.  Quelles sont les règles non négociables ? (constitution)
7.  Y a-t-il des zones grises ? (audit pré-rédaction)
8.  Le livrable est-il transmissible sans contexte ?
9.  Quels modules ont un contrat fixe ? (tests de contrat avant implémentation)
10. Quels risques de sécurité n'ont pas été adressés ?
11. La version a-t-elle été validée sur échantillon avant d'être généralisée ?
```

---

### 2 règles absolues avant de coder

> **Un prompt = un module = un test avant de passer au suivant.**

> **Ne jamais patcher : toute divergence remonte aux specs d'abord.**

---

*Ce Quick Start permet de démarrer. Les sections suivantes expliquent pourquoi chaque étape existe.*

---

## 3. Glossaire

Les termes suivants sont utilisés de façon précise tout au long de ce document.

**Backlog**
Registre de suivi d'un projet — gaps de spec, bugs, évolutions planifiées, idées brutes. Il pointe vers SPECS.md, ne duplique jamais le contenu des specs. Au niveau d'un projet : `BACKLOG.md`. Au niveau de l'écosystème : `BACKLOG.global.md` et `BACKLOG.projects.md`.

**Chunking**
Découpage d'un texte long en morceaux plus petits pour contourner la limite de contexte d'un LLM. Risque : le LLM analyse des fragments sans voir l'ensemble — la cohérence globale se perd. À tester comme hypothèse technique risquée avant de spécifier.

**Constitution**
Ensemble de règles non négociables définies avant l'implémentation, que ni le code ni le LLM ne peuvent violer. Ce ne sont pas des contraintes techniques — ce sont des principes de fidélité au besoin réel. Sans constitution, le LLM n'est pas faux. Il est indéfini.

**Contrat**
Description formelle de ce qu'un module doit produire — ses entrées, ses sorties, ses cas d'erreur — telle que définie dans les specs avant d'écrire le code. Un module respecte son contrat quand son output correspond exactement à ce qui a été spécifié, indépendamment de son implémentation interne.

**Dette de contrôle**
Écart croissant entre ce qu'une organisation délègue à l'IA générative et ce qu'elle supervise réellement. La dette de contrôle s'accumule en silence, souterraine — jusqu'à l'incident qui la révèle brutalement. Chaque usage non gouverné, chaque décision prise par le LLM sans validation humaine, chaque spec qui dérive sans être documentée ajoute à cette dette.

**Prompt injection**
Technique par laquelle un contenu malveillant fourni en entrée — texte d'une page web, contenu d'un fichier, message utilisateur — détourne les instructions données au LLM pour lui faire faire autre chose que ce qui était prévu. Dans un projet LLM, tout texte externe traité par le modèle est une surface d'attaque potentielle : un transcript YouTube, une fiche Markdown, une réponse d'API. La défense n'est pas technique — elle est architecturale : définir dans la constitution ce que le LLM ne doit jamais faire, quelle que soit l'instruction reçue dans le contenu traité.

**Slug**
Version simplifiée d'un titre, formatée pour être utilisée comme nom de fichier ou dans une URL. Les accents sont supprimés, les espaces remplacés par des tirets, les caractères spéciaux éliminés. Exemple : "La chute d'Anthropic (2026)" devient `la-chute-d-anthropic-2026`.

**Validateur**
Module du code dont le rôle est de vérifier que l'output généré par le LLM respecte le contrat défini dans les specs. Il ne teste pas le code interne — il teste le résultat. Il retourne une liste d'avertissements, pas d'exceptions, pour permettre une sauvegarde partielle avec signalement plutôt qu'un blocage.

**Writer**
Module du code dont le rôle est d'écrire le fichier de sortie sur le système de fichiers. Il construit le chemin, génère le slug, crée les dossiers si nécessaire, demande confirmation si le fichier existe déjà, et préfixe les avertissements du validateur en tête de fichier si nécessaire.

---

## 4. Le rôle irremplaçable de l'humain

### Sans l'humain, l'IA générative ne s'arrête pas — elle dérive

L'IA générative ne s'arrête pas quand le cadre manque. Elle produit — souvent de façon impressionnante. Mais sans présence humaine pour définir, cadrer et contrôler, elle dérive.

> Sans la présence humaine pour définir, cadrer et contrôler,
> l'IA générative ne s'arrête pas — elle dérive.
>
> Elle produit, souvent de façon impressionnante, mais hors-cible :
> du code techniquement correct pour le mauvais problème,
> des outputs structurés mais infidèles au besoin réel,
> des specs qui évoluent sans que personne ne mesure
> l'accumulation de dette.
>
> La dérive est silencieuse. Elle ne produit pas d'erreur —
> elle produit de l'apparence. C'est précisément pourquoi
> le contrôle humain n'est pas une option de confort :
> c'est la condition sans laquelle le travail du LLM
> n'a pas de sens.
>
> Sur les tâches de définition, d'arbitrage, de validation
> et du jugement, l'humain n'est pas assisté par l'IA générative.
> Il en est le gouverneur.

### Les trois formes de dérive

**Dérive de cible** : le LLM optimise pour ce qu'il perçoit, pas pour ce qui est voulu. Sans l'humain pour définir le problème réel, il répond au problème apparent — et le fait très bien. Le résultat est techniquement correct pour le mauvais usage.

**Dérive de spec** : sans l'humain pour geler les décisions, les specs évoluent pendant le codage. Le code patch, s'allonge, devient ingérable. Le LLM ne résiste pas — il s'adapte à chaque évolution sans signaler l'accumulation. Ce qui semblait être de la flexibilité est en réalité une perte de contrôle progressive. C'est la dette de contrôle appliquée au développement.

**Dérive de qualité** : sans critères définis par l'humain, le LLM produit des outputs qui *semblent* corrects. Il n'y a pas d'erreur visible — il y a une apparence de justesse. Le contenu est plausible, la forme est respectée, le résultat est livrable. Mais la fidélité au besoin réel s'est évaporée silencieusement à chaque génération. Personne ne détecte la dégradation parce que personne n'a défini ce que "correct" signifie précisément. La qualité ne se mesure pas contre un vide — elle se mesure contre une constitution. Sans elle, le LLM n'est pas faux. Il est indéfini.

### Ce que l'humain fait que l'IA ne fait pas

L'humain excelle là où le LLM ne peut structurellement pas le remplacer : **le jugement**. Juger c'est peser des éléments hétérogènes, tenir compte du contexte implicite, assumer une décision dont on est responsable. Le LLM produit des options plausibles — l'humain tranche.

Demander à Claude.ai une explication, un avis, une recommandation fait partie de la co-construction. C'est s'informer avant de décider — pas déléguer la décision. La précaution est ailleurs : rester conscient qu'on évalue la réponse avant de la suivre. Celui qui suit systématiquement sans évaluer ne co-construit plus — il suit.

Sur les tâches suivantes, l'humain n'est pas remplaçable — et ne doit pas chercher à l'être :

- **Définir le problème réel** : identifier ce qui est réellement cherché, derrière ce qui est formulé. Le LLM répond à ce qu'on lui dit, pas à ce qu'on veut dire.
- **Exercer son jugement** : évaluer si ce qui a été produit est juste, utile, fidèle — pas seulement plausible. C'est la capacité que l'humain ne doit jamais déléguer entièrement.
- **Trancher les décisions** : aucune décision de spec n'est prise par le LLM seul. L'humain valide, arbitre, assume.
- **Geler les specs** : décider que la phase de définition est terminée et que l'implémentation peut commencer. Sans ce geste délibéré, la dérive de spec est inévitable.
- **Contrôler la stratégie de test** : vérifier que les tests sont prévus, que leur type est approprié, que l'ordre est respecté. L'humain n'est pas le testeur — il est le garant de la stratégie de test.
- **Arbitrer les divergences** : quand le LLM produit quelque chose d'inattendu, décider si c'est le code qui doit changer ou la spec.
- **Valider l'alignement final** : s'assurer que ce qui a été produit correspond au besoin réel.

Celui qui code avec un LLM et qui n'exerce plus son jugement — qui suit ce que le LLM propose sans évaluer, sans questionner — a atteint son propre seuil de lucidité structurelle. Il produit encore. Mais il ne gouverne plus.

---

## 5. Les deux outils en tandem

### Deux outils, deux rôles distincts

Claude.ai et Claude Code ne sont pas interchangeables. Les utiliser comme s'ils l'étaient produit du code techniquement correct pour le mauvais usage.

| Dimension | Claude.ai | Claude Code |
|---|---|---|
| Posture naturelle | Réfléchir, cadrer, arbitrer | Produire, implémenter, avancer |
| Contexte principal | Le problème réel et les specs | Le code existant |
| Tendance observée | Séquentiel, prudent | Paralléliser, aller vite |
| Risque propre | Trop conceptuel | Diverge des specs sans le signaler |

### La phase d'orchestration — deux niveaux simultanés

L'orchestration opère à deux niveaux dans tout projet développé avec un LLM.

**Niveau technique** : un module central coordonne les autres sans contenir de logique métier. Il appelle, il transmet, il séquence — il ne décide pas. La logique métier est dans les modules spécialisés, pas dans le point d'entrée. C'est une décision architecturale qui prépare les évolutions futures.

**Niveau processus** : l'humain orchestre les outils LLM en tandem. Il décide quand utiliser Claude.ai pour cadrer, quand basculer vers Claude Code pour implémenter, quand stopper et remonter aux specs. Cette orchestration n'est pas automatique — elle est le travail central de pilotage du projet.

Ces deux niveaux s'alimentent mutuellement : une architecture bien orchestrée au niveau technique rend le pilotage plus simple. Et un pilotage rigoureux au niveau processus protège l'intégrité de l'architecture technique.

### Ce qui se passe quand Claude Code diverge

En cours de projet, Claude Code est confronté au code existant et peut proposer une architecture cohérente avec ce qu'il voit — techniquement correcte, mais pour un problème différent de celui défini en co-construction.

**La règle opérationnelle** : face à une proposition de Claude Code qui diverge des specs ou dépasse le cadre du prompt en cours, copier cette proposition dans Claude.ai — qui dispose du contexte des specs et de la constitution — et exercer son jugement depuis ce contexte. La décision revient ensuite dans Claude Code sous forme d'instruction précise. On ne corrige pas dans le code. On juge et arbitre dans Claude.ai d'abord, puis on instruit Claude Code.

### La tension séquentiel / parallèle

Claude Code tend naturellement à paralléliser. Cette tendance est risquée quand les modules ont des dépendances strictes : on ne peut pas écrire le validateur sans les tests de contrat, on ne peut pas tester le writer sans le validateur.

La discipline à imposer : un prompt, un module, un test avant de passer au suivant — dans l'ordre logique des dépendances.

### Le principe de gouvernance

```
Claude.ai   →  co-construction + specs + constitution
     ↓
Claude Code →  implémentation séquentielle dans ce cadre
     ↓
Divergence  →  l'humain exerce son jugement dans Claude.ai
     ↓
Claude.ai   →  arbitrage depuis le contexte des specs
     ↓
Claude Code →  reçoit une instruction précise et reprend
     ↑
Humain      →  orchestre, juge, valide à chaque étape
```

### Transférer cette méthode à Claude Code via CLAUDE.md

Claude Code lit automatiquement les fichiers CLAUDE.md présents dans l'environnement. Il existe trois niveaux, lus en cascade du plus général au plus spécifique :

```
~/.claude/CLAUDE.md              → global, tous les projets
~/Projects/CLAUDE.projects.md   → tous les projets dans Projects
~/Projects/mon-projet/CLAUDE.md → ce projet uniquement
```

En inscrivant les principes de cette méthode dans le CLAUDE.md global, Claude Code les consulte au démarrage de chaque session sans qu'on ait besoin de les répéter.

**Ce qui va dans le CLAUDE.md global** : les principes de méthode, la règle anti-dérive de spec, la constitution minimale de tout projet, la stratégie de test en deux temps.

**Ce qui va dans le CLAUDE.md de projet** : la constitution du projet, la stack technique, les comportements aux limites.

**Règle sur les fichiers de documentation** : quand l'instruction porte sur la mise à jour d'un fichier de documentation (.md), Claude Code ne touche pas au code. Il met à jour le fichier demandé uniquement. Sans cette précision explicite dans l'instruction, Claude Code se lance dans l'écriture du code.

### Configurer le projet Claude.ai

Claude.ai fonctionne par projets. Un projet maintient le contexte entre les conversations — sans lui, chaque session repart de zéro et la co-construction perd sa continuité.

**Ce qui va dans les instructions du projet** : le prompt de co-construction complet — posture, méthode, séquence 11 questions, structure SPECS.md, règles absolues. Ce prompt est disponible en Annexe D.

**Ce qui va dans les documents joints** : SPECS.md dès qu'il existe, BACKLOG.md, CLAUDE.md du projet. Claude.ai dispose ainsi du contexte complet pour arbitrer les divergences et mettre à jour les specs.

```
Projet Claude.ai
├── Instructions  → prompt de co-construction (Annexe D)
└── Documents    → SPECS.md, BACKLOG.md, CLAUDE.md
```

### Gestion des versions entre Claude.ai et Claude Code

SPECS.md, BACKLOG.md et CLAUDE.md existent dans deux endroits : le repo local et les documents du projet Claude.ai. Une divergence entre les deux versions est silencieuse — Claude.ai travaille sur une version périmée sans le signaler.

**Règle** : chaque fichier .md clé porte une version et une date en entête.

```
# SPECS.md
Version : 1.3
Date : 2026-04-15
```

Claude Code incrémente la version à chaque modification. Claude.ai vérifie la version au démarrage de session et signale si le document joint semble périmé.

**Règle opérationnelle** : après toute modification d'un .md en local via Claude Code, re-uploader la version mise à jour dans le projet Claude.ai avant la prochaine session de co-construction. Après toute modification d'un .md dans Claude.ai, télécharger la nouvelle version et la déposer dans le repo local — elle remplace l'ancienne. Supprimer l'ancienne version dans le projet Claude.ai avant d'uploader la nouvelle — deux versions du même fichier créent une ambiguïté que Claude.ai ne signale pas.

---

## 6. La méthode en 9 étapes

### Étape 1 — Dissocier les problèmes

**Ce qu'on a fait** : avant de parler solution, on a nommé précisément les problèmes. La question "extraire la connaissance d'une vidéo YouTube" cachait deux problèmes distincts avec des architectures différentes :

- **Problème 1 — Rétention** : ne pas avoir de trace structurée après une vidéo dense
- **Problème 2 — Retrouvabilité** : ne plus savoir dans quelle vidéo un concept a été développé

**Ce que ça a produit** : une décision architecturale claire — V1 fiche par vidéo, V2 recherche cross-vidéos — avec le transcript conservé dès la V1 pour préparer la V2 sans migration.

**Transférable à** : tout projet où le besoin initial est formulé de façon vague. Forcer la dissociation en sous-problèmes avant de chercher une solution unique.

---

### Étape 2 — Partir de l'usage réel, pas du livrable imaginé

**Ce qu'on a fait** : au lieu de proposer une structure, on a posé des questions sur l'usage concret — ce que l'utilisateur cherche réellement, comment il s'en sert, ce qu'il ne fera probablement jamais.

**Ce que ça a produit** : un output qui répond à des modes de recherche réels, pas à une structure générique. Une section "personnelle" conçue pour rester vide — parce que si le LLM la remplit, c'est lui qui retient, pas l'utilisateur.

**Transférable à** : tout projet où on conçoit un output pour soi-même ou un utilisateur. Toujours partir de "à quoi ça sert concrètement" avant de concevoir la forme.

**Ergonomie de la commande d'exécution** : avant de spécifier les modules techniques, poser explicitement la question : *"Quelle est la commande exacte que l'utilisateur tape pour lancer l'outil ?"* Cette question révèle immédiatement des décisions d'architecture — nom du script, arguments, modes d'usage, chemins, configuration.

**Règle** : la commande d'usage est une spec à part entière. Elle se définit à l'Étape 2, pas après le code.

---

### Étape 3 — Tester les hypothèses avant d'écrire les specs

Tout projet repose sur au moins une hypothèse technique centrale. Si elle est fausse, les specs construites dessus sont inutiles.

**Ce qu'on a fait** : avant de rédiger une ligne de spec pour YT Extractor, on a testé si l'extraction du transcript YouTube via API fonctionnait réellement sur une vidéo dense en concepts.

**Ce que ça a produit** : confirmation que l'architecture est faisable, et une référence réelle pour les tests. Si le test avait échoué, les specs auraient été différentes.

**Ce qui aurait dû être fait** : le choix du provider LLM par défaut — Groq — n'a pas été testé avant de spécifier. La limite du free tier (~6 000 tokens par requête) n'a été découverte qu'au test réel, sur une vidéo de plus de 5 minutes. Résultat : pivot vers Gemini en cours de projet, réécriture de la couche LLM. Ce pivot aurait été évité en testant l'hypothèse avant de spécifier.

> **Chunking** : découpage d'un texte long en morceaux plus petits pour contourner la limite de contexte d'un LLM. Risque : le LLM analyse des fragments sans voir l'ensemble — la cohérence globale se perd. Dans YT Extractor, cette limite a imposé de choisir un provider capable de recevoir le transcript complet en une seule fois. C'est une hypothèse technique à tester avant de spécifier, pas après.

**Transférable à** : tout projet avec une dépendance externe (API, service tiers, limite de quota). Tester le point d'incertitude le plus risqué avant de spécifier. Un test raté avant les specs coûte une heure. Découvert en cours de projet, il coûte une réécriture.

---

### Étape 4 — Forcer chaque décision à être nommée

Un projet contient des dizaines de décisions implicites. Le LLM en prend la plupart sans les signaler — il choisit le comportement qui lui semble raisonnable et avance. Ces décisions silencieuses deviennent des bugs ou des mauvaises surprises plus tard.

L'étape 4 est un moment explicite dans Claude.ai : pour chaque comportement du système, forcer le choix entre les options disponibles, avec la raison documentée.

**Ce qu'on a fait** dans YT Extractor :

| Décision | Options | Choix et raison |
|---|---|---|
| Envoyer le transcript au LLM | Complet en une fois / Découpé en chunks | Complet — le chunking fragmente l'analyse, la cohérence globale se perd |
| Comportement si la fiche est incomplète | Bloquer / Sauvegarder avec avertissement | Sauvegarder — bloquer fait perdre le travail déjà produit |
| Contexte trop long pour le LLM | Tronquer silencieusement / Bloquer avec message | Bloquer — tronquer silencieusement viole la constitution |
| Provider LLM par défaut | Groq / Gemini / OpenAI | Gemini free tier — seul capable de recevoir un transcript long |

Sans cette étape, Claude Code aurait chunké par défaut, tronqué silencieusement, et choisi Groq parce que c'était le premier provider dans les exemples. Techniquement correct. Faux par rapport au besoin réel.

**Transférable à** : avant de commencer l'implémentation, parcourir les specs avec Claude.ai et poser explicitement la question pour chaque comportement non trivial : *"Quelles sont les options ? Laquelle on choisit et pourquoi ?"* Si la réponse est "ça dépend" — c'est une zone grise à résoudre maintenant.

---

### Étape 5 — Inscrire une constitution avant le code

La constitution est l'ensemble des règles que le LLM ne peut jamais violer — même si le résultat semble acceptable en surface.

Ce ne sont pas des contraintes techniques. Ce sont des principes de fidélité au besoin réel, définis avant que le code existe.

**Ce qu'on a fait** : YT Extractor a une constitution de 8 règles, définie avant le premier module. Wiki LLM en a 15.

Pourquoi 15 au lieu de 8 ? Parce que Wiki LLM est *stateful* — chaque ingestion modifie un état persistant. Des règles d'intégrité dans le temps sont apparues, qui n'existaient pas dans un projet stateless : ne jamais supprimer une page sans instruction humaine, ne jamais écraser une source qui contredit — ajouter un angle. Ces règles n'auraient pas émergé sans la constitution de YT Extractor comme base.

**Ce que ça a produit** : dans les deux projets, quand un cas limite est apparu pendant l'implémentation, la règle existait déjà. Le validateur de YT Extractor est simple à écrire parce que les 8 règles sont déjà définies — il ne fait que les vérifier.

Sans constitution, le LLM n'est pas faux. Il est indéfini.

**Transférable à** : tout projet impliquant un LLM dans la chaîne de traitement. La question à poser dans Claude.ai : *"Qu'est-ce que ce système ne doit jamais faire, même si le résultat semble correct ?"*

---

### Étape 6 — Demander à Claude.ai de relire les specs

Une fois les specs rédigées, elles contiennent presque toujours des zones grises non détectées — comportements non spécifiés, cas limites oubliés, décisions implicites que le LLM interprétera à sa façon.

L'étape 6 est un prompt explicite dans Claude.ai, avant de passer à l'implémentation :

> *"Relis ces specs. Identifie les zones grises, les comportements non spécifiés, et ce qui manque."*

Ce n'est pas une vérification mentale — c'est une demande formelle de relecture critique. Claude.ai dispose du contexte complet des specs et de la constitution. Il voit ce que l'auteur ne voit plus après plusieurs heures de rédaction.

**Ce que ça produit** : une liste de points à trancher avant que Claude Code les rencontre en production. Chaque point résolu ici évite un signal 🚨 SPEC MANQUANTE pendant l'implémentation.

**Transférable à** : tout document de specs avant de basculer vers l'implémentation. L'audit pré-rédaction est une étape à part entière, pas une vérification optionnelle. Si Claude.ai ne trouve aucune zone grise, les specs sont prêtes.

---

### Étape 7 — Distinguer la pensée du livrable

La tentation de raccourci apparaît à un moment précis dans toute session de co-construction : on *voit* le livrable — mais les décisions ne sont pas toutes tranchées.

Ce moment est reconnaissable : on commence à formuler des phrases de specs, à imaginer la structure du fichier, à vouloir produire. C'est le signal que la co-construction n'est pas terminée.

**Ce qu'on a fait** dans Wiki LLM : avant d'écrire une ligne de spec, une Phase 0 entière a été consacrée à corriger la terminologie — "outil LLM pilote" au lieu d'"agent", "ingestion" au lieu d'"import". Ce n'est pas du détail : un terme mal posé dans les specs produit un module mal conçu. On aurait pu passer directement au code. On est resté dans la co-construction jusqu'à ce que le cadre conceptuel soit juste.

**Comment savoir que les specs sont terminées ?** Quand l'étape 6 — la relecture par Claude.ai — ne trouve plus de zones grises. Pas avant.

**Transférable à** : tout livrable co-construit avec un LLM. Si la tentation de produire apparaît avant que la pensée soit complète, nommer ce moment explicitement et revenir à la co-construction.

---

### Étape 8 — Définir les tests avant d'implémenter les modules à contrat fixe

#### Principe

Ne pas écrire les tests après le code pour valider ce qui a été fait. Écrire les tests avant le code pour décrire ce qui doit être fait.

Cette discipline s'applique spécifiquement aux modules dont le contrat est entièrement défini dans les specs — en particulier le validateur, qui vérifie que chaque output du LLM respecte la constitution avant d'être sauvegardé.

L'humain ne joue pas le rôle de testeur. Il vérifie que les tests sont prévus, que leur type est adapté, que l'ordre est respecté.

#### Les deux temps

**Temps 1 — Tests de contrat : avant les modules concernés**

Dans YT Extractor, le validateur a été écrit en premier, puis les tests ont été écrits pour ce validateur. Résultat : les tests s'adaptent à ce que le code fait — pas à ce qu'il devrait faire. Si le validateur laisse passer une fiche incomplète, le test le laisse passer aussi.

Dans Wiki LLM, l'ordre a été inversé. Les 9 tests TC-01 à TC-09 ont été écrits avant que `validator.py` existe. Chaque test décrit une règle de la constitution : champ obligatoire présent, format correct, règle non violée. Le validateur a ensuite été écrit pour faire passer ces tests — pas l'inverse. Un seul ajustement a été nécessaire en cours de route, dans le test lui-même, pas dans le module.

> "Ne modifie pas les tests pour qu'ils s'adaptent au code. C'est le code qui s'adapte aux tests."

**Temps 2 — Tests d'intégration : après le pipeline complet**

Dans les deux projets, le test smoke — test minimal de bout en bout qui vérifie que les modules s'enchaînent correctement, de l'entrée jusqu'à la sortie sans erreur — a été le dernier prompt avant la documentation.

#### Pourquoi c'est important avec un LLM implémenteur

Un LLM écrit du code qui fonctionne selon sa propre interprétation des specs. Les tests écrits avant lui donnent une définition non ambiguë du succès. Sans eux, "ça marche" signifie "ça tourne" — pas "ça respecte le contrat".

---

### Étape 9 — Angle mort : la sécurité

La sécurité n'a pas été abordée dans les sessions de co-construction de YT Extractor. Ce n'est pas un oubli anodin — c'est un angle mort structurel qui aurait dû être adressé à deux étapes précises.

**À l'Étape 4** : valider les entrées utilisateur, définir le comportement si une clé API est absente, avertir l'utilisateur que le contenu est envoyé à une API externe.

**À l'Étape 5** : inscrire dans la constitution la responsabilité de l'utilisateur concernant la confidentialité du contenu soumis. Inscrire également la règle sur le prompt injection — voir Glossaire.

**La règle** : la sécurité s'inscrit dans les décisions (Étape 4) et dans la constitution (Étape 5). Si elle n'est pas adressée à ces deux moments, elle ne sera pas adressée. L'Annexe B contient une checklist minimale à parcourir systématiquement.

---

## 7. Gouvernance après le MVP

### 7.1 La boucle de retour

Livrer un MVP n'est pas la fin du projet — c'est le début de la gouvernance. Ce qui change après le MVP : le LLM n'est plus la source principale de signaux. C'est la production.

La boucle de retour est le flux qui permet de traiter ces signaux sans perdre l'intégrité des specs :

```
Production → Observation → BACKLOG → Claude.ai → SPECS.md → Claude Code
```

**Cinq situations concrètes, un seul flux :**

**"J'ai détecté un bug en production"**
→ BACKLOG.md, section Bugs. Décrire le comportement observé, le comportement attendu, les conditions de reproduction. Ne pas toucher au code avant d'avoir remonté aux specs si le bug révèle un cas non spécifié.

**"Une nouvelle idée m'est venue"**
→ BACKLOG.md, section Idées. La tracer sans l'évaluer maintenant. Le BACKLOG n'est pas un filtre — c'est un registre. L'arbitrage se fait lors de la revue périodique, pas au moment de l'idée.

**"Je veux faire évoluer les specs"**
→ BACKLOG.md d'abord, puis session Claude.ai pour évaluer l'impact sur l'architecture existante, puis mise à jour de SPECS.md, puis instruction à Claude Code. Ne jamais modifier les specs directement dans Claude Code sans passer par Claude.ai.

**"Mon provider LLM va être déprécié"**
→ BACKLOG.md, section Dépendances externes. Note la date de dépréciation, le provider concerné, les alternatives identifiées. C'est une évolution planifiée — elle suit le flux normal vers les specs quand elle est arbitrée.

**"wiki-llm dépend d'une évolution de yt-extractor"**
→ BACKLOG.projects.md — pas dans le BACKLOG local du projet. Les dépendances inter-projets vivent au niveau écosystème, pas au niveau projet. Sinon elles sont invisibles lors de la priorisation.

---

### 7.2 Les artefacts de gouvernance

| Artefact | Rôle | Qui lit | Qui écrit |
|---|---|---|---|
| `SPECS.md` | Constitution et architecture du projet | Claude Code à chaque session | L'humain après arbitrage dans Claude.ai |
| `CLAUDE.md` | Règles opérationnelles du projet | Claude Code automatiquement | L'humain quand une règle change |
| `BACKLOG.md` | Registre vivant du projet | L'humain en revue | L'humain en continu |
| `BACKLOG.global.md` | Arbitrage portfolio, idées cross-projets | L'humain en revue | L'humain en continu |
| `BACKLOG.projects.md` | Dépendances et priorisation inter-projets | L'humain en revue | L'humain quand une dépendance est identifiée |
| `CLAUDE.global.md` | Règles universelles, tous les projets | Claude Code automatiquement | L'humain quand un principe de méthode évolue |
| `CLAUDE.projects.md` | Conventions communes à l'ensemble des projets | Claude Code automatiquement | L'humain quand une convention change |

**La cascade CLAUDE.md** : Claude Code lit ces fichiers dans l'ordre, du plus général au plus spécifique. Une règle dans `CLAUDE.global.md` s'applique à tous les projets sauf si elle est explicitement surchargée dans `CLAUDE.md` du projet.

```
~/.claude/CLAUDE.md              → global, tous les projets
~/Projects/CLAUDE.projects.md   → tous les projets dans Projects
~/Projects/mon-projet/CLAUDE.md → ce projet uniquement
```

---

### 7.3 Quand mettre à jour les specs

SPECS.md n'est pas un document figé après le MVP. Il évolue — mais selon trois déclencheurs précis, pas en continu.

**Déclencheur 1 — Bug avec impact spec**
Un bug révèle un comportement non spécifié ou mal spécifié. Le flux : BACKLOG → Claude.ai pour évaluer si c'est un bug de code ou un gap de spec → mise à jour de SPECS.md si c'est un gap → instruction à Claude Code.

Si c'est un bug de code pur : correction dans Claude Code sans toucher aux specs.
Si c'est un gap de spec : corriger les specs d'abord, le code ensuite. Toujours dans cet ordre.

**Déclencheur 2 — Gap détecté en production**
Un cas réel non couvert par les specs apparaît. Il ne produit pas d'erreur — il produit un output indéfini. Le flux est identique au déclencheur 1.

**Déclencheur 3 — Évolution planifiée vers une V2**
Une fonctionnalité du BACKLOG est arbitrée et planifiée. Avant d'instruire Claude Code, une session de co-construction dans Claude.ai relit les specs existantes, évalue l'impact de l'évolution sur l'architecture, et produit une version mise à jour de SPECS.md. Claude Code reçoit les specs mises à jour — pas un prompt de modification directe.

**Règle sur les fichiers de documentation** : Claude Code peut écrire dans n'importe quel fichier de documentation (.md) — uniquement pour exécuter une décision arbitrée dans Claude.ai. L'instruction doit être explicite : *"Mets à jour ce fichier selon ces décisions. Tu ne codes rien."* Sans cette précision, Claude Code se lance dans l'écriture du code.

---

### 7.4 Le BACKLOG dans la durée

Le BACKLOG n'est pas une todo-list. C'est un registre vivant — il trace ce qui a été observé, décidé, reporté, sans jugement immédiat.

**Ce qu'il contient** : bugs, gaps de spec, idées brutes, dépendances techniques, évolutions planifiées. Tout ce qui arrive en production et mérite d'être tracé sans nécessairement être traité maintenant.

**Ce qu'il ne contient pas** : les specs elles-mêmes, les décisions d'architecture, le contenu de CLAUDE.md. Il pointe vers SPECS.md — il ne le duplique pas.

**La revue périodique** : une session régulière — mensuelle sur un projet actif, avant chaque reprise sur un projet en pause — dans Claude.ai avec BACKLOG.md joint. L'objectif : arbitrer ce qui monte en V2, archiver ce qui ne sera jamais fait, identifier les dépendances inter-projets à remonter dans BACKLOG.projects.md.

Sans revue périodique, le BACKLOG grossit sans jamais être arbitré. Il devient un cimetière d'idées — et perd son rôle de signal.

Le template du BACKLOG.md est en Annexe C.

---

## 8. Ce que cette méthode permet de faire

Après avoir appliqué cette méthode sur un premier projet :

**Avant de coder**
- Co-construire des specs dans Claude.ai qui tiennent jusqu'à l'implémentation
- Définir une constitution que le LLM ne peut pas violer
- Identifier les hypothèses risquées avant qu'elles deviennent des bugs

**Pendant l'implémentation**
- Instruire Claude Code sans qu'il dérive des specs
- Détecter une divergence et la remonter aux specs avant de patcher
- Avancer module par module sans régression silencieuse

**Après le MVP**
- Reprendre un projet 3 mois plus tard sans tout relire
- Tracer bugs, gaps et évolutions sans polluer les specs
- Faire évoluer les specs selon un flux qui préserve l'intégrité du code

---

*Ce que cette méthode ne promet pas : aller plus vite au départ. Ce qu'elle garantit : ne pas reculer.*

---

## Annexe A — Guide technique : tests et séquence d'implémentation

*Pour les développeurs qui implémentent : les exemples de code sont des gabarits à adapter. Pour les non-développeurs qui pilotent : l'essentiel est la séquence et l'instruction à donner à Claude Code — pas le code lui-même.*

---

### Prérequis : installer gstack

`/review` est une commande Claude Code qui active le rôle "staff engineer" — analyse le code comme un ingénieur senior qui cherche les bugs de production, les failles de sécurité, et les problèmes de maintenabilité. Elle détecte ce que les tests ne détectent pas.

Installation : **https://github.com/garrytan/gstack**
Licence MIT. Gratuit.

---

### La règle de base

> *1 prompt → 1 module → 1 test → `/review` → module suivant*

Ne jamais passer au module suivant sans avoir validé le précédent. `/review` est le dernier geste avant de continuer — pas une option.

---

### Tests de contrat — écrits avant les modules concernés

Les modules à contrat précis défini dans les specs ont leurs tests écrits avant eux. Le code est ensuite écrit pour faire passer ces tests — pas l'inverse.

```python
def test_sections_obligatoires():
    assert "## Section A" in output
    assert "## Section B" in output

def test_section_personnelle_vide():
    section = extraire_section(output, "## Notes")
    assert section.strip() == "*(espace libre)*"

def test_chapitrage_dans_les_limites():
    n = compter_lignes_tableau(output, "## Chapitrage")
    assert 6 <= n <= 12
```

**Règle absolue** : si un test échoue, on corrige le code — jamais le test.

---

### Tests d'intégration — écrits après le pipeline complet

Le test smoke — test minimal de bout en bout qui vérifie que les modules s'enchaînent correctement, de l'entrée jusqu'à la sortie sans erreur — est écrit en dernier, après tous les modules.

```python
def test_smoke():
    result = run_pipeline("URL_DE_REFERENCE")
    assert result.exists()
    assert result.stat().st_size > 0
```

---

### Séquence d'implémentation avec TDD partiel

```
Prompt 1   — Bootstrap (structure + fichiers vides + docstrings)
Prompt 2   — Module source de données
             → test → /review
Prompt 3   — Module métadonnées
             → test → /review
Prompt 4   — Couche LLM abstraite
             → test → /review
Prompt 5   — Module de génération
             → test → /review
Prompt 6   — Tests de contrat     ← avant validateur et writer
Prompt 7   — Validateur           ← implémenté pour passer les tests
             → /review
Prompt 8   — Writer
             → test → /review
Prompt 9   — Point d'entrée CLI
             → /review
Prompt 10  — Test d'intégration   ← après pipeline complet
Prompt 11  — Documentation
```

---

### Instruction à donner à Claude Code

```
Écris les tests de contrat avant que les modules concernés existent.
Les tests décrivent le contrat défini dans les specs — pas le code actuel.
Le module sera implémenté ensuite pour faire passer ces tests.
Ne modifie jamais un test pour qu'il s'adapte au code produit.
Après chaque module : attends que les tests passent et que /review
soit lancé avant de passer au suivant.
```

---

## Annexe B — Sécurité : avant de coder et pendant le code

*À parcourir à l'Étape 4 (décisions) et à l'Étape 5 (constitution).*

**Note sur l'outillage** : le plugin Security Guidance (Anthropic, installé dans Claude Code) détecte automatiquement les patterns dangereux à l'écriture du code — injection de commande, XSS, eval(), pickle. Il ne remplace pas cette checklist : il couvre la sécurité du code produit, pas la sécurité du projet.

---

### Avant de coder — sécurité projet
*Checklist humaine. Ces points ne sont pas vérifiables par un outil.*

**Entrées utilisateur**
- [ ] Les entrées sont validées avant tout traitement
- [ ] Un format invalide produit un message d'erreur lisible, pas un crash
- [ ] Les URLs ou chemins sont vérifiés avant d'être passés aux APIs

**Clés et secrets**
- [ ] Les clés API sont dans `.env`, jamais dans le code
- [ ] `.env` est dans `.gitignore` — vérifié avant le premier commit
- [ ] `.env.example` documente les variables sans valeurs réelles
- [ ] Le comportement si une clé est absente est défini dans les specs

**Données envoyées aux APIs externes**
- [ ] La nature des données envoyées au LLM est documentée
- [ ] L'utilisateur est averti si le contenu peut être confidentiel
- [ ] Le provider LLM par défaut tient compte de la confidentialité
- [ ] Le risque de prompt injection est adressé dans la constitution — voir Glossaire

**Versioning**
- [ ] `.gitignore` exclut `.env`, fichiers de config, dossiers de sortie
- [ ] Le premier commit ne contient aucune clé ou donnée personnelle
- [ ] Les fichiers d'exemple ne contiennent que des valeurs fictives

---

### Pendant le code — sécurité code
*Protocole de réponse aux warnings du plugin Security Guidance.*

Quand le plugin émet un warning pendant une session Claude Code :

1. **Ne pas ignorer.** Le warning signale un pattern potentiellement dangereux dans le code que Claude Code s'apprête à écrire.
2. **Copier le warning complet dans Claude.ai.** Demander : *"Est-ce un vrai risque dans le contexte de ce projet ?"*
3. **Trancher avant de continuer.** Si c'est un vrai risque : demander à Claude Code d'appliquer la remédiation suggérée. Si ce n'est pas applicable : documenter la décision dans BACKLOG.md (section Gaps de spec) et continuer.
4. **Ne jamais ignorer deux fois le même warning.** Un warning ignoré une fois est une décision. Ignoré deux fois, c'est une dette silencieuse.

---

## Annexe C — Template BACKLOG.md

*Le BACKLOG est un registre de statut — il pointe vers SPECS.md, ne duplique jamais le contenu des specs.*

---

```markdown
# BACKLOG — [nom du projet]
Version : 1.0
Date : [date]

## Idées brutes
*Une phrase. Pas encore qualifiées. L'arbitrage se fait en revue périodique.*

## Évolutions planifiées
### [ID] Titre court
**Projet** :
**Version cible** :
**Problème** : une phrase + réf. SPECS.md si existant
**Dépendances** :
**Source** : [titre discussion Claude.ai] — [lien]
**Statut** : problème défini | specs en cours | specs validées
             | implémentation | fait

## Dépendances externes
### [DATE LIMITE] Titre court
**Projet** :
**Description** :
**Action requise** :
**Source** : [titre discussion Claude.ai] — [lien]
**Statut** : surveillé | action planifiée | traité

## Bugs connus
### [SEVERITY] Titre court
**Projet** :
**Source** : /review | production | test | [lien discussion]
**Description** :
**Impact** :
**Fix** : réf. SPECS.md ou note courte
**Statut** : ouvert | en cours | différé

## Tests manquants
### [module] fonction
**Projet** :
**Source** : /review | [lien discussion]
**Cas** : liste
**Statut** : ouvert | différé

## Gaps de spec
### Titre court
**Projet** :
**Source** : production | Claude.ai — [lien]
**Description** : comportement non spécifié découvert
**Action** : mise à jour SPECS.md avant implémentation
**Statut** : ouvert | specs en cours | résolu
```

---

### Règles

- Le BACKLOG pointe vers SPECS.md — il ne duplique jamais le contenu des specs
- Une idée brute devient une évolution planifiée seulement quand le problème est formulé et arbitré dans Claude.ai
- Toute entrée cite sa source — discussion Claude.ai avec lien si possible
- Un bug non tracé est une dette silencieuse
- Une évolution non tracée est une intention perdue
- Revue périodique : mensuelle sur un projet actif, avant chaque reprise sur un projet en pause

---

## Annexe D — Prompt d'instructions : Projet Claude.ai

*À copier dans les instructions du projet Claude.ai au démarrage de tout nouveau projet.*

```
Tu es le co-constructeur de specs de ce projet.
Ton rôle : cadrer, questionner, arbitrer — pas implémenter.

## Ta posture

- Tu formules des avis, pas des validations
- Tu signales les incohérences, même si je semble convaincu
- Tu présentes les alternatives avant de recommander
- Tu dis explicitement quand une décision est risquée
- Tu ne vas pas dans mon sens par défaut
- Tu poses la question qui dérange si elle n'a pas été posée
- Tu co-construis la pensée avant de produire un livrable

## Ce que tu ne fais pas

- Tu ne produis pas de livrable avant que la pensée soit complète
- Tu ne prends pas de décision d'architecture seul
- Tu ne codes rien — jamais

## Exception : scripts shell et commandes terminal

Tu peux produire des scripts shell (.sh) et des commandes
terminal à la demande. Ce n'est pas de l'implémentation —
c'est de l'outillage. Tu restes dans ton rôle de co-constructeur.

## La méthode

Quand je démarre un nouveau projet, tu me guides à travers
ces 11 questions dans l'ordre :

1.  Quel est le problème réel ? (dissocier si plusieurs)
2.  Quel est l'usage concret ?
3.  Quelle est la commande exacte que l'utilisateur tape
    pour lancer l'outil ?
4.  Quelle est l'hypothèse technique risquée ?
    (à tester avant de spécifier)
5.  Quelles sont les décisions à prendre ?
    (les nommer toutes, sécurité incluse)
6.  Quelles sont les règles non négociables ? (constitution)
7.  Y a-t-il des zones grises ? (audit pré-rédaction)
8.  Le livrable est-il transmissible sans contexte ?
9.  Quels modules ont un contrat fixe ?
    (tests de contrat avant implémentation)
10. Quels risques de sécurité n'ont pas été adressés ?
11. La version a-t-elle été validée sur échantillon
    avant d'être généralisée ?

Une question à la fois. Tu attends ma réponse avant de passer
à la suivante. Tu reformules si ma réponse est ambiguë.
Tu signales si une décision est encore ouverte.

## Structure de SPECS.md à produire

À l'issue des 11 questions, tu co-construis SPECS.md
en 5 blocs + constitution :

Bloc 0 — Constitution (règles non négociables)
Bloc 1 — Vue d'ensemble (objectif, périmètre, hors scope)
Bloc 2 — Architecture (stack, structure, flux de traitement)
Bloc 3 — Prompt système (en anglais, instruction LLM)
Bloc 4 — Comportements aux limites (chaque cas d'erreur)
Bloc 5 — Stratégie de test (contrat, smoke, checklist)

## 2 règles absolues avant de coder

1. Un prompt = un module = un test avant de passer au suivant
2. Ne jamais patcher : toute divergence remonte aux specs d'abord

## BACKLOG

Tu m'aides à maintenir BACKLOG.md comme registre vivant.
Quand un gap, bug ou idée apparaît en cours de session :
- Tu proposes une entrée BACKLOG plutôt qu'un patch immédiat
- Tu ne modifies jamais SPECS.md sans que le problème
  soit d'abord tracé dans BACKLOG.md

## Intégrité des specs

Tu signales quand SPECS.md devient illisible ou contradictoire.
Tu proposes une réécriture uniquement quand la cohérence est
compromise — pas à chaque évolution.

## Gestion des versions

Tu indiques la version et la date en entête de tout .md
que tu produis ou modifies. Au démarrage de session, tu
vérifies la version des documents joints et tu signales
si une version plus récente semble exister.

## Reprise de projet

Quand je reprends un projet après une pause, tu commences par :
1. Relire SPECS.md et BACKLOG.md joints au projet
2. Me signaler les points ouverts à arbitrer
3. Attendre mon instruction avant d'avancer

## Gouvernance des discussions

Une discussion Claude.ai a une durée de vie utile. Au-delà,
le contexte se dégrade et la co-construction perd en précision.

**Claude.ai signale proactivement** quand :
- Le sujet change significativement par rapport aux specs en cours
- La discussion est longue et les réponses perdent en précision

Signal à utiliser :
> 💬 NOUVELLE DISCUSSION RECOMMANDÉE : [raison en une phrase]
> Nom suggéré : [nom court et descriptif]
> Prompt de démarrage : [prompt prêt à coller dans la nouvelle discussion]
> Ouvrir dans : projet actuel [nom] | nouveau projet [nom suggéré]
> Documents à mettre à jour : [liste]
>   → Supprimer l'ancienne version dans le projet cible avant d'uploader

**L'humain peut poser la question à tout moment** :
> "On reste dans cette discussion ou on en ouvre une nouvelle ?"

Claude.ai répond avec une recommandation claire, la raison,
le nom suggéré, le prompt de démarrage, et les documents à mettre à jour.

**Règle de transfert** : avant d'ouvrir une nouvelle discussion,
mettre à jour SPECS.md et BACKLOG.md si nécessaire.
Dans le projet Claude.ai : supprimer l'ancienne version du document
avant d'uploader la nouvelle — deux versions du même fichier
créent une ambiguïté que Claude.ai ne signale pas.
Ne jamais ouvrir une nouvelle discussion sans transférer
le contexte documenté à jour.

## Signal de divergence

Si une décision de spec manque pendant l'implémentation :
🚨 SPEC MANQUANTE : [description précise]
On ne continue pas sans validation humaine.
```

---

*Document produit dans le cadre du projet vibe-coding-governed*
*Méthode applicable à tout projet de spécification technique impliquant un LLM*
*Repo : https://github.com/fbi92120/vibe-coding-governed*
