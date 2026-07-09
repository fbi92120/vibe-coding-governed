Version : 1.0
Date : 2026-06-30 18:15

# Leçons méthode issues du projet enex2obsidian

Auteur : François Biller (rédigé en co-construction Claude.ai Opus 4.7)
Objet : matière première pour la rédaction éventuelle d'une V7.5 de METHODE_SPECS_CO-CONSTRUCTION.md
Base : METHODE_SPECS_CO-CONSTRUCTION.md V7.4 du 2026-05-10

---

## Pourquoi ce document

Le projet enex2obsidian a produit sept observations méthodologiques qui ne sont pas ou pas complètement présentes dans la V7.4. Ce document les cristallise pendant qu'elles sont fraîches, sans engager la rédaction de la V7.5 elle-même.

Il est destiné à être relu au démarrage d'une session dédiée à la V7.5 — session qui pourra alors :
- Valider ou invalider chaque item
- Trancher son statut (prêt / à mûrir)
- Rédiger la formulation méthode définitive

Ce document n'est pas la V7.5. Il est le brief pour la session qui la produira.

## État de la V7.4 face à ces leçons

Une partie des leçons enex2obsidian est déjà couverte par la V7.4 et n'appelle aucune modification. En particulier :
- Le pattern BACKLOG.md (section 7.4) — matérialisation de la dette
- La règle "Ne jamais patcher : toute divergence remonte aux specs d'abord" (règles absolues avant de coder)
- Le signal 🚨 SPEC MANQUANTE (Annexe D)
- La cascade CLAUDE.md
- La gouvernance des discussions et le transfert de contexte
- La formule "C'est le code qui s'adapte aux tests" (Étape 8, Wiki LLM)
- Constitution avant code
- Ordre des tests (contrat avant, smoke après)

Ce qui suit est ce qui n'y est pas, ou pas complètement.

## Les sept items

### Item 1 — Récap structuré pré-commit obligatoire

Classification : prêt à intégrer en V7.5.

Description : toute session Claude Code qui produit des modifications de code doit se terminer par un récap structuré en sept rubriques (modifications code prod, tests, fichiers nouveaux, signaux SPEC MANQUANTE émis, découvertes non traitées, adaptations vs prompt initial, décompte final tests) avant de demander l'autorisation de commit. Sans ce récap, l'autorisation ne peut pas être donnée.

Origine : PROMPT-13 du projet enex2obsidian. Claude Code a effectué six adaptations silencieuses (assertions ajustées au comportement réel divergent des SPECS, sans émission des signaux 🚨 SPEC MANQUANTE prévus). La détection s'est faite par question subsidiaire ad hoc pré-commit ("as-tu fait des choix d'adaptation ?"), pas par rituel formalisé. Sans cette question, six divergences code/SPECS entraient en commit invisibles.

Pourquoi c'est méthode et pas ad hoc : la V7.4 pose bien le principe "ne jamais patcher, remonter aux specs" mais ne dit pas comment vérifier opérationnellement que Claude Code l'a respecté. Le récap structuré est le mécanisme opérationnel. Il transforme la vigilance humaine effective d'une dépendance à la bonne question au bon moment en un rituel imposé qui ne peut pas être sauté.

Portée universelle : oui. Le pattern "LLM implémenteur qui adapte silencieusement plutôt que de signaler" n'est pas spécifique au domaine ENEX. Il se reproduira sur tout projet impliquant un LLM implémenteur face à des specs strictes.

Formulation candidate pour V7.5 (à raffiner en session dédiée) :
> Toute session Claude Code produisant des modifications de code se termine par un récap structuré en sept rubriques avant demande de commit. Sans ce récap, l'autorisation ne peut pas être donnée. Le récap distingue : modifications code prod, modifications tests, fichiers nouveaux, signaux 🚨 SPEC MANQUANTE émis, découvertes non traitées, adaptations vs prompt initial, décompte final tests. Ce rituel remplace la vigilance ad hoc par une discipline systématique.

Emplacement suggéré dans V7.5 : nouvelle section dans la partie 6 (Gouvernance) ou en Annexe A (guide technique).

---

### Item 2 — Protocole "divergence code vs SPECS révélée par un test"

Classification : prêt à intégrer en V7.5.

Description : quand un test découvre qu'un code déjà commité diverge des SPECS, le comportement obligatoire est de signaler 🚨 SPEC MANQUANTE et stopper — pas d'adapter les assertions au comportement réel. C'est une variation du principe "C'est le code qui s'adapte aux tests" spécifique au cas où le test est écrit après le code (test d'intégration, smoke, limits).

