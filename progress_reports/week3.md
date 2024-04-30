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

De son côté Armand à refactorisé son code afin d'avoir des classes pouvant lier un Label et un élément QT, comme avec les EditLine.
Il a fait de même pour les Slider & pour les comboBox, mais cette dernière s'avère compliquée car les enum en c++ sont moins performant qu'en Java, il cherche a faire une méthode pouvant remplir une ComboBox avec tous les élements d'une énumération, mais cela s'avère trop compliqué & il essaye donc de faire cela avec une map.

## 30/04/24

Armand de son côté a fini les comboBox, en choissisant une implémentation plus simple en prennant le parti que la classe LinkedComboBox (celle qu'il faisait) ne devrait s'occuper uniquement que de faire le lien entre l'affichage du label et de ce qui est choisi par l'utilisateur.

Il a ensuite changer son code afin de faire fonctionner tout cela via une seconde fenêtre, et a changer une LinkedEditLine afin qu'elle maitrise le maximum du slider.
