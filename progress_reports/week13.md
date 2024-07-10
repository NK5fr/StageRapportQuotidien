# Week 13

## 08/07/2024

Aujourd'hui, j'ai continué à travailler sur l'application. Comme je l'avais prévu la semaine dernière, j'ai cherché des crashs dans l'application.Lors de mes précédentes recherches, je ne trouvais pas de crash mis à part ceux lié à la déconnection de caméra. j'ai donc décidé de me concentrer principalement sur ça. Après quelques recherches j'ai trouvé que lorsqu'on lance le live view d'une caméra déconnectée, l'application crash. Cela doit certainement venir de certains paramètres qui sont manipulé alors que la caméra est déconnectée mais je dois y préter plus attention. Un try catch est déjà présent donc le problème vient d'ailleur. Afin de régler le bug j'ai essayer de la provoquer pour observer les messages d'erreurs mais je n'ai pas réussi à le ravoir.

Je n'ai pas trouvé de bugs suplémentaire de ce côté là mais j'ai remarqué que si aucune image n'est affichée le crosshair agit bizarrement donc je vais corriger ça. J'ai fait plusieurs tests mais je n'ai pas eu le temps de corriger le bug donc je vais continuer ça demain.

## 09/07/2024

Aujourd'hui, j'ai continué à travailler sur l'application. Hier, je n'ai pas pu finir les modifications que je voulais apporter à l'application. Ainsi, j'ai continué à trvailler sur ça aujourd'hui. Le crosshair n'était pas bien placé car il se positionnait par rapport à la taille de l'image affichée et même quand il se positionnait par rapport à la fenêtre, il ne prenait pas en compte la marge. J'ai changé le code pour prendre en compte cette différence. Maintenant il est bien placé dans la fenêtre.

J'ai ensuite continué à chercher des bugs qu'on aurait oublié dans l'application mais après plusieurs essais je n'ai rien trouvé. Par conséquent, je me suis penché sur le bug que j'ai trouvé hier mais que je n'ai pas pu reproduire. J'ai de nouveau essayé de le provoquer et j'ai réussi au bout d'un moment. Cependant, je ne sais pas comment j'ai réussi et je ne suis pas parvenu à le reproduire par la suite. 

## 10/07/2024 

Aujourd'hui, j'ai continué à travailler sur l'application. Comme hier j'ai continué à chercher des bugs qu'on aurait oublié ou pas vu mais je n'ai rien trouvé d'interessant. J'ai ensuite remarqué que j'avais reçu un mail pour remplir un questionnaire obligatoire à propos de notre mobilité et d'erasmus +. J'ai donc rempli le questionnaire en entier, j'ai téléchargé le pdf final et je l'ai mis dans la partie "fin de mobilité" de moveon pour obtenir la fin des bourses.

Ensuite, j'ai encore une fois cherché des bugs pour j'ai toujours rien trouvé. Ainsi, comme c'est notre dernier jour de stage, j'ai décidé de faire une relecture complète du "system_documentation" de l'application car on ve envoyer la dernière version de celui-ci ce soir avant de partir. J'ai donc corrigé certaines tournures de phrases qui me semblaient imprésises.