Origine : même incident PROMPT-13 que l'Item 1, angle complémentaire. Six divergences dont deux violations Constitution règle 2 (perte silencieuse) ont été adaptées dans les assertions au lieu d'être signalées.

Pourquoi c'est distinct de l'item 1 : le récap structuré (item 1) est un rituel de fin de session. Ce protocole (item 2) est une règle en cours de session, appliquée au moment où la divergence est détectée. Les deux se renforcent mais couvrent des moments différents.

Pourquoi c'est distinct de "C'est le code qui s'adapte aux tests" (V7.4 étape 8) : la formule V7.4 s'applique au cas TDD, où le test est écrit avant. Le cas où le test est écrit après un code déjà commité soulève une nouvelle question : est-ce le code qui est buggé ou le test qui doit s'adapter à un choix de design ? La V7.4 ne tranche pas explicitement.

Portée universelle : oui. Applicable à toute stratégie de test qui combine TDD partiel (contrat avant module) et tests d'intégration écrits après.

Formulation candidate pour V7.5 :
> Si un test d'intégration écrit après un module découvre une divergence entre le comportement réel du code et ce que les SPECS décrivent, la règle est : signaler 🚨 SPEC MANQUANTE et stopper. Il est interdit d'ajuster les assertions au comportement réel — cela équivaut à valider un bug comme s'il était une feature. L'arbitrage humain tranche ensuite entre trois options : corriger le code, modifier les SPECS pour acter la divergence, ou marquer le test en pytest.xfail(strict=True) avec entrée BACKLOG.md.

Emplacement suggéré dans V7.5 : extension de l'Étape 8 ou section dédiée dans la partie 7 (Gouvernance après le MVP).

---

### Item 3 — Validation à trois étapes pour modules à interface format externe

Classification : prêt à intégrer en V7.5.

Description : pour tout module qui consomme ou produit un format externe (XML, JSON-Schema, base64, format YAML lu par un outil tiers), la validation avant commit comprend trois étapes obligatoires — pas seulement les tests unitaires : (1) tests unitaires sur fragments synthétiques, (2) audit externe (Codex, staff engineer, ou équivalent), (3) test empirique sur un fichier réel non synthétique.

Origine : incident V1.5 enex_parser dans le projet enex2obsidian. Le bug `huge_tree=False` bloquait tout parsing d'ENEX de plus de quelques Mo. Il n'est pas apparu sur les fragments XML inline des tests CT-01 (petits, bien formés). Il n'est apparu que sur cyber.enex (91 Mo réel). Le bug était structurellement invisible aux tests synthétiques.

Pourquoi c'est méthode : la V7.4 mentionne les tests de contrat et le test smoke, mais ne différencie pas les modules selon leur exposition à un format externe. Or c'est précisément cette exposition qui rend un module vulnérable à des bugs qui n'apparaissent que sur données réelles. La règle est une reconnaissance de cette asymétrie.

Portée universelle : oui, mais restreinte au cas "consomme ou produit un format externe". Ne s'applique pas à un module de logique métier pure.

Formulation candidate pour V7.5 :
> Un module qui consomme ou produit un format externe documenté (XML, JSON-Schema, protocole binaire, DTD, format YAML lu par un outil tiers) suit une validation à trois étapes obligatoires avant commit :
> (1) Tests unitaires sur fragments synthétiques — vérifient le contrat.
> (2) Audit externe (Codex, /review, ou équivalent) — détecte les bugs de structure et de sécurité.
> (3) Test empirique sur un fichier réel non synthétique — détecte les particularités du format réel que les fragments ne couvrent pas.
> Cette règle est extraite d'un incident où un bug bloquant n'a été révélé que par un fichier réel de 91 Mo, après avoir passé tous les tests sur fragments.

Emplacement suggéré dans V7.5 : Annexe A (guide technique), section dédiée dans "Tests de contrat".

---

### Item 4 — Inspection visuelle obligatoire pour outputs consommés visuellement

Classification : prêt à intégrer en V7.5.

Description : pour tout projet dont l'output est consommé visuellement par un humain dans un outil cible (Obsidian, Word, browser, PDF viewer), l'inspection visuelle dans cet outil est une étape de validation obligatoire — pas un nice-to-have post-livraison. L'échantillon inspecté doit contenir des caractères Unicode non-ASCII pour couvrir les bugs de normalisation invisibles en ASCII.

Origine : bug NFC/NFD révélé à l'étape 11 du projet enex2obsidian. cyber.enex a été migré, tous les tests automatisés ont passé (23 notes → 23 .md, 113 PJ copiées), mais l'inspection visuelle dans Obsidian a révélé que les liens vers les 5 PDFs au nom accentué étaient cassés (création de fichiers vides au clic). Le bug NFC/NFD est structurellement invisible aux tests automatisés qui utilisent les mêmes chaînes de caractères pour écrire et pour vérifier.

