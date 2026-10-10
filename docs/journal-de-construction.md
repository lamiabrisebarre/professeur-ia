# Journal de construction

Comment j'ai construit l'agent, dans l'ordre, avec ce que j'ai modifié et pourquoi. Tout a été fait le 10 octobre 2026, en une après-midi, en avançant brique par brique : je ne passais à l'étape suivante que quand la précédente fonctionnait.

## Point de départ

L'agent existait déjà sous une forme simple : un projet dans l'application Claude, avec des instructions (rôle, niveau débutante, trois modes) et un accès à mes notes Notion. Il fonctionnait, mais il attendait que je vienne le voir et ne savait rien faire seul.

J'ai décidé de le reconstruire dans Claude Code pour deux raisons : apprendre comment un agent se construit, et ajouter de l'automatisation.

| Ce que j'avais | L'équivalent dans Claude Code |
|---|---|
| Instructions du projet | Un fichier `CLAUDE.md`, relu à chaque session |
| Connexion à Notion | Un connecteur (MCP, *Model Context Protocol*) |
| « teste-moi », « complète » | Des commandes personnalisées (*skills*) |
| Rien | Des tâches planifiées |

## Les étapes

### 1. Installer Claude Code

J'ai utilisé l'application de bureau Claude, onglet **Code**, plutôt que le terminal. C'est le même moteur avec une interface à boutons, et les fichiers restent compatibles si je passe au terminal plus tard.

### 2. Créer le dossier du projet

Un dossier `professeur-ia` sur mon Mac, ouvert en environnement **Local** et en mode de permission **Manual**.

Ce que j'ai retenu : l'agent n'a accès qu'au dossier que je lui désigne, pas à tous mes documents. Et le mode Manual me montre chaque action avant qu'elle se produise, ce qui est plus lent mais permet de comprendre ce que fait l'agent.

### 3. Écrire `CLAUDE.md`

J'ai repris mes instructions existantes dans un fichier `CLAUDE.md` à la racine du dossier.

Test : `Qui es-tu et quelles sont tes règles ?` L'agent a cité son rôle, mon niveau, les sources et les modes. Le fichier était bien lu.

### 4. Brancher Notion

Le connecteur était déjà lié à mon compte Claude. Je l'ai vérifié avec une lecture simple, en précisant « ne modifie rien » : on vérifie qu'un agent sait lire avant de le laisser écrire.

### 5. Créer les commandes

Quatre skills : `/quiz`, `/evaluation`, `/complete`, `/revise`. Chacun est un fichier `SKILL.md` dans `.claude/skills/<nom>/`. Le nom du dossier devient le nom de la commande.

