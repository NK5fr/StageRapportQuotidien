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

## 21/05/2024

J'ai fait quelques tests supplémentaires sur les changements du trigger. Lorsque j'enlève le trigger, que je change la source et que je remets à on la caméra arrive à envoyer des images. Cependant si je change la source sans enlever le trigger, la caméra ne renvoie plus d'images. On dirait qu'il arrive à changer la source et à utiliser le trigger sans élément externe si on le paramètre bien.

Il est normal que le trigger software focntionne car on lancer une demande d'image avant chaque récupérations dans le code. Cependant le trigger software ne devrait pas marcher. Ce qu'il se passe c'est que le changements des paramètres se fait en boucle dans un thread même pendant la diffusion. Ce qu'il se passe c'est que l'image est récupéré pile dans le moment ou le trigger software est mis à off donc l'image est récupéré sans trigger. Je ne change plus le trigger mode lors de la configuration du trigger source.

On a ensuite essayé d'ajouter les couleurs. On a répéré l'endroit dans le code où openGL crée la texture pour l'image et l'endroit où il faut lui dire quel pixel format prendre. Maintenant il nous suffit d'ajouter une valeur 0 ou 1 qu'on va pouvoir changer permettant ainsi l'affichage ou non des couleur. On va transmettre cette valeur au différentes classes affichant les images pour pouvoir changer la couleur de l'image.

On n'arrive pas bien à appliquer ça car on n'arrive pas à changer la texture de l'image en cours de route. On a essayé plusieurs méthodes mais rien de concluant. En plus l'application n'est pas adapté pour utiliser des couleurs. On va donc essayer autre chose.

Le but est de créer une deuxième window identique à la première mais qui elle affichera le nécessaire en couleur. J'ai donc créé cette deuxième windows en copiant le code de la première. J'arrive à l'afficher pour l'instant je n'arrive pas à afficher des images dedant. Le code est un peu brouillon donc c'est difficle d'ajouter un gros morceau comme ça donc je prèfere laisse de côté pour l'instant. J'ai eu également l'idée de créer un bouton activant un "mode couleur" mais ça impliquerait beaucoup de choses à désactiver et ce n'est pas non plus ce que je veux.

Je vais essayer de faire deux windows en même temps de nouveau.