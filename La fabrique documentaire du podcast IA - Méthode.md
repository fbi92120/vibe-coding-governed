Je vous partage une méthode que j’expérimente pour transformer une grosse base documentaire en une série de podcasts pédagogiques avec ChatGPT et NotebookLM.

Le principe n’est pas de déposer toute la documentation dans NotebookLM et de lui demander un résumé audio global. Je confie d’abord la base de connaissances à ChatGPT pour qu’il l’analyse et la découpe en épisodes cohérents.

La chaîne de travail est donc :

Base documentaire volumineuse → analyse et découpage par ChatGPT → sélection d’un sous-corpus → génération du podcast par NotebookLM → écoute et contrôle humain.

### 1. Je fournis la base documentaire

La source peut être un rapport volumineux, un ensemble de notes, des articles, des transcriptions ou plusieurs documents déjà consolidés.

Je précise à ChatGPT :

– à qui les podcasts sont destinés ;  
– ce que je veux apprendre ou comprendre ;  
– le niveau technique attendu ;  
– la durée souhaitée ;  
– les sujets prioritaires ;  
– les informations qui ne doivent pas être divulguées.

### 2. ChatGPT cartographie la source

ChatGPT analyse la base documentaire pour identifier :

– les grands thèmes ;  
– les concepts structurants ;  
– les acteurs et leurs positions ;  
– les affirmations importantes ;  
– les controverses ;  
– les contradictions entre sources ;  
– les sujets qui méritent un épisode autonome.

Cette étape sert à transformer une grosse masse de connaissances en parcours d’apprentissage.

### 3. ChatGPT découpe la base en épisodes

Pour chaque épisode, ChatGPT propose :

– une question directrice ;  
– les parties précises de la source à utiliser ;  
– les notions à expliquer ;  
– les deux perspectives à confronter ;  
– les erreurs ou simplifications à éviter ;  
– les conclusions attendues.

Par exemple, à partir d’un dossier fictif consacré à la gouvernance des systèmes agentiques :

– épisode 1 : à quoi sert une architecture agentique ?  
– épisode 2 : où l’humain doit-il intervenir ?  
– épisode 3 : comment contrôler les actions réalisées par les agents ?  
– épisode 4 : quelles traces conserver pour auditer le système ?  
– épisode 5 : jusqu’où peut-on déléguer une décision ?

Chaque épisode n’utilise donc qu’une partie choisie de la base documentaire.

### 4. ChatGPT prépare les consignes pour NotebookLM

Exemple :

« Produis un podcast analytique en français sous la forme d’un dialogue entre deux intervenants.

Le premier explique les bénéfices et les conditions de réussite du dispositif étudié. Le second teste ces arguments, relève les limites, les risques et les incertitudes.

Distingue les faits établis, les déclarations des acteurs, les analyses de tiers et les hypothèses. Appuie les arguments uniquement sur les sources fournies. N’invente aucun chiffre, exemple ou résultat.

Lorsque les sources divergent, expose les différentes positions sans les réconcilier artificiellement.

Explique les termes techniques à leur première apparition. Termine par trois enseignements essentiels, deux incertitudes et cinq questions restant à examiner. »

### 5. NotebookLM génère le dialogue audio

NotebookLM produit ensuite le podcast à partir du sous-corpus et des consignes préparées.

Il ne lit pas un script mot à mot : il construit et reformule la discussion. Le résultat peut donc être naturel, mais il reste partiellement imprévisible.

### 6. Je contrôle le résultat avec ChatGPT

Après écoute, je peux signaler à ChatGPT les passages problématiques. Il m’aide à rechercher :

– les informations inventées ;  
– les hypothèses transformées en faits ;  
– les nuances supprimées ;  
– les contradictions escamotées ;  
– les affirmations impossibles à rattacher aux sources ;  
– les simplifications qui changent le sens du corpus.

ChatGPT peut alors modifier le découpage, réduire le corpus ou renforcer les consignes avant une nouvelle génération.

L’idée centrale est donc la suivante : NotebookLM génère le podcast, mais ChatGPT réalise en amont le travail documentaire et éditorial qui rend l’épisode réellement utile.

L’humain reste responsable du choix de l’objectif, de la validation du découpage, de l’écoute critique et de la décision de conserver ou de recommencer l’épisode.