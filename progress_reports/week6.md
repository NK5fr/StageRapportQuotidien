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

Armand:
J'ai aidé Nathan avec les images durant la matinée, l'idée de base étant d'avor un paramètres Couleur sur les caméras qui, selon si il est toggled ou pas, change la conversion de l'image récupérée par la caméra et change la texture OpenGL appliquée a l'image afin de pouvoir affiché la caméra.
Ceci, cependant, n'a pas marché car la texture OpenGL est créée une fois au lancement et qu'elle est utilisée et modifiée très souvent, l'idée tombe a l'eau car le process qui créé la texture la traite aussi d'innombrables fois en partant du principe que l'image est monochrome, changer la texture cause forcément un crash lors de ces traitements.
Dans l'après midi, j'ai travaillé afin de rendre le code plus lisible, pour cela j'ai changé les Project Files du projet.
J'ai changé les fichiers dans les champs "HEADERS" & "SOURCES" afin qu'ils trouvent des fichiers existant, avec un chemin qui se base sur le PWD au lieu du fichier courant (c'est a peu près la même chose mais comme ça on est sur que c'est cross platform).
J'ai aussi changer les divers dossiers dans les INCLUDESPATH, en enlevant tous les dossiers qui n'existent pas, car les ajouter est strictement inutile. J'ai aussi uniformiser la mise en page des includes de Qt en utilisant la variable QTDIR qui mène directement au dossier dans lequel Qt est.

J'ai aussi ajouter un warning qui s'affiche au lancement de l'application afin de prévenir l'utilisateur si des fichiers de configuration sont manquants des fichiers du projet (notamment les paramètres de default tracking point & des spincamera).

## 22/05/2024

J'ai réglé le problème de fermeture des fenêtres. J'ai ajouté une vérification que la windows que l'on cherche à enlever existe bien.

J'arrive donc a afficher deux fenêtre liées ensembles et à la caméra et maintenant je vais essayer d'afficher la même image dans ces deux fenêtres. Le problème est que sur la deuxième fenêtre créée pour la couleur les images ne veulent pas s'afficher. C'est pourtant une copie conforme de la permière et je n'ai même pas encore ajouté le nécessaire pour la couleur donc je ne comprends pas pourquoi ça ne marche pas.


J'ai remarqué qu'il y avait une valeur qui n'était pas initialisée dans la deuxième window j'ai alors réussi à afficher deux windows ayant la même image en noir et blanc. J'ai ensuite ajouté une condition me permettant de choisir de récupérer une image en noir et blanc ou en couleur. Maintenant je récupère donc deux images que j'envoie aux deux windows.

J'ai maintenant un bouton permetant d'activer et de désactiver le color mode. Tout ce qu'il faut pour l'instant c'est de changer une variable chargée de savoir si le mode est activé ou non. Le bouton désactive aussi tout les autres option incompatibles avec le mode couleur.

Le bouton permet maintenant d'ouvrir et de fermer la bonne window selon si l'utilisateur veut voir en couleur ou non.

J'ai aussi cherché la raison d'un crash, en effet lorsqu'on retire une caméra pendant qu'elle diffuse l'application crash. J'ai tenté de fermer la capture d'images mais en raison de problèmes de thread je ne peut pas l'arrêter comme ça. Je n'ai pas eu le temps de trouver l'origine du crash.

Armand:
Dans la matinée, je me suis renseigner sur comment faire de la documentation avec Qt, et j'ai commencé a faire de la documentation qu'on pourra généré afin de rendre la compréhension du code plus simple.
J'ai commencé par MainWindow, et j'ai fait a peu près la moitié.
Après notre entrevue avec Tomas Holt, j'ai récupéré le projet des élèves Français de 2016 (Projet3DOpenGL) et je l'ai fait passé sur Qt6, puis j'ai corrigé les bugs de l'application (elle ne marchait pas car le projet ne propose pas de choisir quel fichier on veux charger).
Le projet se lance et réussi a charger un fichier, mais la démo ne marche pas, et donc je vais essayer de faire marcher cela demain.

## 23/05/2024

J'ai commencé la journée en essayant de comprendre pourquoi l'application crashait lorsqu'on retirait une caméra. J'ai remarqué que le problème venait du fait que je n'avais pas ajouté de vérification que la caméra était branchée avant de chenger les paramètres du trigger. Une fois cela fait je n'ai plus eu de crash. Cependant il m'est arrivé d'avoir des crash en reconnectant une caméra après l'avoir déconnectée. Cependant ce bug n'est pas apparu tout le temps donc c'est compliqué de le corriger. Pour tenter d'éviter tout problème j'ai fait en sorte qui si la caméra débranchée est celle selectionnée alors on la déselectionne.

