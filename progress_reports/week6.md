## 20/05/2024

Pour commencer la journée, nous avons fait un tour sur le rapport de l'amélioration de CameraManager. Ce rapport est encore imcomplet mais nous avons fait une relecture complète de ce qu'on a déjà écrit. Nous avons changé les fautes et reformulé certaines phrases.

Après ça je vais regarder comment ajouter des paramètres modifiables à la caméra. En effet il y a beaucoup de fonctions autour des caméras et j'aimerais les connaître pour pouvoir ajouter plus de fonctionnalité. J'ai réussi à ajouter une checkbox pour activer et désactiver le trigger. Il semblerait cependant que ça ne marche pas comme prévu et que une fois que j'active le trigger le thread permettant d'activer la récupération d'images plante.  

Le problème est que le code demande la récupération d'une image et l'attend indéfiniment. Ce qu'il faut faire c'est attendre l'image pendant un certain temps (faisable grâce à Spinnaker) et si l'image n'est pas récupéré on renvoie un nullptr. Bien sûr il faut vérifier que l'image n'est pas nullptr avant de l'afficher.  

J'ai ensuite corrigé quelques bugs lié aux paramètre et à leur gestion. En effet il faut ajouter Trigger à tout les endroit où les paramètres sont mentionné. J'ai finalement ajouté un changement des paramètres des caméras lorsqu'on coche la checkbox pour directement changer le mode ou la source du trigger sans devoir relancer la capture d'images.

Après des tests je me suis rendu compte que parfois des images sont récupérée alors qu'elles ne devraient pas l'être mais je vais investiguer plus demain. 

On a aussi changé un truc au niveau de la detection de camera. En effet la détection de nouvelles cameras se faisait toutes les 200 ms grâce à un thread mais pour des raisons obscures une erreur s'affichait à propos du fait qu'on ne pouvait pas lancer un timer dans un thread (la détection marchait bien pourtant) alors qu'on a jamais déclaré de timer. On a donc remplacé le thread par un timer pour faire la même chose mais sans erreur.

Armand:
J'ai aujourd'hui travailler quasiment entièrement sur le problème des couleurs, le problème étant que les images sont en noir et blanc, et que Tomas nous a dit qu'il trouverais ça bien de pouvoir voir en couleur (même si cela rendrait les fonctionnalitées du programmes inutiles, car il faut que ce soit monochrome pour le tracking. Je n'ai pas réussi a mettre des couleurs mais j'ai remarqué que lors de l'affichage il y a une texture OpenGL qui est créée, et qui assure que l'image soit monochrome en plus du fait que l'image elle même est convertie en format Mono8 qui lui aussi est monochrome.
J'ai essayer de changer les textures, ce qui permet d'avoir l'image en rouge ou jaune (mais c'est pas ça qu'on veux) et j'ai essayer de changer le format de l'image, ce qui résulte en soit un crash si la texture n'est pas bonne, soit en une image trouble où on voit a la fois en zoomé et en normal mais aussi 3 fois différentes (surement du au fait que quand on prends un format RGB8 ou Bayer8, on a 3 canals (pour les couleurs) alors qu'en Mono nous n'avons qu'un seul canal)
Puis vers la fin de la journée j'ai passé a nouveau sur les thread et j'ai fait un timer (même si nathan l'a fait un tooooooooooooout petit peu avant moi donc ça servait a rien)
