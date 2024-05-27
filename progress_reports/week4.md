# Week 4

## 06/05/2024

Pour le premier jour de cette semaine je vais commencer par importer la projet sur un oridinateur du laboratoire pour l'avoir sur un pc linux. Je pourrais essayer de lier le dépot github mais j'ai peur des problèmes de compatibilité entre windows et linux donc je vais me contenter de copier les fichiers à la main. Une fois cela fait je vérifie que le projet tourne bien  sur cet oridinateur et je fais quelques changement de style.

Il est difficile de dire ce qu'il nous faut rajouter mise à part plus de paramètres donc on va se contenter d'en ajouter un maximum.

Armand a quand même du corriger quelques erreurs. En effet à cause des différents refresh que j'ai rajouté dans le code, les caméras ne changeait pas de frame rate car il y avait un reset toutes les 3 secondes remettant leur frame rate à 30. Pendant que le rendu caméras est ouvert on stop le refresh. J'ai également changé le calcul automatique de la frame rate, au lieu de la calculer dans le flir_camera on le calcul dans le cameraWidget. A chaque image prise, on calcul avec un produit en croix les fps de la caméras si toutes les images était récupérées à la vitesse de celle prise. On additionne toutes ces estimation et si on a pris plus de x images on affiche la moyenne c'est à dire la somme des estimations de fps divisé par x et on reset la somme et le compteur. x est égal aux fps fixe de la caméra. Si les fps sont fixés à 4 on affiche la moyenne si le compteur est à 4 ou plus. J'ai aussi ajouté le fait que les fps changent quand on appuie sur pause ou play.

Armand a également importé le projet sur un pc windows du laboratoire.

J'ai changé le fichier .ui de editParameters pour ajouter les changements de paramètres que je voudrait ajouter par la suite.

J'ai commencé par finir l'exposure time en ajoutant un minimum (tout le reste était déjà rempli). J'ai ensuite ajouté le gain en me basant sur le code de l'exposure time. C'est à dire un slider pour changer la valeur, un minimum modifiable pour le slider, un maximum modifiable pour le slider et une checkbox pour activer ou non le slider. J'en ensuite ajouté le paramètre triggerSource et triggerMode pour pouvoir utiliser la "triggerBox" sur notre application. Tout à bien fonctionné donc j'ai ajouté également un maximum pour le frame rate car l'application bug si c'est trop élevé en raison du nombre d'images récupérés qui est trop haut. L'application bug parfois quand on met à 60 fps mais j'en connais pas la raison et je ne sais pas vraiment si on peut y faire quelque chose.

Armand:
- correction bug framerate
- enorme refacto utils
- problèmes d'ordinateur & de git

## 07/05/2024

Pour commencer la journée je me suis renseigner un peu sur comment créer un bon rapport de stage en utilisant markdown. et LaTex pour convertir en pdf. J'ai ensuite fait quelques sur un fichier markdown au hasard et j'ai eu des résultats satisfaisant donc je pense utiliser cette méthode pour le rapport de stage. Cette partie n'avait rien à voir avec le projet mais je pense que c'est important de se renseigner sur comment faire un bon rapport de stage.

J'ai ensuite ajouté dans le projet un calcul automatique du maximum de fps possibles car selon le pc et le moment ça peut changer.
Le calcul automatique du maximum pose problème quand il est trop haut car si trop d'images sont récupérées tout bug. J'ai donc ajouté une limité à 60 fps.

On réfléchit à quoi ajouter dans l'application mais nous pensons avoir accomplit l'objectif et plus encore donc on tente d'ajouter des fonctionnalités pour le plaisir mais on commence à être à cours d'idée.

On a envoyé un mail à notre superviseur la semaine dernière mais pas de réponse pour l'instant. On ne sait pas combien de temps il va rester en vacances.

J'ai finalement trouvé une fonctionnalité à ajouter qui est la prise de photo. J'ai pas eu trop de mal à l'implémenter et ça a focntionné direct j'ai donc fini la journée en ajoutant de la documentation au code.