J'ai ensuite essayé de brancher une camera uniquement en noir et blanc pour voir ce que ça fait. J'ai remarqué que l'image est très sombre et j'ai rapidement compris que c'est l'exposure time qui est mit au minimum si sa valeur ne rentre pas dans la limite. J'ai changé ça pour mettre au minimum ou au maximum selon si la valeur est en dessous ou au dessus de la limite.

J'ai passé une bonne partie de la journée à régler de nombreux problèmes d'affichages liés à la couleur. Après avoir tenté beaucoup de chose j'ai trouvé qu'au final la meilleure solution c'est de décocher les caméras pour complètement fermer les windows et laisser l'utilisateur recocher ce qu'il veut.

J'ai ensuite regardé une application qu'on va devoir fusionner avec la notre. Cette application fonctionne aussi avec du stop motion et permet de voir le résultat total de toute les frames sous forme de vidéo.

L'application est censée afficher une zone en 3D permettant de visualiser les points capturés par la caméra lors du stop motion. Cependant les créateur de l'application on précisé que la rotation de la zone 3D pour visualiser les points dans tous les angles ne marchait pas. J'ai donc essayé de comprendre la rotation avec openGL et j'ai réussi à manipuler un peu les variables pour changer la rotation et j'ai rapidement compris que c'est l'axe y qui a un problème. Je pense qu'ils se sont trompé dans un nom de fonction et qu'ils ont appelé x() au lieu de y(). 
Après correction la rotation marchait beaucoup mieux malgrès qu'elle soit encore imparfaite.

Armand:
Dans la journée, j'ai continuer a travailler sur l'intégration du widget. 
J'ai fixé le problème sur la démo qui ne marche pas, j'ai ajouter les fichiers a notre projet principal, ajouter un bouton dans un widget pre-existant permettant donc d'ouvrir ce nouveau projet avec le fichier actuellement entrain d'être utilisé.
Le projet en lui même est plutôt dur d'utilisation, en effet les élèves ayant fait ce projet en 2016 n'ont pas fait quelque chose de semblable sur OpenGL a ce qui est déjà en OpenGL dans CameraManager.
Nathan a donc changé la manière de bouger la caméra,
je m'occupe d'ajouter un sol a la vision 3d, ce qui permettra de pouvoir plus facilement se repérer dans leur environnement 3d, mais aussi me permettra de faire des choses avec OpenGL.

Après correction la rotation marchait beaucoup mieux malgrès qu'elle soit encore imparfaite. 

## 24/05/2024

J'ai commencé la journée en me renseignant sur la rotation de la zone 3D. Le problème est que pour basculer la zone en haut et en bas le code calcul la rotation seulement en fonction de l'axe y. Cela fait que on peut seulement aller en haut et en bas mais pas part rapport à l'axe x. J'ai fait certaines recherches mais ça prends du temps car il faut comprendre et visualiser les rotations.

La rotation actuelle est acceptable et notre objectif de base est d'ajouter cette application à la notre et non pas de la changer. Donc pour l'instant je vais laisser comme c'est là et sauf inditcation de Tomas Holt je n'y toucherais plus.

Je vais plutôt regarder le problème d'affichage des images dans un widget sur windows en effet sur windows les images ne sont pas affichées correctement et ça provoque des erreurs lorsqu'on clique sur l'image. Il est difficile de savoir d'où vient l'erreur lorsqu'on la retrouve sur windows mais pas sur linux. Cela peut vouloir dire que les comportement des 2 os est différent et il va falloir faire des tests pour trouver à quel niveau.

Il y a beaucoup de windows imbriquées les unes dans les autres donc je dois trouver quelle window pose problème. Je n'arrive pas à trouver la window contenant exactement l'image, la possibilité la plus probable est que tout le placement est géré automatiquement pas opengl et dans ce cas là il va être compliqué de changer quoi que ce soit. En plus je ne veux pas que ça change l'affichage sur linux donc il faut faire attention à ce que je modifie.

J'ai remarqué qu'en changeant les valeurs dans la fonction glViewport je pouvais déplacer cette zone d'affichage mais bien entendu ça déplace aussi sur linux donc ça n'est pas une solution valable.