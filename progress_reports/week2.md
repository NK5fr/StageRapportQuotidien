# Week 2

## 24/04/2024

Nous avons commencé la journée un peu plus tard car nous n'avions pas accès au pc habituel mais on a pu obtenir un pc de substitution en attendant en debut d'après midi. Sur ce pc nous avons essayé de faire un installation propre de cuda grâce notament aux informations de ce site https://gist.github.com/denguir/b21aa66ae7fb1089655dd9de8351a202#open-python-and-execute-a-test. Une fois que cuda et cuDNN étaient installés nous avons installé la version 9.0 de openCV cependant la compilation à échoué et nous tentons d'installer une version un peu plus ancienne de openCV qui pourrait être compatible avec notre version de cuda et cuDNN. 

De plus nous avons eu accès au pc habituel à partir de 16h mais ubuntu n'a pas voulu démarrer donc impossible de travailler dessus. Nous continuons donc sur le pc de substitution.

La compilation de openCV ne marche pas en raison de fichiers cuda qui ne peuvent pas se build.

Nous avons réussi à lancer ubuntu sur notre pc habituel en passant par le recovery mode puis nous avons régressé de version gcc et g++ afin de pour compiler openCV. Récapitulatif des version utilisée pour l'instant :

- cuda 11.5
- cuDNN 9 (pour cuda 11)
- openCV 4.5.5
- gcc 9
- g++ 9

Nous avons ensuite suivi les instructions pour créer l'éxécutable de live_disparity (en corrigeant les erreurs de compilation). Puis on copie les fichiers de calibration dans le répertoire de l'éxécutable. Nous avons réglé les problèmes de compilations (position des fichiers paramètres). Nous avons finalement lancé l'éxécutable mais nous avons eu un problème de "kernel image".

Le problème provient du gpu qui est trop puissant pour notre version de cuda. Après avoir utilisé la commande nvidia-smi on s'est rendu compte que c'est cuda 12.2 qu'il nous faut. Nous nous lançons donc dans l'installation de cuda 12.2 et de cudnn9 pour cuda 12. Nous changeons aussi la version de opencv. Ca marche toujours pas on recommencera demain.