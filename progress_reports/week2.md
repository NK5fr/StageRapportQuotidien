# Week 2

## 22/04/2024

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

## 23/04/2024

Après certaine recherches nous avons trouvé que la version cudnn9 est peut être trop élevée donc on install la version cudnn8.9.7 avec opencv 9.0. OpenCV compile voici une mise à jour des versions utilisées :

- cuda 12.2
- cuDNN 8.9.7 (pour cuda 12)
- openCV 4.9.0
- gcc 11.5
- g++ 11.5

Nous avosn ensuite construit l'éxecutable de live_disparity mais il n'a ntoujours pas marché et il renvoie la même erreur, cependant le version de cuda est bonne donc on cherche l'origine de l'erreur.

L'erreur proviens du fait que le script que l'article nous donne spécifie a l'aide de "-D CUDA_ARCH_BIN=6.2" que la "Compute Capacity" (CC), soit la vitesse a laquelle le GPU peut calculer des operations de matrices, est de 6.2.
L'ordinateur sur lequel nous sommes entrain de faire ayant une RTX 4090, le CC vaut 8.9 et donc nous devons changer le paramètre de make en : "-D CUDA_ARCH_BIN=8.9"

Nous avons recompilé openCV avec cette nouvelle option puis compilé live_disparity et cela a fonctionné. Nous avons donc pu voir le rendu de la caméra de gauche et la carte de disparité de la caméra de droite. Cependant la carte de disparité n'est pas correcte, il y a des erreurs de calculs. Nous allons donc devoir revoir les paramètres de calibration des caméras. On pense que l'éloignement des caméras est trop grand mais nous n'avons pas le matériel pour les rapprocher en les laissant stable.

En attendant nous allons essayer de faire marcher le perception de la profondeur et ds personnes. Pour cela nous suivont la dernière partie du lien https://www.flir.eu/discover/iis/machine-vision/how-to-build-a-custom-embedded-stereo-system-for-depth-perception/. La perception des personnes se fait gràace à jetson-inference, on s'est rendu compte que l'installation de jetson-inference ne marche pas non plus donc on va essayer de le faire marcher.

Premièrement il y a une confusion dans le placement des dossiers, nous les plaçons donc dans le home. On a aussi remarqué qu'il faut installer TensorRT.

On a pas le pc habituel pour cette après midi on essaie donc de de compiler jetson-inference sur le pc de substitution de manière à ce qu'on puisse le tester sur le pc habituel demain.