Pourquoi c'est méthode : les tests automatisés vérifient la structure et la complétude des outputs, mais ne révèlent pas les bugs de rendu propres à l'outil cible. Aucun test unitaire n'aurait révélé le bug NFC/NFD. Seul l'ouverture du vault dans Obsidian avec clic sur un lien accentué l'a révélé.

Portée universelle : oui, restreinte au cas "output consommé dans un outil externe". Ne s'applique pas à un outil qui produit du JSON pour un autre programme.

Formulation candidate pour V7.5 :
> Un projet qui produit un output consommé visuellement par un humain dans un outil cible (Obsidian, Word, browser, PDF viewer) intègre l'inspection visuelle dans cet outil comme étape de validation obligatoire — pas comme vérification post-livraison. L'échantillon inspecté doit contenir des caractères Unicode non-ASCII : les bugs de normalisation (NFC/NFD, encodage) sont systématiquement masqués par un échantillon ASCII pur. Un module n'est pas considéré comme validé tant qu'un échantillon de sa sortie n'a pas été ouvert dans l'outil cible et inspecté visuellement.

Emplacement suggéré dans V7.5 : nouvelle sous-section dans l'Étape 8 (tests), en parallèle des tests de contrat et smoke.

---

### Item 5 — Représentativité de l'échantillon empirique

Classification : à laisser mûrir sur un projet supplémentaire.

Description : le fichier réel utilisé pour le test empirique doit être représentatif du corpus cible. Un test empirique sur un carnet "knowledge" (bookmarks, captures web) ne valide pas le comportement attendu sur un corpus "admin" (PDFs reçus, scans).

Origine : incident V1.6 attachment_handler dans enex2obsidian. cyber.enex (knowledge) a passé les tests avec 114 PJ traitées incluant des ressources web parasites (SVG inline, images de tracking, ressources sans file-name). Sur un carnet admin, ces phénomènes n'existent pas — les PJ sont des PDFs reçus. La règle MIME allowlist V1.6 a été introduite pour corriger.

Pourquoi laisser mûrir : cette leçon est vraie, mais c'est peut-être un cas particulier d'un principe plus général — "tester sur des données proches du contexte de production". Ce n'est pas neuf. La V7.4 dit déjà "hypothèse technique risquée à tester avant de spécifier" (Étape 4). La question ouverte : cette leçon mérite-t-elle une entrée méthode dédiée, ou est-elle une note à ajouter à l'Étape 4 existante ?

À vérifier sur un projet supplémentaire : est-ce que la non-représentativité de l'échantillon est un piège qui se reproduit sous différentes formes ? Si oui, elle mérite un traitement méthode. Si c'est un cas isolé enex2obsidian, une note dans l'Étape 4 suffit.

---

### Item 6 — Consultation documentaire des formats sources en cadrage

Classification : à laisser mûrir sur un projet supplémentaire.

Description : pour tout projet qui consomme ou produit un format externe documenté, la consultation de la documentation officielle du format (DTD, XSD, JSON Schema, spec éditeur) est une étape de cadrage avant écriture des SPECS — pas une vérification post-implémentation. Elle doit produire un diff explicite entre les champs du format et le scope du projet, chaque champ hors scope étant tracé avec une décision documentée.

Origine : dans enex2obsidian, la consultation documentaire du format ENEX a été faite à mi-parcours (entre étapes 7 et 8 sur 14), pas en cadrage. Conséquences : V1.5 (huge_tree=True) et V1.6 (MIME allowlist) sont des amendements correctifs post-commit. L'amendement V1.7 documente le diff exhaustif rétroactivement.

Pourquoi laisser mûrir : cette leçon est vraie, mais elle chevauche l'Étape 4 (hypothèse technique risquée) et l'Étape 6 (audit pré-rédaction) de la V7.4. La question ouverte : est-ce une étape à part entière (Étape 4bis "cadrage documentaire"), ou une extension de l'Étape 4 existante, ou une note à ajouter à l'Étape 6 ?

À vérifier sur un projet supplémentaire : cette étape gagne-t-elle à être une étape à part entière, ou est-ce une pratique implicite qui s'intègre naturellement dans les étapes existantes ? Un projet de plus le dira.

---

### Item 7 — Cycles de correction X-AUDIT, X-FIX comme pattern accepté

Classification : à laisser mûrir sur un projet supplémentaire.

Description : la règle V7.4 "1 prompt = 1 module = 1 test avant de passer au suivant" peut se lire strictement (un seul prompt par module) ou souplement (un prompt principal plus des sub-prompts audit et fix associés). Dans enex2obsidian, la séquence réelle a été 12 → 12-AUDIT → 12-FIX → 13 → 13-FIX. Chaque sub-cycle est un livrable testable en soi, mais reste rattaché à son étape macro.

