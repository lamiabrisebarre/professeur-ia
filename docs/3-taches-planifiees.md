# 3. Les tâches planifiées

Ce que l'agent fait sans moi, à heure fixe. Les deux tâches sont locales : elles tournent sur mon Mac, dans le dossier du projet, en mode de permission Manual.

## rappel-revision

Tous les jours à 12 h. Elle vérifie que mon planning est à jour et recrée le rappel manquant si j'ai sauté une séance.

```
Vérifie mon planning de révision, sans me poser de question et sans lancer de quiz.

1. Consulte la base Notion « Suivi de révision » et liste les notions dont la date de prochaine révision est atteinte ou dépassée.
2. Regarde les rappels à venir dans la liste « Révisions IA » de l'application Rappels.
3. Si des notions sont dues, ou si aujourd'hui est un jour de cours, et qu'aucun rappel n'est prévu pour mon prochain créneau disponible, crée-le en suivant les règles « Planification des séances » et « Suivi » de CLAUDE.md.
4. Si un rappel existe déjà pour ce créneau, ne crée rien.
5. Ne modifie rien dans Notion.
6. Termine par un résumé en trois lignes : notions dues, prochaine séance prévue, rappel créé ou déjà présent.
```

## preparation-complements

Les vendredis et samedis à 17 h 30. Elle lit mes notes du jour et prépare ses compléments dans un fichier brouillon. Je valide à 17 h 45.

```
1. Vérifie avec « Mon emploi du temps » de CLAUDE.md si aujourd'hui est un jour de cours. Si ce n'est pas le cas, dis-le en une phrase et arrête-toi.
2. Cherche dans le teamspace Notion « Aivancity » les pages de cours modifiées aujourd'hui. S'il n'y en a aucune, dis-le en une phrase et arrête-toi.
3. Applique le Mode 1 sur ces pages, uniquement sur ce qui est nouveau.
4. Présente tes compléments, classés en ERREUR, IMPRÉCISION et MANQUE, et enregistre-les dans un fichier brouillon du dossier complements-a-valider/.
5. Ne modifie rien dans Notion, ne crée aucun rappel, ne touche pas à mon texte.
6. Termine par un résumé en trois lignes et la phrase « À valider à 17 h 45 ».
```

## Les choix derrière ces consignes

- **« Sans me poser de question »** : personne n'est devant l'écran quand la tâche se lance.
- **Lecture seule sur Notion** : une erreur dans mes dates ou mes paliers fausserait mes révisions sans que je le voie. Un mauvais rappel, lui, se supprime en deux secondes.
- **Un fichier brouillon** : il est lisible depuis n'importe quelle session, contrairement à une conversation.
- **Premier lancement devant l'écran** : pour voir ce que fait la tâche et accorder les permissions une fois pour toutes.
