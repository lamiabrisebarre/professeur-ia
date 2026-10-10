# 2. La réflexion

Ce qui manquait dans le [premier prompt](1-prompt-v1.md), et ce que j'ai décidé pour arriver au [prompt final](../CLAUDE.md).

## Six constats, six décisions

| Constat | Décision |
|---|---|
| Un seul mode de test, alors que j'avais deux besoins : apprendre un cours et mesurer mon niveau | Trois modes distincts : test de cours, évaluation, mémorisation |
| Le texte parlait de moi à la troisième personne, avec des passages ambigus | Tout réécrire à la première personne |
| La méthode des J donne des dates fixes, mais je travaille en alternance | Ajouter mon emploi du temps et des créneaux de révision |
| L'agent notait chaque notion, sans vue d'ensemble | Un bilan par cours et par chapitre |
| L'agent attendait que je vienne le voir | Des rappels sur mon Mac et deux tâches planifiées |
| Je devais lancer moi-même le complément de mes notes | Les compléments sont préparés automatiquement, je n'ai plus qu'à valider |

## Les trois modes de test

| | Test de cours | Évaluation | Mémorisation |
|---|---|---|---|
| But | Apprendre | Mesurer | Retenir |
| Périmètre | Un cours | Un cours ou tous | Les notions dues |
| Rythme | Une question à la fois | 10 questions à la suite | Plusieurs à la suite |
| Correction | Tout de suite | À la fin, note sur 20 | À la fin |
| Cours mélangés | Non | Au choix | Oui |

## Les compromis assumés

- **Des dates « au plus tôt ».** Une révision prévue à J+3 a lieu au premier créneau libre à partir de cette date. Une révision décalée d'un jour reste plus utile qu'une révision sautée.
- **Pas d'écriture automatique dans mes notes.** L'agent prépare ses compléments dans un brouillon et attend mon accord. Une erreur de sa part entrerait sinon dans mes notes sans que je la voie.
- **Des tâches locales plutôt que dans le cloud.** Elles ont besoin de mon dossier de projet, donc de mon Mac allumé.

## Ce que l'écriture m'a appris

Ma première version du skill `/evaluation` contenait trois défauts typiques :

1. **« Demande si je veux… »** : c'est l'agent qui lit le fichier, le « je » est ambigu.
2. **« Sur le cours suivant »** alors que l'évaluation peut porter sur plusieurs cours.
3. **« Tous les cours »** sans dire lesquels : l'agent aurait pu m'interroger sur des cours pas encore étudiés.

À chaque fois, un humain aurait compris, mais un agent doit deviner. J'ai relu le prompt final avec cette question : où l'agent doit-il deviner ? Quatre précisions en sont sorties.

| Situation floue | Précision ajoutée |
|---|---|
| La tâche de préparation n'a pas tourné | « Propose-moi /complete à la place » |
| Une tâche planifiée est-elle une « séance » ? | Non : elle ne suit que ses propres instructions |
| Quand réviser une notion pour la première fois ? | Le lendemain du cours |
| À quelle heure créer le rappel ? | À l'heure de début du créneau |

La suite : [le prompt final](../CLAUDE.md), puis [les skills](../.claude/skills/) et [les tâches planifiées](3-taches-planifiees.md).