Origine : introduction d'un audit LLM tiers (Codex CLI) dans la boucle. La V7.4 mentionne `/review` (staff engineer intégré à Claude Code) comme dernier geste avant de passer au module suivant. Elle ne mentionne pas l'audit par un LLM tiers indépendant.

Pourquoi laisser mûrir : deux questions ouvertes. Premièrement, l'audit LLM tiers doit-il devenir une pratique standard ou reste-t-il une pratique spécifique aux projets à fort enjeu de conformité ? Deuxièmement, la formulation "1 prompt = 1 module" mérite-t-elle d'être nuancée en "1 sujet = 1 module, 1 à N prompts dont un principal et des correctifs si nécessaire" ?

À vérifier sur un projet supplémentaire : est-ce que le pattern X-AUDIT / X-FIX est structurel (émerge dès qu'il y a un audit externe) ou anecdotique (enex2obsidian a eu deux corrections parce que les tests d'intégration étaient particulièrement volumineux) ?

## Synthèse pour la session V7.5

Quatre items sont classés prêts à intégrer en V7.5 :
1. Récap structuré pré-commit obligatoire
2. Protocole "divergence code vs SPECS révélée par un test"
3. Validation à trois étapes pour modules à interface format externe
4. Inspection visuelle obligatoire pour outputs consommés visuellement

Trois items sont classés à laisser mûrir sur un projet supplémentaire :
5. Représentativité de l'échantillon empirique
6. Consultation documentaire des formats sources en cadrage
7. Cycles de correction X-AUDIT, X-FIX comme pattern accepté

Le passage V7.4 → V7.5 est un raffinement, pas une refonte. La structure existante de la V7.4 (les 9 étapes, la section 7 gouvernance, les Annexes A à D) est saine et n'est pas remise en cause. Les quatre items s'insèrent dans les emplacements suggérés pour chacun.

## Recommandation opérationnelle pour la session V7.5

1. Ouvrir une nouvelle discussion Claude.ai dans le projet vibe-coding-governed (pas dans le projet enex2obsidian). Le contexte doit être celui de la méthode, pas d'un projet applicatif.
2. Documents à joindre à la session : METHODE_SPECS_CO-CONSTRUCTION.md V7.4, ce fichier LEÇONS-METHODE-ENEX2OBSIDIAN.md, CLAUDE.md V1.8 d'enex2obsidian (pour référence des formulations déjà éprouvées).
3. Prompt de démarrage :
```
Je souhaite rédiger la V7.5 de METHODE_SPECS_CO-CONSTRUCTION.md.
Base : la V7.4 jointe.
Matière première : le document LEÇONS-METHODE-ENEX2OBSIDIAN.md joint
identifie 4 items prêts à intégrer et 3 items à laisser mûrir.
Objectif : raffiner la V7.4 en V7.5 avec les 4 items validés,
sans refonte structurelle. Je suis en Opus 4.7 effort high.
```
4. Attendre l'arbitrage Claude.ai sur chaque item avant de rédiger. La méthode elle-même impose de ne pas produire avant que la pensée soit complète (Étape 7 V7.4).

## Limites de ce document

Ce document est produit en fin de session dense sur enex2obsidian. Il porte le biais d'une expérience unique. Il ne remplace pas une session dédiée à la V7.5 — il est la matière première de cette session.

Les formulations candidates proposées sont des points de départ, pas des versions finales. Elles doivent être re-évaluées à froid.

Les classifications "prêt / à mûrir" sont mon jugement à un instant t. Elles peuvent être révisées dans la session dédiée à la V7.5 — un item classé "à mûrir" peut être promu si l'argumentaire est solide, un item classé "prêt" peut être renvoyé au mûrissement si la portée universelle est contestée.

## Emplacement suggéré pour ce fichier

Deux options :
1. Dans le repo vibe-coding-governed, à la racine, à côté de METHODE_SPECS_CO-CONSTRUCTION.md. Avantage : co-localisation avec la méthode. Inconvénient : mélange matière première et livrable final.
2. Dans `~/Migration-Evernote/`, à côté de CONTEXTE-PROJET.md. Avantage : reste dans l'espace de travail d'enex2obsidian d'où est extraite la leçon. Inconvénient : moins visible depuis vibe-coding-governed.

Recommandation : option 1 (repo vibe-coding-governed), avec suppression du fichier une fois la V7.5 rédigée et commitée. Le fichier a un cycle de vie limité : matière première pour V7.5, puis obsolète.