J'ai créé `/quiz` à partir d'un modèle, puis rédigé `/evaluation` moi-même. Ma première version fonctionnait, mais contenait trois défauts (voir « Ce que j'ai corrigé »).

### 6. Les rappels sur le Mac

L'agent crée des rappels dans l'application Rappels grâce à AppleScript (`osascript`), dans une liste dédiée « Révisions IA ». Il ne touche à aucune autre liste.

Test : un rappel de test à 5 minutes. Deux permissions à accorder : celle de Claude Code pour lancer la commande, celle de macOS pour contrôler Rappels.

### 7. Les tâches planifiées

Deux tâches locales, détaillées dans [`taches-planifiees/`](../taches-planifiees/) :

- `rappel-revision`, tous les jours à 12 h : vérifie le planning et recrée le rappel manquant.
- `preparation-complements`, vendredi et samedi à 17 h 30 : prépare les compléments de mes notes du jour.

J'ai choisi des tâches locales plutôt que distantes (cloud), parce qu'elles ont besoin de `CLAUDE.md` et des skills, qui sont dans mon dossier. En contrepartie, le Mac doit être allumé.

## Les modifications et améliorations

### Séparer les tests en deux modes

**Avant** : un seul mode « M'entraîner », avec une question à la fois.

**Problème** : je voulais deux choses différentes. Des tests qui suivent le cours pour apprendre, et des évaluations plus générales pour mesurer mon niveau.

**Après** : trois modes distincts.

| | Test de cours | Évaluation | Mémorisation |
|---|---|---|---|
| But | Apprendre | Mesurer | Retenir |
| Périmètre | Un cours | Un cours ou tous | Les notions dues |
| Rythme | Une question à la fois | 10 questions à la suite | Plusieurs à la suite |
| Correction | Tout de suite | À la fin, note sur 20 | À la fin |
| Cours mélangés | Non | Au choix | Oui |

J'ai aussi ajouté une section « Suivi » commune à tous les modes : point en début de séance, mise à jour après chaque séance, repérage des notions ratées plusieurs fois.

### Réécrire les instructions à la première personne

J'ai réécrit `CLAUDE.md` avec « je » et « mon » au lieu de « Lamia » et « elle ». Il restait deux phrases à l'ancienne forme (« Elle répond à toutes », « avant de lui demander »), qui rendaient le texte ambigu. Elles ont été corrigées.

J'ai aussi changé deux règles : l'analogie n'est plus systématique, seulement quand la notion est compliquée, et les recherches web doivent toujours citer leurs sources.

### Ajouter mon emploi du temps

**Problème** : la méthode des J donne des dates fixes (J+1, J+3, J+7...), mais je travaille en alternance et je n'ai pas de temps libre tous les jours.

**Solution** : une section « Mon emploi du temps » avec une semaine de référence, à partir de laquelle l'agent calcule seul les semaines de cours suivantes. Et une section « Planification des séances » avec deux types de créneaux :

- soirs de cours : à partir de 17 h 45, 1 heure maximum ;
- autres soirs : à partir de 19 h, 15 à 30 minutes, 3 soirs par semaine au plus.

**Le compromis** : les dates de la méthode des J deviennent des dates au plus tôt. Une révision faite un ou deux jours plus tard reste plus efficace qu'une révision sautée.

### Suivre mes progrès par chapitre

**Avant** : l'agent notait chaque notion séparément.

**Après** : il tient aussi un bilan par cours et par chapitre (maîtrisé, fragile, à retravailler), avec la date et une phrase d'explication.

### Préparer les compléments automatiquement

**Avant** : je devais lancer `/complete` moi-même, et l'agent relisait toute la page à chaque fois.

**Après** :

- une règle 6 dans le Mode 1 : ne traiter que ce qui est nouveau, en datant chaque ajout ;
- une tâche planifiée qui prépare les compléments à 17 h 30 dans un fichier brouillon ;
- je valide à 17 h 45, et seulement alors l'agent écrit dans Notion.

J'ai écarté l'option où l'agent écrirait directement dans Notion : une erreur de sa part entrerait dans mes notes sans que je la voie.

### Quatre précisions pour que l'agent n'ait pas à deviner

En relisant le fichier complet, quatre endroits demandaient à l'agent de deviner :

| Situation floue | Précision ajoutée |
|---|---|
| La tâche de 17 h 30 n'a pas tourné | « Propose-moi /complete à la place » |
| Une tâche planifiée est-elle une « séance » ? | Non : elle ne suit que ses propres instructions |
| Quand réviser une notion pour la première fois ? | Palier J+1, le lendemain du cours |
| À quelle heure créer le rappel ? | À l'heure de début du créneau |

## Ce que j'ai corrigé

### Mon premier skill

Ma première version de `/evaluation` :

```
1. Si aucun cours n'est indiqué, demande si je veux certains cours ou tous.
2. Lis la page des cours en entier dans le teamspace Notion « Aivancity ».
```

Trois défauts :

1. **Le « je »** : c'est l'agent qui lit ce fichier. « Si je veux » est ambigu.
2. **« Le cours suivant »** alors que l'évaluation peut porter sur plusieurs cours. Le mot juste est « périmètre ».
3. **« Tous les cours »** n'était pas défini. L'agent aurait pu m'interroger sur des cours pas encore travaillés. Précision ajoutée : uniquement les cours présents dans « Suivi de révision ».

Les trois corrections portent sur des endroits où un humain aurait compris, mais où un agent doit deviner.

### Deux noms pour la même section

`CLAUDE.md` disait « Compléments de l'assistant », le skill `/complete` disait « Compléments du prof », et ma page Notion contenait une section à l'ancien nom. L'agent a compris que c'était la même chose, mais il a dû deviner. J'ai aligné les trois.

### Des notes de test dans la vraie base

En testant `/quiz`, j'ai écrit de vraies notes dans « Suivi de révision ». La tâche planifiée l'a remarqué et me l'a signalé. À retenir : une commande qu'on teste agit pour de vrai.

## Ce que j'ai appris sur les agents

- **Un agent est fait de quatre briques** : des instructions, des outils, des commandes et un déclencheur.
- **`CLAUDE.md` ou skill ?** `CLAUDE.md` contient ce qui vaut tout le temps (le rôle, le tutoiement, le niveau). Un skill contient une procédure qu'on déclenche à la demande.
- **Plus un agent travaille seul, moins on lui donne de droits d'écriture.** Mes tâches planifiées lisent Notion mais n'y écrivent pas.
- **On choisit les droits selon le coût d'une erreur.** Un mauvais rappel se supprime en deux secondes. Une date fausse dans le suivi passe inaperçue.
- **Un bon agent signale ses doutes** au lieu de trancher à ma place.
- **Le premier lancement se fait devant l'écran**, pour voir ce que fait la tâche et accorder les permissions.

## Pistes pour la suite

- Ajuster les règles après quelques semaines d'usage réel.
- Ajouter de nouveaux cours et vérifier que le mélange fonctionne en révision.
- Tester une tâche distante (cloud) pour ne plus dépendre du Mac allumé.