TensorRT, Cuda, Cudnn sont tous des outils installables sur le site de nvidia.
Les compabilités entre TensorRT, Cuda & cudNN sont trouvable sur [ce site](https://docs.nvidia.com/deeplearning/tensorrt/support-matrix/index.html). C'est ici que nous trouvons que les versions 10.0.x de TresorRT sont compatibles avec 12.2 de CUDA & avec cuDNN 8.9.7 pour les distributions Linux x86_64, c'est donc cette version que nous choisisons.

D'ailleur pour savoir les bonnes versions à utiliser voici des instructions :
- Pour les drivers ubuntu : `ubuntu-drivers devices` cela affiche les drivers recommandés pour la carte graphique.
- Pour la version de CUDA : `nvidia-smi` cela affiche la version de CUDA qu'il convient (il faut installer les drivers nvidia avant).
- Pour la version de cuDNN : `nvcc --version` vous affichera la version de CUDA à laquelle il faudra adapter cudnn. Pour openCV je vous conseil de prendre cudnn 8.9.7.
- Pour OpenCv : prenez la dernière version stable 4.9.0.
- Pour l'option de cmake -D CUDA_ARCH_BIN= : regardez sur internet la version de votre carte graphique et adaptez le CC en conséquence.
- gcc et g++ : prenez la dernière version stable 11.5. Et changez la si vous avez des erreurs de compilations.

Certaine erreurs de compilations sont provoquées par des erreurs dans les fichiers de opencv. Il est possible de nécessaire parfois de les modifier pour que cela fonctionne. Regardez sur internet si quelque chose de la sorte se présente.

Nous n'arrivons pas à installer la bonne version de tensorrt. On a aussi installé vpi car il est mentionné dans le CMakeList.txt.

L'installation de tensorRT ne marche pas sur le pc de substitution. On va essayer de le faire marcher sur le pc habituel demain.

## 24/04/2024

L'installation de tensorrt fonctionne sur le pc habituel mais il nous est impossible de trouver certain fichiers de tensorrt en on ne sais pas comment les récupérer pour pouvoir compiler jetson-inference. Le deep learning semble donc être pas possible à effectuer pour le moment. Il nous faut donc pour l'instant nous concentrer sur la calibration des caméras. Cela semble être une tache difficile car malgrès les nombreux essais nous n'arrivons pas à obtenir une carte de disparité correcte. Il faudrait prendre un maximum de photos dans plusieurs angles pour pouvoir calibrer les caméras correctement. Nous allons aussi ajuster les autres paramètres pour rendre l'image des caméras le plus net possible.

Nous effecutons aussi nos premiers essaies sur Qt qui est un framework qui nous permet pour l'instant de créer des interfaces graphiques. Nous avons pour l'instant réussi à créer une fenêtre avec un bouton. 

Après avoir passé plusieurs heures à avoir des fichiers de calibration corrects nous n'avons toujours pas réussi à avoir une carte de disparité correcte.

Après que Tomas Holt soit venu nous parler dans la matinée, il nous a dit que nous allions utilisé QT. Nous avons donc commencer a prendre en main le framework ainsi que d'IDE proposé pour créer des projets avec ce framework.

## 25/04/2024

Nous avons continué à travailler sur la calibration des caméras ce matin. Nous avons essayé plusieurs angles, à des distances différentes avec des chestboard différents avec un décors vide ou non, avec nous sur les photos ou non, avec des caméras différentes, avec un nombre de photos différent mais rien n'y fait la calibration est toujours aussi mauvaise. De plus nous avons perdu l'accès au pc donc nous ne pouvons plus travailler sur le projet pour le moment.

Nous pensons que la qualité des caméras est peut être en partie responsable de la mauvaise calibration mais il semble difficile de pouvoir modifier ça car nous avons déjà utilisé toutes les caméras que nous avions à disposition.

Nous allons également essayer de faire une interface graphique avec Qt pour pouvoir afficher les images des caméras et pouvoir éventuellement changer les paramètres de ces dernières. Pour intégrer les caméras nous allons utiliser une API caméras (spinnaker). Il nous faut donc télécharger l'API spinnaker et l'installer. On nous donnera par la suite une autre application qui fera sensiblement la même chose qu'on devra améliorer sur la dernière version de Qt depuis une version 5.x de Qt. Cette application à améliorer consiste à afficher plusieurs caméras en même temps et de pouvoir les contrôler. Cette app utilise OpenGL.

Nous avons ensuite passé une bonne partie de la journée à s'entrainer sur Qt et nous avons aussi essayer de changer le cablage des caméras pour voir si cela pouvait améliorer la calibration. Nous avons aussi essayé de changer les paramètres de calibration mais rien n'y fait.

## 26/04/2024

Nous avons passé l'ensemble de la journée à travailler sur Qt. Armand s'est concentré sur le code et à comment créer une interface graphique avec ce framework. Il a crée différentes pages avec des boutons qui réagissent quand on clique dessus mais il a surtout fait une page qui simule une interface qur laquelle on peut changer les paramètres des caméras avec des LineInput, des labels et des sliders.

De mon côté j'ai passé une grande partie de la journée a comprendre comment lier l'API Spinnaker et Qt, en effet nous avons besoin de cette API pour lire les images des caméras ainsi que pour changer ces paramètres. L'import de l'API sur linux fut assez simple cependant l'import sur windows provoque une erreur à la compilation du projet comme si les fichiers c++ de spinnaker n'arrivent pas à être compris. 
La documentation sur l'import de l'API sur windows est assez pauvre et je n'ai pas encore de solution. Si aucune solution n'est trouvé nous devrons certainement tou faire sur linux.