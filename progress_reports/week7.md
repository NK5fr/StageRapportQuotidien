# Week 7

## 27/05/2024

Nous ne savons pas trop quoi ajouter à notre projet on a déjà fait beaucoup de choses et nous n'avons pas d'idée sur ce qui est à faire de plus. Pour l'instant on va travailler sur notre soutenance que nous allons faire la semaine du 10 juin.

J'ai fait une bonne partie de mon diaporama ce weekend donc j'ai fait une présentation à Armand pour commencer la journée. On a discuté un peu ce qui est bien et de ce qu'il faudrait ajuster. Je me suis mis à faire un script à suivre pour la présentation. Je n'aime pas forcement préparer un texte avant une soutenance car j'aime y aller au feeling mais je pense que c'est nécessaire ici car mon niveau en anglais est trop faible pour me permettre de tout improviser.  

J'ai fait les explications pour plus de la moitié des slides allant de l'introduction avec la sommaire, présentation de la NTNU, de Vizlab jusqu'à la présentation de CameraManager, les améliorations de Qt la présentation de Spinnaker et j'ai commencé les explications du trigger.

## 28/05/2024

Toujours en attendant un meeting avec Tomas Holt j'ai continué et fini la première version du script pour l'oral j'ai fini tout l'aspect technique et ce qu'il reste à faire. Je ne vais pas faire la conclusion pour l'instant car je ne sais pas trop quoi dire.

Durant une partie de la journée il y a eu des présentations sur d'autres projets dans le laboratoire. Nous avons écouté les présentations des autres. 

Après les présentations qui ont durée plusieurs heures nous avons eu un meeting qui a lui duré un peu plus d'une heure. On a présenté les avancées, nos remarques et nos questions. Nous avons surtout discuté des problèmes qui restent à résoudre. Les principaux problèmes sont les problèmes d'affichage des images sur windows et la sous application qui marche à moitié et qui pourrait être grandement améliorée.

Nous avons commencé à regarder comment résoudre les problèmes mais nous n'avons pas eu le temps et on a fini pour s'entrainer pour l'oral.

## 29/05/2024

Pour commencer la journée j'ai continué de regarder ce problème d'affichage d'une image dans une window. D'après mes observations il semblerait qu'en fait une window de même dimension sur linux et window n'est en réalité pas de la même dimension en pixel. Je n'ai aucune idée d'où peut venir le problème et il semblerait que ça touche beaucoup de windows.

Pour tenter de trouver la source de ce problème j'ai lancé l'application sur divers pc et divers os et la taille n'est jamais pareil. Cependant si la taille bouge tout le temps pour la taille de l'image elle ne change pas ?

J'ai tenté de lancer l'application sur un pc du labo sur windows mais l'application crash au lancement de la caméra. J'ai résolu ça en installant la dernière version de Spinnaker sur le pc. Cependant j'ai un autre bug qui est apparu et qui se déclenche lorsqu'on fermer l'application. Après de longue recherchent je me suis rendu compte que lorsqu'on désactive ferme la window pendant la capture et l'envoie d'une image l'application crash. Pourtant on fait des vérification à l'envoie des images mais ça ne semble pas suffisant donc je vais rajouter encore une vérification.

Je suis reparti sur les problèmes de window, premièrement je vérifie la résolution de chaque pc :

- Mon pc : QSize(1536, 864)
- Pc Windows du labo : QSize(2560, 1440)
- Pc Linux du labo : QSize(3840, 2160)
- Pc d'Armand : QSize(1536, 864)

La taille des windows changent selon la résolution cependant la taille de l'image reste la même. Cela provoque un décalage dans l'affichage qui n'est plus correct.

Ce qu'il semble se passer c'est que lorsque qu'on appelle glViewport pour dimensionner la zone d'openGL, la résolution de l'écran n'est pas prise en compte et le viewport prend des dimensions qui ne sont pas en accord avec l'écran.

Il est bien sûr possible d'ajuster la taille du viewport manuellement mais ça ne sert à rien puisque tous les autres écrans seront éronné. Il faudrait donc trouver une solution pour que le viewport prenne en compte la résolution de l'écran.
J'ai fait quelques recherches sur certaine fonction d'openGL à propos du redimensionnement mais je n'ai rien trouvé de plus de ce que je sais déjà.

J'ai fini la journée par un long entrainement pour l'oral.

Armand:

Travaille sur pouvoir selectioner une zone avec la souris, pour selectionner les markers de cette manière.
Pour le moment je peux faire un point a l'origine du clique et un point a l'endroit courant. Le problème est que visuellement elles sont alignées mais le problème rencontré est que ils ne sont pas vraiment alignés, leur coordonnée Z n'est pas la même et donc en essayant de faire un carré allant d'un point a l'autre, ça ne marche pas.
Il faudrait transposé un point sur le Z de l'autre, chose qui est faisable normalement.
Ensuite il faudra réussir a transporté ce carré d'un extrème a l'autre de la matrice, ce qui va permettre de choisir les markeurs selon la selection.
Une fois cela fait, il faudrat simplement lister tous les markers et permettre de les selectionner on click sur les labels, histoire de rendre tous mieux