Armand:
- Fixe la taille des différentes pages
- Ajout d'un slider afin de pouvoir changer le zoom de la caméra
- Gère quand le controlleur & la page de settings se ferme (quand on clique n'importe où d'autre)
- Refacto connections, rendu le code plus propre, ajout de doc

## 08/05/2024

Nous n'avons pas fait grand chose ce matin au niveau du projet mise à part quelques suppressions de fonction inutiles et quelques divisions de fonctions en plsusieurs autres. Pour passer le temps on a testé la création d'une app Qt en python.

Nous avons un meeting prévu à 16h pour parler su projet et de l'application.

Nous avons aussi supprimé un élément lorsqu'on change l'image de taille pour correspondre au label pour voir si ça allait accéléré l'affichage de l'image et ça a eu l'air de marcher donc on a laissé comme ça. Cependant je suis pas sur de savoir si c'est une cohencidence ou non.

Nous attendons le meeting de 16h en même temps que de réfléchir si il y a des choses à ajouter.

Nous avons présenté le projet à nos superviseurs lors du meeting. Ils ont été satisfait du projet car ça nous a fait une bonne base pour la suite. Ils nous ont ensuite montré l'ancienne application avec ses fonctionnalités. Ils nous ont montré ce qu'il fallait faire marcher sur les dernières version de spinnaker et Qt. Ils nous ont passé différentes versions du projet et améliorer. Notre but dans les prochaines semaines est donc de faire marcher l'application sur les dernières versions de spinnaker et Qt.

L'application à améliorer sert globalement à faire du motion capture en 3D grâce à des caméras et des capteurs.

Armand:
- Ajout d'un bouton afin de pouvoir choisir où sauvegarder une image prise depuis l'application
- Changement du nom des images sauvegardées selon la date & le numéro de la frame
- Fix de petits bug
- On avait pas grand chose a faire aujourd'hui donc c'est tout

## 09/05/2024

Nous avons commencé par prendre connaissance du projet, en effet nous pouvont y trouver une bonne documentation et de bonnes informations pour le projet. Nous avons importé le projet sur Qt et nous sommes en train de regarder les fichiers à importer pour faire marcher l'application. Premièrement nous avons changer l'import de librairie dans le fichier .pro pour lui dire d'importer Spinnaker. Ensuite on essaye d'adapter les import de Qt parce que certains fichiers de Qt ont été changé de place ou supprimé avec les nouvelles versions. On a pour le moment utiliser la dernière version de Qt 6.7.0 avec les includes de Spinnaker déjà dans le projet mais on changera les includes si besoin.  

Voici une liste des changements à faire de Qt 5 à Qt 6 que nous avons trouvé:

- la classe QAction qui était dans QtWidgets est maintenant dans QtGui
- QtOpenGl/QGL est maintenant remplacé par QyOpenGL
- qopenglwidget est maintenant dans QtOpenGLWidget/QOpenGLWidget
- La classe QTreeWidgetItem ne possède plus de fonction setBackgrounds mais doit être remplacé par setBackground
- QtNetwork/qnetworkconfiguration.h, QtNetwork/qnetworkconfigmanager.h QtNetwork/qnetworksession.h sont dépréciés et on n'a pas trouvé de substitution donc on supprime.
- la classe QGLWidget est remplacé par la classe QOpenGLWidget
- la méthode swap de Qlist est remplacé par la méthode swapItemsAt
- QString::skipEmptyParts est remplacé par Qt::SkipEmptyParts
- Il n'est pas possible de fait QString != null donc on fait QString.isNull()
- Une des méthodes veut renvoyer un QPixmap* mais renvoie un QPixmap donc on a changé le type de retour pour QPixmap.
- La méthode delta() de QWheelEvent est remplacé par angleDelta().y()  
- La méthode setTabStopWidth de QPlainTextEdit est remplacé par setTabStopDistance
- La méthode width de QFontMetrics est remplacé par horizontalAdvance
- on a eu une erreur "member access into incomplete type 'QScrollBar'" donc on a ajouté un include de QScrollBar
- On a dû également include QDialogButtonBox
- On a dû également include QMessagebox
- Comme dit plus haut on a dû changer QGLWidget par QOpenGLWidget donc il faut remplacer l'appel à la méthode updateGL par update
- On aussi eu un problème avec la méthode std::min car les deux paramètres n'étaient pas du même type donc on a rajouté un cast int pour le deuxième paramètre

