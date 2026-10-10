# Les prompts de construction

Tous les messages que j'ai envoyés à Claude Code pour construire et tester l'agent, dans l'ordre. Les prompts permanents de l'agent sont dans [`CLAUDE.md`](../CLAUDE.md), [`.claude/skills/`](../.claude/skills/) et [`taches-planifiees/`](../taches-planifiees/).

## Sommaire

1. [Les instructions de départ (version 1)](#1-les-instructions-de-départ-version-1)
2. [Vérifier que `CLAUDE.md` est lu](#2-vérifier-que-claudemd-est-lu)
3. [Vérifier Notion](#3-vérifier-notion)
4. [Créer les skills](#4-créer-les-skills)
5. [Faire évoluer `CLAUDE.md`](#5-faire-évoluer-claudemd)
6. [Tester les rappels](#6-tester-les-rappels)
7. [Créer les tâches planifiées](#7-créer-les-tâches-planifiées)
8. [Corrections ponctuelles](#8-corrections-ponctuelles)

## 1. Les instructions de départ (version 1)

Les modes d'entraînement et de mémorisation tels qu'ils étaient avant la reconstruction. Je les garde ici pour montrer l'évolution.

```
# Mode 2 : M'entraîner
Quand je dis « teste-moi », « quiz » ou « exercice » :
- Varie les formats : QCM (4 choix), vrai ou faux à justifier, questions
  ouvertes, « explique avec tes mots », cas pratiques, détection d'erreur.
- Une seule question à la fois. Attends ma réponse, ne donne jamais la solution
  avant.
- Commence par des questions de vocabulaire et de définition. Passe à
  « expliquer », puis « appliquer », seulement quand les définitions sont sûres.
- Corrige chaque réponse : note sur 5, ce qui est juste, ce qui manque, puis la
  réponse modèle en langage simple.
- Reviens plus tard dans la séance sur les notions ratées, formulées autrement.
- Termine par un bilan : notions maîtrisées, fragiles, à revoir.

# Mode 3 : Mémoriser
- Tiens à jour la base Notion « Suivi de révision » : statut, dernière note,
  palier, dernière révision, prochaine révision. Ajoute chaque nouvelle notion
  vue en cours, avec sa définition simple.
- Rythme : J+1, J+3, J+7, J+14, J+30. Note de 4 ou 5 : palier suivant. Note de
  3 ou moins : retour à J+1.
- Au début de chaque séance, consulte la base et commence par les notions dont
  la date de prochaine révision est atteinte, mélangées au cours du jour.
```

Dans la version actuelle, ces deux modes sont devenus quatre sections : Test de cours, Évaluation, Mémorisation et Suivi.

## 2. Vérifier que `CLAUDE.md` est lu

```
Qui es-tu et quelles sont tes règles ?
```

Résultat attendu : l'agent se présente comme mon professeur d'IA, me tutoie, cite mon niveau et les modes. S'il répond de façon générale (« je suis Claude, un assistant »), le fichier n'est pas lu.

## 3. Vérifier Notion

Savoir ce qui est branché :

```
Quels connecteurs sont configurés dans cette session ?
```

Tester la lecture, sans risque :

```
Dans le teamspace Aivancity de Notion, trouve la page « Séminaire de rentrée » et donne-moi son titre et ses trois premières lignes. Ne modifie rien.
```

Premier point de suivi, toujours sans écriture :

```
Oui, fais le point de suivi, mais ne modifie rien dans Notion.
```

## 4. Créer les skills

Le dossier `.claude` est caché dans le Finder (son nom commence par un point). J'ai donc demandé à Claude Code de créer les fichiers lui-même.

### `/quiz`

```
Crée le fichier .claude/skills/quiz/SKILL.md avec exactement ce contenu :

---
description: Lance un test de cours (une question à la fois) sur un cours aivancity. À utiliser quand Lamia dit « teste-moi », « quiz » ou « exercice ».
---

Lance le « Mode 2 : Test de cours » défini dans CLAUDE.md sur le cours suivant : $ARGUMENTS

1. Si aucun cours n'est indiqué, demande lequel.
2. Lis la page du cours en entier dans le teamspace Notion « Aivancity ».
3. Applique toutes les règles du Mode 2, puis la section « Suivi ».
```

`$ARGUMENTS` est un emplacement vide : ce que j'écris après `/quiz` vient s'y placer. Avec `/quiz Séminaire de rentrée`, l'agent lit « sur le cours suivant : Séminaire de rentrée ».

Après la création du tout premier skill, il faut ouvrir une nouvelle session : le dossier des skills n'existait pas au démarrage, donc la session en cours ne le surveille pas.

### `/evaluation`, ma première version

```
Crée le fichier .claude/skills/evaluation/SKILL.md avec exactement ce contenu :

---
description: Lance une évaluation (plusieurs questions directement) sur plusieurs cours présent dans le teamspace aivancity. À utiliser quand Lamia dit « évalue-moi », « contrôle » ou « évaluation ».
---

Lance le « Mode 3 : évaluation » défini dans CLAUDE.md sur le cours suivant : $ARGUMENTS

1. Si aucun cours n'est indiqué, demande si je veux certains cours ou tous.
2. Lis la page des cours en entier dans le teamspace Notion « Aivancity ».
3. Applique toutes les règles du Mode 3, puis la section « Suivi ».
```

### `/evaluation`, version corrigée

```
Crée le fichier .claude/skills/evaluation/SKILL.md avec exactement ce contenu :

---
description: Lance une évaluation (plusieurs questions à la suite, correction à la fin) sur un cours ou sur tous les cours étudiés. À utiliser quand Lamia dit « évalue-moi », « contrôle » ou « évaluation ».
---

Lance le « Mode 3 : Évaluation » défini dans CLAUDE.md sur le périmètre suivant : $ARGUMENTS

1. Si aucun périmètre n'est indiqué, demande à Lamia : un cours précis ou tous les cours étudiés.
2. Si le périmètre est « tous », prends uniquement les cours présents dans la base Notion « Suivi de révision ».
3. Lis en entier les pages des cours concernés dans le teamspace Notion « Aivancity ».
4. Applique toutes les règles du Mode 3, puis la section « Suivi ».
```

Ce qui a changé : « je » remplacé par « Lamia », « cours » par « périmètre », et une étape qui définit « tous les cours ».

### `/complete` et `/revise`

```
Crée deux fichiers avec exactement les contenus suivants.

Premier fichier : .claude/skills/complete/SKILL.md

---
description: Analyse et complète les notes d'un cours aivancity (erreurs, imprécisions, manques). À utiliser quand Lamia dit « complète » ou « reprends mon cours ».
---

Lance le « Mode 1 : Compléter les notes » défini dans CLAUDE.md sur le cours suivant : $ARGUMENTS

1. Si aucun cours n'est indiqué, demande à Lamia lequel.
2. Lis la page du cours en entier dans le teamspace Notion « Aivancity ».
3. Applique toutes les règles du Mode 1.
4. Ne modifie jamais le texte de Lamia. N'écris dans Notion qu'après son accord explicite, uniquement dans la section « Compléments de l'assistant ».
5. Ajoute les nouvelles notions à la base « Suivi de révision », comme le prévoit la section « Suivi ».

Second fichier : .claude/skills/revise/SKILL.md

---
description: Lance une séance de mémorisation (méthode des J) sur les notions dont la date de révision est atteinte, tous cours mélangés. À utiliser quand Lamia dit « révise », « révision » ou « notions dues ».
---

Lance le « Mode 4 : Mémorisation » défini dans CLAUDE.md.

1. Consulte la base Notion « Suivi de révision » et liste les notions dont la date de prochaine révision est atteinte ou dépassée.
2. S'il n'y en a aucune, dis-le à Lamia et propose un test de cours ou une évaluation.
3. Sinon, applique toutes les règles du Mode 4, puis la section « Suivi ».
```

`/revise` n'a pas de `$ARGUMENTS` : ce sont les dates de la base qui choisissent les notions, pas moi. L'étape 4 de `/complete` répète volontairement une règle de `CLAUDE.md`, car c'est la seule commande qui écrit dans mes notes de cours.

## 5. Faire évoluer `CLAUDE.md`

### Ne traiter que ce qui est nouveau

```
Dans CLAUDE.md, ajoute ce point 6 à la fin du « Mode 1 : Compléter les notes », sans rien modifier d'autre :

6. Ne traite que ce qui est nouveau. Avant d'analyser, lis la section « Compléments de l'assistant » de la page et la base « Suivi de révision » : ignore les passages déjà complétés et les notions déjà enregistrées. Quand tu écris des compléments, indique la date du jour en tête de ton ajout, pour retrouver où tu t'es arrêté la fois suivante.
```

### Changer une heure

```
Dans CLAUDE.md, section « Planification des séances », remplace « à partir de 18 h » par « à partir de 17 h 30 ».
```

La formule « sans rien modifier d'autre » et le nom exact de la section évitent que l'agent réécrive plus que demandé. Pour les ajouts plus longs (emploi du temps, planification, suivi par chapitre, rappels), j'ai remplacé le fichier entier par la nouvelle version.

## 6. Tester les rappels

```
Crée un rappel de test dans la liste « Révisions IA » pour dans 5 minutes, avec le titre « Test du professeur IA ».
```

Deux permissions s'affichent : celle de Claude Code (on y voit `osascript`), puis celle de macOS pour contrôler l'application Rappels.

## 7. Créer les tâches planifiées

`rappel-revision` a été créée par le menu **Routines** > **New routine** > **Local**. Ses instructions sont dans [`rappel-revision.md`](../taches-planifiees/rappel-revision.md).

`preparation-complements` a été créée en la décrivant dans une session :

```
Crée une tâche planifiée locale nommée « preparation-complements », qui se lance tous les vendredis et tous les samedis à 17 h 30, dans le dossier professeur-ia, en mode de permission Manual, avec ces instructions :

1. Vérifie avec « Mon emploi du temps » de CLAUDE.md si aujourd'hui est un jour de cours. Si ce n'est pas le cas, dis-le en une phrase et arrête-toi.
2. Cherche dans le teamspace Notion « Aivancity » les pages de cours modifiées aujourd'hui. S'il n'y en a aucune, dis-le en une phrase et arrête-toi.
3. Applique le Mode 1 sur ces pages, uniquement sur ce qui est nouveau.
4. Présente tes compléments dans la conversation, classés en ERREUR, IMPRÉCISION et MANQUE.
5. N'écris rien dans Notion. Termine par : « Réponds "ok" pour que j'écrive ces compléments dans Notion, ou dis-moi ce que tu veux changer. »
```

Les questions que Claude Code m'a posées en retour et ce qu'elles ont changé sont dans [`preparation-complements.md`](../taches-planifiees/preparation-complements.md).

## 8. Corrections ponctuelles

Aligner le nom d'une section dans un skill :

```
Dans .claude/skills/complete/SKILL.md, remplace « Compléments du prof » par « Compléments de l'assistant ».
```

Et sur la page Notion :

```
Sur la page du cours d'introduction, renomme la section « Compléments du prof » en « Compléments de l'assistant ». Ne change rien d'autre.
```

Déplacer un rappel :

```
Déplace mon rappel de ce soir à 17 h 45.
```

Annuler des notes écrites pendant un test technique :

```
Les notes de Softmax et Logit du 10/10 venaient d'un test technique. Remets ces deux notions dans leur état d'avant, au palier J+1, jamais interrogées.
```

## Ce que ces prompts ont en commun

- **Le contenu exact** quand il s'agit d'un fichier : « avec exactement ce contenu ».
- **Une limite explicite** : « ne modifie rien », « sans rien modifier d'autre », « ne change rien d'autre ».
- **Les noms exacts** des sections, des fichiers, des bases et des listes, entre guillemets.
- **Un test après chaque création**, avant de passer à la suite.
