# Tâche planifiée : preparation-complements

Les soirs de cours, elle lit mes notes du jour et prépare ses compléments avant ma séance. Je n'ai plus qu'à lire et valider.

## Réglages

| Champ | Valeur |
|---|---|
| Type | Locale |
| Nom | `preparation-complements` |
| Dossier | `professeur-ia` |
| Mode de permission | Manual |
| Fréquence | Tous les vendredis et samedis à 17 h 30 |

Le menu des horaires ne propose pas « deux jours par semaine ». J'ai donc créé cette tâche en la décrivant à Claude Code dans une session, avec le message ci-dessous.

## Le message que j'ai envoyé pour la créer

```
Crée une tâche planifiée locale nommée « preparation-complements », qui se lance tous les vendredis et tous les samedis à 17 h 30, dans le dossier professeur-ia, en mode de permission Manual, avec ces instructions :

1. Vérifie avec « Mon emploi du temps » de CLAUDE.md si aujourd'hui est un jour de cours. Si ce n'est pas le cas, dis-le en une phrase et arrête-toi.
2. Cherche dans le teamspace Notion « Aivancity » les pages de cours modifiées aujourd'hui. S'il n'y en a aucune, dis-le en une phrase et arrête-toi.
3. Applique le Mode 1 sur ces pages, uniquement sur ce qui est nouveau.
4. Présente tes compléments dans la conversation, classés en ERREUR, IMPRÉCISION et MANQUE.
5. N'écris rien dans Notion. Termine par : « Réponds "ok" pour que j'écrive ces compléments dans Notion, ou dis-moi ce que tu veux changer. »
```

## Ce que Claude Code a ajouté

Avant de créer la tâche, Claude Code m'a posé deux questions. Mes réponses ont amélioré la consigne de départ.

| Question | Ma réponse | Pourquoi |
|---|---|---|
| Créer la tâche, préparer maintenant, ou les deux ? | Créer la tâche | Mes notes du jour n'étaient pas terminées |
| Où ranger les compléments en attendant ma validation ? | Fichier local | Un fichier est lisible depuis n'importe quelle session, une conversation non |

Les instructions finales contiennent donc en plus :

- l'enregistrement du résultat dans un fichier brouillon, `complements-a-valider/AAAA-MM-JJ-nom-du-cours.md`, dans le dossier du projet ;
- des interdictions plus strictes : ne rien modifier dans Notion (ni la page du cours, ni « Suivi de révision »), ne créer aucun rappel, ne pas toucher à mon texte ;
- un résumé final en trois lignes : cours traité, nombre d'erreurs, d'imprécisions et de manques, et la phrase « À valider à 17 h 45 ».

Le texte exact de la tâche est enregistré sur le Mac, dans `~/.claude/scheduled-tasks/preparation-complements/SKILL.md`.

J'ai écarté l'option « Brouillon dans Notion » : elle écrivait dans ma page de cours avant mon accord, ce qui contredit la règle 4 du Mode 1.

## Pourquoi ces choix

- **17 h 30** : mes cours finissent à 17 h, mes notes sont alors complètes. Mon rappel sonne à 17 h 45, quand tout est prêt.
- **Vérification du jour de cours** : la tâche se lance chaque vendredi et samedi, mais je n'ai cours qu'une semaine sur deux. Elle s'arrête d'elle-même les autres semaines.
- **Uniquement ce qui est nouveau** : sans cette règle, elle recomplèterait toute la page à chaque passage.
- **Un filet de sécurité dans `CLAUDE.md`** : si la tâche n'a pas tourné (Mac en veille), l'agent me propose `/complete` à la place.

## Exemple de résultat (premier lancement, 10 octobre 2026)

```
Cours traité : Introduction à l'Intelligence Artificielle et aux IA Génératives (partie du 10.10.26).
0 erreur / 1 imprécision / 5 manques (dont 3 optionnels), en plus des compléments déjà écrits dans la page.
À valider à 17 h 45.
```

Ce que ce premier lancement a montré :

- elle a reconnu le jour de cours ;
- elle a vu que 7 notions étaient déjà dans « Suivi de révision » et ne les a pas répétées ;
- elle a appliqué la règle sur l'état de l'art : un chiffre de mes notes ne correspondait pas à la source, elle me renvoie vers mon professeur ;
- elle a séparé les notions nécessaires des notions « bonus », à garder seulement si le professeur en a parlé ;
- elle a signalé qu'elle avait tourné en avance et que mes notes ajoutées ensuite ne seraient pas prises en compte.