Après avoir changé tous ces problèmes on a du faire face à un autre problème, en effet en plus de spinnaker le projet de base utilise flycamera pour récupérer les images des caméras. C'est en quelque sorte l'ancêtre de Spinnaker et il est trop vieux pour l'utiliser avec notre version  d'ubuntu, on supprime donc toute référence à ça dans le projet, ce qui malheuresement supprime la compabilité avec les caméras plus anciennes (caméra FireFly)

On a ensuite tenté d'importer les librairies d'OpenGL au projet car le projet ne reconnaissait pas les classes d'OpenGL. On a donc installé GLU mais même en ajoutant la librairie ça n'a pas marché. On a donc ajouté openglwidget dans le .pro aux widgets et ça a l'air de marcher pour l'instant.

On a aussi ajouté un constructeur dans ImageLabel car il n'y en avait pas et que l'application en réclamée un. Après l'avoir ajouté, l'application s'est lancée.

Mais le rendu des caméras n'a pas eu l'air de marcher pour l'instant. Le code nous renvoie une erreur comme quoi il manque un fichier de settings donc on va essayer de le chercher.

## 10/05/2024

Nous commençons la journée en analysant le code pour trouver pourquoi le rendu des caméras ne marche pas. Nous esseyons de monter de classe en classe pour comprendre ce qui se passe. Après quelques heures de recherches on s'est rendu compte que le trigger mode est incorrect. En effet le trigger mode est mis à "on" alors que le trigger source est mis à "software" et que le trigger selector est mis à FrameStart. On a donc changé le code pour mettre à "on" seulement si la source est autre que software (le trigger selector restera le même). On a ensuite testé et ça a marché. 

Armand a ensuite travaillé sur l'import de settings depuis un fichier car certains paramètres étaient incorrects et ont dû être modifié pour correspondre à ceux de la caméra. Je me suis personnelement renseigné sur le fonctionnement du trigger et comprendre ces paramètres de spinView.  

Pour le problème du trigger mode, on a réalisé qu'en fait lorsqu'il est sur off les images ne seront pas récupéré par un signal mais plustôt de manière continue par rapport aux paramètres de la caméra. Au contraire le trigger mode "on" attend un signal pour récupérer une image. On a donc changé le code pour que le trigger mode soit à off car si nous n'utilisons pas la trigger box on a aucun moyen d'envoyer un signal.

On essaie depuis quelques heures de décortiquer le code pour comprendre l'import de paramètres depuis un fichier car cet import provoque une erreur au niveau du trigger que nous n'avons pas encore trouvé. Le problème vient du fait qu'il nous est impossible de récupérer les images en "auto trigger" car le cela implique d'utiliser le trigger software qui n'existe pas car notre trigger mode est à off et les images ne sont pas récupérées par un signal. Le code présent à la récupération d'image à propos du trigger software est donc mis en commentaire.

Nous avons ensuite tenté d'utiliser le bouton pour prendre une image par image mais ça n'a pas marché et l'application a planté dès le début. Grâce au message d'erreur on a pu voir que le lancement et l'arrêt de l'acquisition d'image était différent, peut être plus ancien que ce qui est fait maintenant. On a donc changé le code pour que l'acquisition d'image soit plus correct mais ça a tout de même planté. 
Après inspection du code j'ai remarqué que les données de l'image prise sont sauvegardées dans un buffer puis l'image est supprimée. J'ai déjà remarqué que la suppression d'une image dont les données sont utilisées ailleur provoque un crash. Pour le moment même si je suis pas sûr de si c'est la meilleure option je vais supprimer le supression de l'image. Ca a fonctionné mais on va peut être essayer de trouver une autre solution.