## 30/05/2024

Aujourd'hui j'ai continué à chercher comment ajuster la taille des du viewport en fonction de la résolution. J'ai fait quelques tests mais rien de concluant. J'ai finalement trouvé quelqu'un parlant d'un device pixel ratio. J'ai multiplié la taille de la window par le device pixel ratio et ça a fonctionné. Tous les endroits où l'image n'était pas affichée correctement sont maintenant corrigés. Cependant ça n'a pas fonctionné sur la sub application qui a également des problèmes sur l'affichage des images.

Le device pixel ratio est une valeur qui permet de savoir combien de pixel de l'écran correspond à un pixel de la window. En mutipliant la taille du viewport par cette valuer nous obtenons la taille de la window effective sur l'écran.

J'ai ensuite modifié mon diapo pour l'oral pour retirer le bug d'affichage de la partie erreurs restantes. J'ai ajouté la correction de l'erreur à la partie faire marcher l'application sur windows.

J'ai ensuité travaillé sur la sub application. J'ai repéré l'endroit où les calculs de pixels sont fait et j'ai multiplié les valeurs par le device pixel ratio. Cela a corrigé le problème et nous n'avons plus eu de décalage dans l'affichage.

J'ai continué la journée en travaillant sur mon rapport de stage. J'ai préparé un plan il y a quelques semaine mais je trouve qu'il ne met pas assez en valeur la mission. J'ai alors changé le plan et j'ai noté des idées sur ce quoi dire pour chaque parties.

Je fini par un entrainement pour mon oral comme d'habitude.

Armand:
Encore plus de travail sur la selection, j'ai reussi au bout d'un moment a faire une selection qui fait un carré entre le point d'arrivé et d'entré, mais il n'est absoluement pas centré sur la vue de la caméra et regarde toujours le même sens.
J'ai fait des recherches sur comment changé ça mais tous le code que j'ai trouvé ou essayer de faire n'a absolument pas marché.
Je reprendrais demain ou alors j'essayerais de refaire la rotation de l'écran demain. ce dernier pourrait s'averrer plus simple maintenant que nous avons le pouvoir de GLU.

## 31/05/2024

Pour commencer la journée j'ai fait le tour de l'application pour repérer des bugs. J'ai répéré une erreur venant du fait que certaines options spécifique aux images noires et blanches restent actives lorsqu'on change le colorMode. J'ai docn fait en sorte de désactiver toutes ces options au changement du color mode.

J'ai ensuite ajouté une petite option à l'activation du crosshair faisant en sorte que le curseur soit affiché avec un croix qui le suit. J'ai fait le dessin des lignes avec OpenGL et j'ai fait en sorte que cela s'active et se désactive avec le crosshair.

J'ai ensuite travaillé un peu sur l'introduction et la conclusion de mon rapport de stage. J'ai juste écrit quelques notes de ce que je vais dire. J'ai également travaillé sur la conclusion de mon oral car je n'avais encore fait sur mon diapo dans la partie conclusion.

J'ai ensuite fait des recherches sur la rotation de la sous application. J'ai cherché un moyen de la rendre meilleure avec ce qu'on a déjà. Je n'ai pas trouvé de solution. J'ai égelement tenté de copier la rotation de CameraManager mais en vain.

J'ai ensuite fait des recherches sur la sélection des marqueurs. J'ai essayé de comprendre comment le code s'y prend pour les sélectionner et pourquoi ça marche pas. Je n'arrive pas vraiment à savoir si c'est la souris qui n'est pas repéré au bon endroit ou si c'est autre chose. Etant donné que nous n'avons pas accès au pc Linux il est compliqué de comparer quoi que ce soit.

J'ai ensuite investigué le MenuBar pour voir ce qui fonctionne. J'ai vu qu'on pouvait lancer un exécutable depuis celui ci. J'ai vu aussi que beaucoup de fonctions ne sont pas implémentées alors qu'elles devraient l'être. Un écouteur d'événement est bien implémenté mais il n'est jamais appelé pourtant tout est bien implémenté.

Je fini la journée par un entrainement pour l'oral.

Armand:

Aujourd'hui j'ai décidé de me coller a la caméra, car je me suis dit qu'après avoir fait des recherches sur OpenGL hier, je pourrais essayer de résoudre ce problème en recommencant de 0.
J'ai passé la journée a faire différents tests et j'ai atteins a un moment un résulat plutôt similaire ce que nous attendions, mais c'étais encore trop chaotique, et j'ai donc revert mes changements.
Mon problème étant qu'il faut qu'on puisse régler le "pitch" et le "yaw" de la caméra, donc l'orientation X et Z, mais pas le "roll", soit le Y.
Comme ça, nous ne pouvons pas "bouger le sol". On peut regarder de haut en bas et on peut tourner de gauche a droite, mais on ne penche pas la caméra.

Durant l'après midi, je suis passé sur une moyen plus simple de selectionner les markeurs, via une liste dans l'onglet "tab".
J'ai presque fini cela, il faut encore que je puisse choisir si on veux séléctionner ou lié des markeurs entre eux.
