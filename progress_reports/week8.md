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

## 04/06/2024

Pour commencer la journée j'ai commencé par travailler sur mon rapport, j'ai déjà rempli quelques parties importantes et je vais maintenant faire le résumé. Remplir cette partie m'a pris une bonne partie de la matinée et j'ai également corrigé les erreurs et fait une relecture de l'introduction. Il ne me reste maintenant plus que le développment à faire sans compter l'annexe et le glossaire que je ferai à la fin.

J'ai ensuite regardé pour corriger quelques erreurs de CameraManager. J'ai d'abord corrigé un bug lancé lorsqu'on lance le widgetGL vide. En effet, lorsque le widgetGL est lancé sans projet il tente de créer une instance de swapping corrector avec un fichier de data vide. Swapping corrector renvoie alors une erreur car il ne peut pas importer les data d'un fichier inexistant. J'ai donc changé le code pour ne plus créer swapping corrector avec un fichier et en plus on ne peux pas l'ouvrir.  

J'ai eu aussi un problème à la fermeture de l'application car certain widget ne se ferment pas à temps, cependant il est difficile de trouver de quel widget il s'agit donc je n'ai pas fini.

J'ai fini par faire une lecture de mon rapport pour reformuler certaines phrases.  

Armand:

Fin de l'implémentation de la liste des markeurs dans SwappingCorrector, ajout de cela dans la vraie application.
Fix des problèmes de selection (liste pas synchro et donc il y avait des bugs).
Travail sur mes slides & sur mon rapport

## 05/06/2024

Pour aujourd'hui j'ai décidé de concentrer toute la matinée sur la soutenance. Il y a beaucoup de passages ou j'hésite et ou je suis incertain et il faut que je corrige ça pour la soutenance. Il faut que je m'entraine et que je corrige les parties trop longues. 

Finalement comme on a déjà fait beaucoup de travail sur CameraManager j'ai décidé de travailler mon rapport le reste de la journée.

J'ai aussi un peu travaillé sur le problème d'hier. L'erreur venait du fait que la sous application n'értait pas bien supprimée. J'ai aussi résolé un problème de connect qui n'existe plus dans Qt6 je l'ai donc remplacé.

J'ai fini la journée par un entrainement oral.

## 06/06/2024

Suite à des indications qu'on a reçu par mail de la part de Tomas Holt, nous allons devoir faire une documentation sur ce qu'on a fait dans le project. On va devoir fait une description du projet mais on va surtout devoir faire une description de ce qu'on a fait. Globalement ce rendu doit permettre à quelqu'un de faire le même travail que nous.

J'ai donc fait un résumé de tout ce qu'on a fait au projet grâce aux rapport quotidients. On devrait pouvoir faire le rapport à partir de ça.
J'ai également fait un code couleur pour séparer ce qu'on a fait en différentes parties.

Pendant le reste de la journée on a écrit le début de ce rapport en épliquant le fonctionnement de l'application et on a fait aussi le début des parties "how to upgrade Qt" et "required libraries and software".  

J'ai fini la journée par un entrainement oral.

## 07/06/2024

Aujourd'hui j'ai continué à travailler sur le rapport avec Armand. Nous avons fini les parties "how to upgrade Qt", "required libraries and software", "how to upgrade Spinnaker", "How to run CameraManager on Windows" et "Other changes". Cela nous a pris une bonne partie de la journée.

J'ai également corrigé un bug lié au faut que lorsqu'une camera est deconnectée elle n'est pas bien déselectionnée. J'ai donc ajouté un appel à la fonction item click pour simuler un click sur un autre item de la liste. Cela a l'air de marcher.

j'ai fini la journée par un entrainement oral.