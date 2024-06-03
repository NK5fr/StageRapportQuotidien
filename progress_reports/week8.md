# Week 8

## 03/06/2024

Pour commencer la journée j'ai travaillé sur le sélection des marqueurs sur swapping corrector. J'ai pu voir qu'il n'y a aucun problème sur linux et que le problème vient encore une fois de windows. J'ai pour objectif de dessiner un pixel rouge à l'endroit où la souris est trouvée pour vois si le problème vient d'un mauvais repérage de la souris. Comme je le pensais c'est encore une fois un problème lié à la réolution de l'écran et de la différence entre les pixels physiques et virtuels. J'ai donc multiplié certaines valeurs par le device pixel ratio et j'ai obtenu un résultat correct. Le souris est repéré au bon endroit et les marqueurs sont sélectionnés correctement.

J'ai ensuite regardé les problèmes du MenuBar. Comme j'ai pu le voir la semaine dernière certain menu du MenuBar peuvent être écouté mais d'autres ne peuvent pas. J'ai aussi remarqué qu'un écouteur d'événement est affecté à chaque menu mais appellent la même fonction. J'ai chnagé ça en mettant un seul écouteur pour le MenuBar entier. Ainsi les actions implémentées dans les menus sont maintenant appelées.

J'ai ensuite changé certaines action dans le MenuBar qui en marchait pas bien et j'ai aussi modifié l'option de real crosshair pour qu'il affiche ou non les coordonnées du curseur. J'ai également ajouté une action close project au MenuBar car il n'y en avait pas.

J'ai ensuite fait un tour de mon diapo pour changer les éléments qui ne sont plus d'actualité et j'ai changé quelques passages trop longs.

Pour finir la journée j'ai fais la partie expérience Erasmus de mon rapport de stage et je me suis entrainé à la présentation orale.

Armand:
J'ai fini de faire le menu permettant de voir tous les markeurs, depuis ici on peux voir la position exacte de chaque markeur.
Pour cela on a un menu additionel dans la partie inférieure de l'application.
Il faudrait maintenant aussi permettre de lier les markeurs depuis ce menu, et aussi les swap entre eux. Il faut encore voir comment faire.
