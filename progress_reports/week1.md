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
spinnaker. Dans ce même fichier on enlève les références à TriggerSoftware car inutile dans notre cas (Notre Trigger source est en Line0 et non software). On a aussi retiré le release du système car on n'arrive pas à kill les buffer ensuite si on le laisse

Après avoir fait les modifications pour pouvoir enregistrer les images on a réglé l'exposition et le gain des caméras pour avoir une image correcte.

On a ensuite lancé le script pour prendre 20 paires de photos différenres d'un chestboard.

On doit ensuite lancer un script python pour générer les fichiers de calibration. On redimmensionnne le tableau dans le fichier par rapport à la taille de notre chestboard.

Le chestboard ne correspond pas à celui nécessaire donc pas possible de calibrer pour l'instant.
