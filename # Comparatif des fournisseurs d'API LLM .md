# Comparatif des fournisseurs d'API LLM — Coûts, quotas et remises

## Vue d'ensemble

Ce document synthétise les tarifs API des principaux fournisseurs de LLM en ligne, avec un focus sur les quotas inclus, les remises batch, et les coûts calculés pour des prompts de **30 000 tokens en entrée**. Il couvre également les particularités des offres Perplexity et Mammouth.

---

## 1. Modèle de tarification général

### Abonnement Chat vs. API

Chez la quasi-totalité des fournisseurs, l'abonnement "chat" (interface utilisateur) et l'API sont **deux produits distincts** :

- L'abonnement (ChatGPT Plus, Claude Pro, Gemini Advanced…) donne accès à une interface conversationnelle avec quotas de messages.
- L'API est facturée **à l'usage**, en fonction du nombre de tokens traités.
- Il n'y a pas de "crédit API" automatique dans les abonnements chat, sauf chez certains acteurs spécifiques (Perplexity, Mammouth).

### Les deux composantes du coût API

Chaque appel API est facturé en deux parties :

- **Tokens en entrée** (prompt, contexte, historique) : en général moins chers.
- **Tokens en sortie** (réponse générée) : en général 3x à 6x plus chers que l'entrée.

Pour un prompt de 30 000 tokens, le coût réel dépend donc aussi de la longueur de la réponse attendue.

---

## 2. Tableau comparatif des fournisseurs

Pour chaque fournisseur, deux modèles sont présentés : **Premium** (le plus puissant) et **Average** (rapport qualité/prix).

| Fournisseur | Modèle | Niveau | Entrée / 1M tokens | Sortie / 1M tokens | Coût 30k entrée | Coût 30k sortie | Batch API |
|---|---|---|---:|---:|---:|---:|---|
| OpenAI | GPT-5.4 | Premium | $2.50 | $15.00 | $0.075 | $0.450 | -50% |
| OpenAI | GPT-5.4 nano | Average | $0.20 | $1.25 | $0.006 | $0.038 | -50% |
| Mistral | Mistral Large 3 | Premium | $0.50 | $1.50 | $0.015 | $0.045 | -50% |
| Mistral | Mistral Small 3.1 | Average | $0.03 | $0.09 | $0.0009 | $0.003 | -50% |
| DeepSeek | DeepSeek R1 | Premium | $0.55 | $2.19 | $0.0165 | $0.066 | Non documenté |
| DeepSeek | DeepSeek V3.2 | Average | $0.28 | $1.10 | $0.008 | $0.033 | Non documenté |
| Gemini | Gemini 3.1 Pro | Premium | $2.00 | $8.00 | $0.060 | $0.240 | Non documenté |
| Gemini | Gemini 1.5 Flash | Average | $0.07 | $0.30 | $0.002 | $0.009 | Non documenté |
| Perplexity | Sonar Pro | Premium | ~$1.00 | ~$1.00 | ~$0.030 | ~$0.030 | Non documenté |
| Mammouth | Modèles agrégés | All-in-one | Crédit $4 inclus/mois | — | — | — | — |

> Note : Les prix peuvent évoluer. Vérifier les pages officielles avant tout engagement.

---

## 3. Simulation de coût — 30 000 tokens entrée + sortie

Simulation pour **30 000 tokens en entrée + 30 000 tokens en sortie** (équivalent à un prompt long avec réponse détaillée) :

| Fournisseur | Modèle | Coût total (30k + 30k) | Batch total (-50%) |
|---|---|---:|---:|
| OpenAI | GPT-5.4 | $0.525 | ~$0.263 |
| OpenAI | GPT-5.4 nano | $0.044 | ~$0.022 |
| Mistral | Mistral Large 3 | $0.060 | ~$0.030 |
| Mistral | Mistral Small 3.1 | $0.004 | ~$0.002 |
| DeepSeek | DeepSeek R1 | $0.083 | — |
| DeepSeek | DeepSeek V3.2 | $0.041 | — |
| Gemini | Gemini 3.1 Pro | $0.300 | — |
| Gemini | Gemini 1.5 Flash | $0.011 | — |

---

## 4. Réductions Batch API

Deux fournisseurs ont une politique de remise batch **publique et documentée** :

### OpenAI Batch API
- **Remise : -50%** sur le prix entrée ET sortie.
- Fonctionne en mode asynchrone : la réponse peut prendre jusqu'à 24h.
- Idéal pour : classification en masse, extraction de données, traitements différés.
- Aucun abonnement requis, juste un compte API actif.

### Mistral Batch API
- **Remise : -50%** également sur les requêtes batchées.
- Disponible sur les modèles principaux (Large, Small, etc.).
- Particulièrement adapté à l'OCR, résumé en volume, classification.
- Compatible avec LangGraph et les pipelines automatisés.

### DeepSeek, Gemini, Perplexity
- Aucune remise batch publique clairement documentée dans les sources disponibles.
- DeepSeek compense par des prix de base très bas.

---

## 5. Cas particuliers : Perplexity et Mammouth

### Perplexity
- Perplexity est principalement connu pour son moteur de recherche IA, mais propose une API.
- Le modèle Sonar intègre de la recherche web en temps réel, ce qui lui donne un avantage différenciant pour les prompts nécessitant de l'information fraîche.
- L'abonnement Pro peut inclure un crédit API mensuel, mais la tarification reste à l'usage.
- **Pas de batch discount documenté** dans les sources publiques.

### Mammouth
- Mammouth n'est pas un fournisseur d'API LLM traditionnel, mais une **plateforme d'agrégation** qui donne accès à plusieurs modèles via un abonnement unique.
- L'abonnement inclut des crédits API.
- Utile pour **explorer plusieurs modèles à coût fixe**, mais pas adapté à de la production API en gros volume.
- **Pas de remise batch** : le modèle est orienté "accès multi-LLM" plutôt qu'optimisation volume.

---

## 6. Recommandations selon le cas d'usage

| Cas d'usage | Recommandation |
|---|---|
| Prompts de 30 000 tokens, traitement temps réel | Mistral Large 3 (rapport qualité/prix) ou DeepSeek R1 |
| Traitement en volume, pas de besoin de temps réel | OpenAI ou Mistral Batch API à -50% |
| Besoin d'informations fraîches / web search | Perplexity Sonar |
| Explorer plusieurs modèles sans engagement fort | Mammouth (abonnement mensuel) |
| Coût le plus bas possible | Mistral Small 3.1 ou DeepSeek V3.2 |
| Qualité maximale, sans compromis | OpenAI GPT-5.4 ou Gemini 3.1 Pro |

---

## 7. Points de vigilance

- Les prix des API LLM évoluent fréquemment : vérifier les pages officielles avant tout déploiement.
- Le coût de la sortie est souvent sous-estimé : pour des réponses longues, il peut représenter une grande partie de la facture.
- Le cache de contexte proposé par certains fournisseurs peut réduire les coûts sur les prompts avec un préfixe répété.
- Les remises contractuelles existent parfois mais ne sont pas toujours publiques.

---

*Sources vérifiées en avril 2026. Tarifs indicatifs — consulter les pages officielles pour les prix en vigueur.*
