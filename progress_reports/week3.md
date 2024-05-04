# Week 3

## 29/04/2024

Nous avons continué aujourd'hui à s'entrainer sur Qt, j'ai finalement réussi à intégrer l'API Spinnaker à un projet Qt. Pour cela j'ai utilisé un autre compilateur que celui proposé de base par Qt. J'ai utilisé pour cela le fait que des composants de Qt sont fait pour permettre la compilation avec le compilateur de MSVC. J'ai donc installé ce dernier avec les composants de Qt et en configurant pour projet pour utiliser ce compilateur et en ajoutant les lignes ci dessous dans le CMAKELists.txt, j'ai pu compiler le projet sans erreurs et récupérer par exemple le nombre de caméras connectées à l'ordinateur.

```cmake
add_library(spinnaker STATIC IMPORTED)
set_target_properties(spinnaker PROPERTIES IMPORTED_LOCATION "C:/Program Files/Teledyne/Spinnaker/lib64/vs2015/Spinnaker_v140.lib")
set_target_properties(spinnaker PROPERTIES INTERFACE_INCLUDE_DIRECTORIES "C:/Program Files/Teledyne/Spinnaker/include")
```

Il ne faut pas oublier d'ajouter la librairie spinnaker à la méthode `target_link_libraries` pour que le projet puisse utiliser les fonctions de l'API -> target_link_libraries(first_project PRIVATE Qt${QT_VERSION_MAJOR}::Widgets spinnaker).

Armand est en train de travailler sur les changements de paramètre des caméras. Pour cela il a besoin de différentes types d'input : slider, text, combobox, etc. Il a réussi à faire fonctionner les sliders et les textes mais la combobox présente plus de difficulté car il a besoin d'un enum donc il doit trouver comment faire cela en C++.

J'ai réussi pour l'instant à récupérer une image durant le construction de la window et de l'afficher et je vais maintenant essayer de récupérer une image en boucle pour afficher un flux vidéo.

En utilisant un QTimer qui va demander l'affichage de l'image suivante toutes les x millisecondes il est possible d'afficher le flux. Cependant si les fps de la caméra sont trop élevés cela peut poser problème. En effet toutes les x millisecondes l'image suivante va être récupéré mais si le caméra charge plus vite les images que le timer ne les affiche cela va créer un décalage entre l'image affiché et l'image que la caméra est en train de filmer. Pour éviter cela il est possible grâce à la NodeMap de Spinnaker de changer les fps pour les mettre plus bas.

Voici un lien utile pour comprendre le focntionnement de spinnaker :

- https://flir.custhelp.com/app/answers/detail/a_id/4327/~/flir-spinnaker-sdk---getting-started-with-the-spinnaker-sdk

Armand:
- Refactorisé son code afin d'avoir des classes pouvant lier un Label et un élément QT, comme avec les EditLine.
- Pareil pour les Slider & pour les comboBox, mais ce dernier s'avère compliquée car les enum en c++ sont moins performant qu'en Java, il cherche a faire une méthode pouvant remplir une ComboBox avec tous les élements d'une énumération, mais cela s'avère trop compliqué & il essaye donc de faire cela avec une map.

## 30/04/24

Nous avons continué le travaille de la veille, Armand cherche encore une solution pour optimiser les combobox pour pouvoir changer les paramètres de la caméra tout en eyant un code propre et facilement étendable. Peronnellement je suis à la recherche d'une autre méthode pour afficher le flux vidéo des caméras ainsi j'ai trouvé ce dépot github qui pourrait être utile https://github.com/sleepbysleep/FLIR_camera_wrapper_for_Qt5_and_PyQt5;

J'ai appliqué ce que j'ai trouvé dans ce lien mais cela ne focntionne pas car l'appli crash dès qu'on lui demande d'afficher les images. Cependant nous avons bien un appel à la fonction d'affichage qui est fait avec un signal. j'ai ensuite passé le reste de la journée à essayer d'intaller cuda sur le nouveau pc pour pouvoir utiliser opencv si besoin mais j'ai rencontré de nombreux problèmes lors de l'installation des drivers nvidia. Et malgrès avec regardé pendant longtemps sur internet je n'ai pas trouvé de solution. J'ai donc essayé d'utiliser l'ancien pc pour utiliser opencv et éviter que l'appline crash mais le résultat est le même. En attendant de trouver une solution plus appropriée j'ai remis la solution d'hier pour afficher le rendu caméra.

Armand: 
- Fini les comboBox, en choissisant une implémentation plus simple en prennant le parti que la classe LinkedComboBox ne devrait s'occuper uniquement que de faire le lien entre l'affichage du label et de ce qui est choisi par l'utilisateur.

