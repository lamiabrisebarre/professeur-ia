# Tâche planifiée : rappel-revision

La ronde quotidienne. Elle vérifie que mon planning de révision est à jour et recrée le rappel manquant si j'ai sauté une séance.

## Réglages

| Champ | Valeur |
|---|---|
| Type | Locale (elle a besoin du dossier du projet, donc de mon Mac) |
| Nom | `rappel-revision` |
| Description | Vérifie chaque jour mon planning de révision et mes rappels |
| Dossier | `professeur-ia` |
| Mode de permission | Manual |
| Fréquence | Tous les jours à 12 h |

## Instructions

```
Vérifie mon planning de révision, sans me poser de question et sans lancer de quiz.

1. Consulte la base Notion « Suivi de révision » et liste les notions dont la date de prochaine révision est atteinte ou dépassée.
2. Regarde les rappels à venir dans la liste « Révisions IA » de l'application Rappels.
3. Si des notions sont dues, ou si aujourd'hui est un jour de cours, et qu'aucun rappel n'est prévu pour mon prochain créneau disponible, crée-le en suivant les règles « Planification des séances » et « Suivi » de CLAUDE.md.
4. Si un rappel existe déjà pour ce créneau, ne crée rien.
5. Ne modifie rien dans Notion.
6. Termine par un résumé en trois lignes : notions dues, prochaine séance prévue, rappel créé ou déjà présent.
```

## Pourquoi ces choix

- **« Sans me poser de question »** : personne n'est devant l'écran quand la tâche se lance. Une question resterait sans réponse et bloquerait la tâche.
- **Lecture seule sur Notion** : la tâche n'a rien de nouveau à noter, puisque je n'ai répondu à aucune question. Et une erreur dans mes paliers ou mes dates fausserait toute la méthode des J sans que je le voie.
- **Droit de créer des rappels** : un mauvais rappel se voit et se supprime en deux secondes.
- **12 h** : si j'ai sauté la séance de la veille, le rappel du soir même est recréé à temps.

## Premier lancement

Cliquer sur **Run now** juste après la création et choisir « always allow » pour chaque permission demandée (lecture de Notion, lecture et création de rappels). Sans cela, la tâche reste bloquée chaque jour en attendant une réponse.

## Exemple de résultat (premier lancement, 10 octobre 2026)

```
Notions dues : 20 notions, toutes du cours « Introduction à l'IA et aux IA génératives »,
toutes au palier J+1 et jamais interrogées. Demain, 6 autres s'ajoutent.
Prochaine séance : aujourd'hui samedi 10 octobre, 1 h maximum, car c'est un jour de cours.
Rappel : créé dans « Révisions IA », aucun n'existait pour ce créneau.
Rien n'a été modifié dans Notion.
```

La tâche a aussi signalé deux points qu'elle ne pouvait pas trancher seule (deux notions déjà notées dans la journée, un ancien rappel de test non coché) au lieu de décider à ma place.
