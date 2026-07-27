# STATS — vibe-coding-governed

*Mesures générées le 2026-07-26. Commandes à exécuter depuis la racine du dépôt.*

## vibe-coding-governed

- **Date du premier commit** : 2026-04-11
  `git log --reverse --format='%ad' --date=short | head -1`
- **Date du dernier commit** : 2026-07-17
  `git log -1 --format='%ad' --date=short`
- **Nombre total de commits** : 21
  `git rev-list --count HEAD`
- **Nombre de jours calendaires distincts avec au moins un commit** : 10
  `git log --format='%ad' --date=short | sort -u | wc -l`
- **Durée calendaire entre premier et dernier commit** : 97 jours
  `git log --format='%at' | sort -n | awk 'NR==1{f=$1} END{printf "%d jours\n", ($1-f)/86400}'`
- **Plus longue interruption entre deux commits** : 60,68 jours
  `git log --format='%at' | sort -n | awk 'NR>1{g=$1-p; if(g>m)m=g} {p=$1} END{printf "%.2f jours\n", m/86400}'`
> **Note — métriques git.** Valeurs ci-dessus mesurées le 2026-07-26. L'historique a été réécrit le 2026-07-27 (publication, `git filter-repo`), donc l'état git courant diffère : **25 commits, dernier commit 2026-07-27, 11 jours actifs**. Fenêtre d'activité réelle : **2026-04-11 → 2026-07-17**.

- **Nombre de fichiers de code, et lignes de code par langage** : 0 fichier de code.
  Dépôt documentaire : aucun fichier `.py`, `.sh`, `.js`, `.ts`, `.html`, `.css`, `.yml`.
  `find . \( -name '*.py' -o -name '*.sh' -o -name '*.js' -o -name '*.yml' \) -not -path './.git/*' | wc -l`  → 0
- **Nombre de fichiers markdown de documentation, et lignes totales** : 12 fichiers, 2451 lignes (documentation rédigée ; inclut le fichier `STATS` produit pour cet exercice).
  `find . -name '*.md' -not -path './.git/*' | wc -l`
  `find . -name '*.md' -not -path './.git/*' -print0 | xargs -0 cat | wc -l`
  Note inter-dépôts : `METHODE_SPECS_CO-CONSTRUCTION.md` (911 lignes) et `CLAUDE.projects.md` (115 lignes) sont aussi présents en copie dans yt-knowledge-extractor ; dans un total portefeuille, ne les compter qu'une fois (ce dépôt est leur emplacement de référence).
- **Nombre de tests, et commande utilisée pour les compter** : non mesurable — dépôt documentaire, aucun code exécutable ni fichier de test.
  `grep -rE '^\s*def test_' --include='*.py' . | wc -l`  → 0
- **Part du code réservée aux tests** : non mesurable — 0 ligne `.py`, donc pas de dénominateur.
- **Taille moyenne d'une fonction Python (hors test)** : non mesurable — aucun fichier `.py`.
- **Le code est-il commenté / documenté ?** : sans objet — dépôt documentaire, aucun code.
- **Version courante déclarée, si elle figure quelque part** :
  - `METHODE_SPECS_CO-CONSTRUCTION.md` : Version 7.4 — `grep -m1 -i version METHODE_SPECS_CO-CONSTRUCTION.md`
  - `CLAUDE.global.md` : Version 2.0
  - `CLAUDE.projects.md` : Version 1.1
  - `BACKLOG.global.md` : Version 1.10 · `BACKLOG.projects.md` : Version 1.3 · `BACKLOG.md` : Version 1.2

---

