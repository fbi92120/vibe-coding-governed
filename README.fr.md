# Vibe Coding Gouverné

Une méthode pour produire du code avec un LLM où les specs passent en premier, l'humain décide, et le code suit.

Le vibe-coding est puissant. Non gouverné, c'est une loterie. Ce repo documente une méthode qui maintient l'humain aux commandes en tirant parti des LLMs pour la spécification comme pour l'implémentation.

## La méthode

**METHODE_SPECS_CO-CONSTRUCTION.md** (v7.1) définit un processus en 9 étapes pour co-construire les spécifications avec un LLM avant d'écrire une ligne de code :

1. Dissocier les problèmes (pas la solution imaginée)
2. Partir de l'usage réel, pas du livrable
3. Tester les hypothèses risquées avant de spécifier
4. Forcer chaque décision à être nommée
5. Inscrire une constitution (règles non négociables) avant le code
6. Demander à Claude.ai de relire les specs avant d'implémenter
7. Distinguer la pensée du livrable
8. Définir les tests avant d'implémenter les modules à contrat fixe
9. Adresser la sécurité comme contrainte structurelle, pas en dernier

Principe central : **les specs éliminent les ambiguïtés avant que le code ne les rencontre.** Le LLM co-construit les specs (Claude.ai), puis une session LLM séparée implémente (Claude Code). L'humain orchestre, valide et décide à chaque étape.

## Fichiers de support

| Fichier | Rôle |
|---|---|
| **CLAUDE.global.md** | Instructions globales pour Claude Code sur tous les projets |
| **CLAUDE.projects.md** | Conventions communes à tous les projets dans ~/Projects/ |
| **BACKLOG.global.md** | Idées cross-projets, arbitrage portfolio, dépendances externes |
| **BACKLOG.projects.md** | Dépendances inter-projets et priorisation cross-projets |

> **Note sur la numérotation** : la section 8 de METHODE v6 ("Ce qui est systématiquement transférable") a été supprimée en v7 — son contenu a été absorbé dans le Quick Start (section 2) et les 9 étapes (section 6). Le document compte désormais 8 sections au lieu de 9.

## Études de cas

La méthode a été appliquée de bout en bout sur deux projets :

### YT Knowledge Extractor

Outil CLI stateless : une URL YouTube en entrée, une fiche de connaissance Markdown structurée en sortie.

- **Architecture** : flux de données linéaire, couche LLM abstraite, orchestrateur passif
- **Constitution** : 8 règles définies avant le code
- **Résultat** : 10 prompts, V1 fonctionnelle

Voir **YT_EXTRACTOR_RETOUR_EXPERIENCE.md** pour le retour d'expérience complet.

### Wiki LLM

Base de connaissances stateful maintenue par un LLM à partir de fiches sources. Inspiré du [LLM Wiki de Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

- **Architecture** : 7 modules, orchestrateur passif, 3 modes d'ingestion, Obsidian comme interface de lecture
- **Constitution** : 15 règles (les systèmes stateful nécessitent des règles d'intégrité dans le temps absentes des projets stateless)
- **Résultat** : 12 prompts, 3 533 lignes de Python, 20/20 tests, 88 pages wiki produites
- **Ratio specs/code** : 1:2,7 (1 286 lignes de specs pour 3 533 lignes de Python)
- **Temps** : ~6h30 specs + ~3h50 coding = ~10h20 total

Voir **WIKI_LLM_RETOUR_EXPERIENCE.md** pour le retour d'expérience complet.

### La différence structurelle

YT Extractor est **stateless** : chaque exécution est indépendante. Wiki LLM est **stateful** : chaque ingestion modifie un système persistant. Cette différence a entraîné 7 règles de constitution supplémentaires, des tests de contrat sur des invariants octet par octet, et un validateur avec snapshots avant/après. La méthode a géré les deux — les specs font remonter la complexité avant que le code ne la cache.

## Repos

- [wiki-llm](https://github.com/fbi92120/wiki-llm) — code source et wiki
- [yt-knowledge-extractor](https://github.com/fbi92120/yt-knowledge-extractor) — code source YT Knowledge Extractor

## Licence

Non spécifiée pour l'instant.