- Changer son code afin de faire fonctionner tout cela via une seconde fenêtre, et a changer une LinkedEditLine afin qu'elle maitrise le maximum du slider.


## 01/05/24

Pour cette journée j'ai tenté de réinstaller les drivers nvidia, l'utilisation de la commande ubuntu-drivers autoinstall ne fonctionne pas pour à peu près les même raisons que dites dans ce lien : https://askubuntu.com/questions/1510543/unable-to-install-nvidia-drivers-with-apt-get j'ai donc décidé de regarder le driver à installer avec la commande ubuntu-drivers devices, j'ai donc installer manuellement le driver nvidia-550. J'ai également du passer par le sécure boot comme précisé dans la vidéo suivante : https://www.youtube.com/watch?v=A0gxy3xaJlE. Cela a fonctionné et j'ai donc continué par l'installation de cuda pour cela j'ai suivit cette vidéo : https://www.youtube.com/watch?v=8i3BiWa5AZ4&t=425s bien que la version que je dois installer (visible avec nvidia-smi) est différente (il installe cuda 12.2 et moi cuda 12.4). Voir aussi la doc officielle : https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#post-installation-actions. J'ai ensuite installé cuDNN en suivant les indications du lien suivant (juste pour cudnn) : https://gist.github.com/denguir/b21aa66ae7fb1089655dd9de8351a202. Attention le lien d'installation est pas bon car il nous faut cudnn 8.9.7 pour cuda 12, pour cela il faut avoir un compte nvidia avec un accès developpeur (gratuit) et aller dans les archives cudnn pour télécharger le .deb de la bonne version. Je conseil de déplacer le fichier .deb dans /var/cache/apt/archives/ avant de l'installer pour éviter les problèmes de droits. J'ai continé par tenté l'installation d'opencv mais ça n'a pas marché pour les raisons précisées dans ce lien : https://github.com/opencv/opencv_contrib/issues/3690. J'ai donc istallé cuda 12.2 qui m'a permis d'installer opencv avec cuda.

Pour finir la journée (nous partons plus tôt car c'est un jour férié et on a pas pu manger car tout était fermé) j'ai commencé à écrire un rapport plus complet sur le projet de la première semaine qu'on va rendre à Tomas Holt. Pour l'instant j'ai écrit jusqu'à la fin de la permière semaine cad faire fonctionner les scripts de calibration des caméras.

Armand:
- Importation du projet,
- Ajout de la page des paramètres
- Importation du code fait en amont
- Ajout de la fonctionnalitée du Slider pour l'exposure ainsi qu'un bouton pour activer automatiquement le temps d'exposure automatique -> empêche l'utilisateur de changer la valeur manuellement.

## 02/05/24

Pour ce début de journée je me suis replongé dans les exemples de spinnaker pour trouver comment faire pour récupérer les images grâce à un event Spinnaker. Dans le code source il est possible de trouver un dossier imageEvent contenant un fichier c++ expliquant comment se déroule la récupération d'images via un event. J'ai appliqué ce que j'ai trouvé dans ce fichier à notre application et j'ai pu récupérer et afficher les images sans problème. J'ai clean le code pour enlever les parties plus utiles et j'ai enlevé le limiteur de fps. Sans ce dernier l'application bug je l'ai docn augmenté graduellement jusqu'à 60 fps et mon but maintenant c'est de trouver un moyen d'appliquer une limite de fps automatique. Après plusieurs tests j'ai remarqué que le nombre de fps est adapté sur mon pc pour ne pas faire lager mais sur le pc d'armand cela ne fonctionne pas et l'application lag. J'ai donc remis le limiteur de fps à 30 pour que l'application fonctionne correctement sur les deux pc. Le plus important c'est maintenant d'avoir une manière propre de récupérer les images et de les afficher sans problème. 

J'ai passé une bonne partie de la journée à finir le rapport sur le projet de la première semaine.

Armand:
- Fait une nouvelle road map de ce que je dois faire
- Refacto du code pour utiliser un fichier UI pour la page settings
- Ajouts de divers bouton dont start & stop recording
- Ajout de l'affichage du nom de la caméra et du vendeur

## 03/05/24

Armand:
- Ajout d'une fenêtre spéciale qui affiche tous ce qui était codée précedemment
- Lien des pages entre elles -> s'assure que des fenêtre zombies ne soit pas là quand leurs parents sont détruits
- Ajout d'un slider gérant le gain de la caméra
- Ajout d'un slider pour gérer le framerate fixé ainsi que le "vrai" framerate calculé
