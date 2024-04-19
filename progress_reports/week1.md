# Week 1

## 17/04/2024

Le projet nous à été introduit par notre superviseur aujourd'hui, le projet consiste à avoir calculer la distance entre un duo de caméras et un objet visible par ces dernières. Le principe est expliqué ici https://www.flir.eu/discover/iis/machine-vision/how-to-build-a-custom-embedded-stereo-system-for-depth-perception/. 

Le matériel utilisé pour le projet est le suivant:
- 2 caméras FLIR Blackfly S BFS-U3-13Y3C-C
- 2 cables USB3.0 -> micro B pour connecter les caméras à l'ordinateur
- 1 boitier contenant un script permettant de synchroniser les caméras

Nous avons premièrement utilisé spinnaker https://www.flir.eu/products/spinnaker-sdk/ pour capturer les images des caméras. 

Nous avons ensuite récupéré les scripts python et c++ nécessaires pour le projet sur ce lien https://flir.app.box.com/s/l7kb3idgddoh23xshwis6svs6jo3vu5c/file/1017545184711.

Nous avons ensuite installé OpenCV https://opencv.org/releases/ qui permet aux scripts de pouvoir analyser les images des caméras.

Nous avons ensuite installé cmake https://cmake.org/download/ qui permet creer des fichiers de configurations pour make.

Cette première journée est principalement centrée sur la prise en main du matériel et la compréhension du projet.

Spinnaker fonctionne mais le reste est à faire sur linux et nous n'avons pas réussit à faire tourner les scripts sur windows.

## 18/04/2024

Aujourd'hui nous allons réessayer de faire fonction les scripts sur linux.

Nous avons donc suivit les instructions du site https://www.flir.eu/discover/iis/machine-vision/how-to-build-a-custom-embedded-stereo-system-for-depth-perception/.

Nous avons commencé par faire la calibration en suivant les instructions et en modifiant les fichiers pour adapter à notre hardware.

On a modifié le fichier CMakeLists.txt pour ajouter les bonnes lib (vérifier que que les fichiers ajoutés sont bien dans le bon répertoire).

On doit aussi modifier les configurations des caméras dans le fichier source .ccp en adaptant les setValue par rapport à ce qu'on a dans 
spinnaker. Dans ce même fichier on enlève les références à TriggerSoftware car inutile dans notre cas (Notre Trigger source est en Line0 et non software). On a aussi mis le release du système à la fin du script car on n'arrive pas à kill les buffer ensuite si on le laisse avant.

Après avoir fait les modifications pour pouvoir enregistrer les images on a réglé l'exposition et le gain des caméras pour avoir une image correcte.

On a ensuite lancé le script pour prendre 20 paires de photos différenres d'un chestboard.

On doit ensuite lancer un script python pour générer les fichiers de calibration. On redimmensionnne le tableau dans le fichier par rapport à la taille de notre chestboard.

Le chestboard ne correspond pas à celui nécessaire donc pas possible de calibrer pour l'instant.

## 19/04/2024

Nous avons imprimé un nouveau chestboard qui sera reconnaissable par le script python.

Nous avons ensuite recalibré les caméras puis avons pris 20 nouvelles photos.

Nous avons ensuite lancé le script python pour générer les fichiers de calibration à partir des photos. Les fichiers sont les suivants :

- intrinsics.yml : paramètres intrinsèques des caméras, c'est à dire les paramètres qui dépendent de la caméra elle-même (focale, distorsion)

- extrinsics.yml : paramètres extrinsèques des caméras, c'est à dire les paramètres qui dépendent de la position relative des caméras (position 3D des caméras, rotation)


Une fois les fichiers créés, il nous faut lancer le script c++ permettant d'afficher en "live" le rendu normal de la caméra de gauche et afficher le rendu de la caméra de droite en tant que carte de disparité. Pour cela il nous faurt premièrement modifier le script pour l'adapter à notre hardware en changeant les numéros de séries des caméras. Changer les valeurs de paramètres des caméras par rapport à celles dans spinnaker. Enlever les références à TriggerSoftware car inutile dans notre cas (Notre Trigger source est en Line0 et non software).

Il nous faut ensuite modifier le fichier CMakeLists.txt pour ajouter les bonnes lib (vérifier que que les fichiers ajoutés sont bien dans le bon répertoire). Nous avons installé cuda 11 avec cuDNN (nous avions installé des autres versions mais incompatible avec notre version de openCV). Nous avons du ensuite lancer le script d'installation de openCV en supprimant les références à néon car il n'est pas compatible avec notre cpu.
Etant donné que nous avons dû prendre des anciennes versions de cuda, cuDNN et OpenCV nous avons du changer la version de gcc et g++ pour une ancienne.

On a ensuite suivi les instructions pour créer l'éxécutable de live_disparity (en corrigeant les erreurs de compilation). Puis on copie les fichiers de calibration dans le répertoire de l'éxécutable.

Nous avons des problèmes de compatibilité avec le hardware donc on recommence avec les versions des plus récentes. 

L'installation de cuda n'as pas fonctionné.