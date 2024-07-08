# Week 13

## 08/07/2024

Aujourd'hui, j'ai continué à travailler sur l'application. Comme je l'avais prévu la semaine dernière, j'ai cherché des crashs dans l'application.Lors de mes précédentes recherches, je ne trouvais pas de crash mis à part ceux lié à la déconnection de caméra. j'ai donc décidé de me concentrer principalement sur ça. Après quelques recherches j'ai trouvé que lorsqu'on lance le live view d'une caméra déconnectée, l'application crash. Cela doit certainement venir de certains paramètres qui sont manipulé alors que la caméra est déconnectée mais je dois y préter plus attention. Un try catch est déjà présent donc le problème vient d'ailleur. Afin de régler le bug j'ai essayer de la provoquer pour observer les messages d'erreurs mais je n'ai pas réussi à le ravoir.

Je n'ai pas trouvé de bugs suplémentaire de ce côté là mais j'ai remarqué que si aucune image n'est affichée le crosshair agit bizarrement donc je vais corriger ça. J'ai fait plusieurs tests mais je n'ai pas eu le temps de corriger le bug donc je vais continuer ça demain.