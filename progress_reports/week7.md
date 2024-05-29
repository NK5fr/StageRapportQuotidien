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
