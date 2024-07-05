# Week 12

## 01/07/2024

Aujourd'hui, j'ai continué à travailler sur l'application. Je n'ai pas vraiment d'idée d'ajouts à faire donc j'ai regardé pour nettoyer le code et pour régler d'éventuels crashs. Comme on a pu le voir durant le développement, l'événement qui est le moins bien géré dans l'application est la déconnection des caméras. On a déjà résolu beaucoup de crash qui survenait à cause de cet événement. J'en ai trouvé un supplémentaire qui est provoqué par le fait qu'il est possible de stoper l'analyse des caméras et donc de leur déconnection. Dans la situation où l'analyse est stopée et qu'une caméra est donnectée, celle ci reste dans la liste des caméras et il est toujours possible d'ouvrir sa fenêtre. Si l'utilisateur appuie sur le bouton pour récupéré une image, l'application crash. Cela vient du fait que la récupération d'image touche aux paramètres de la camera qui est alors déconnectée dans notre situation. J'ai donc simplement ajouté un try catch qui fait que la fonction renvoie un nullptr si la caméra est déconnectée. Dans ce cas, rien n'est récupéré ni affiché.

Comme d'habitude, j'ai ajouté un tuto de comment corriger cette erreur dans le system documentation de l'application.

## 02/07/2024

Aujourd'hui j'ai continué à travailler sur l'application. J'ai fait un tour complet de toutes les fonctionnalités de l'application toujours à la recherche de crashs. J'ai tenté plusieurs combinaisons de features pour voir si certaines d'entre elles ne pouvait pas fonctionner en même temps mais au final j'ai rien trouvé et je n'ai pas trouvé de crash.

## 03/07/2024

Aujourd'hui j'ai continué à travailler sur l'application. Nous avons enfin fait un meeting avec la personne qui succède à notre supervision. On a présenté globalement l'application avec les changements qu'on a ajouté puis on a parlé de ce qu'il restait à faire. Il nous a été demandé de faire quelques changements esthétiques à l'application ainsi que d'ajouter des pages explicatives pour les widgets complexes. Ce n'est pas des tâches qui devraient prendre beaucoup de temps donc on ve certainement finir le stage en cherchant des crashs et en améliorant le rapport final qu'on envera le dernier jour à nos superviseurs. Pour finir la journée, j'ai cherché des crash de l'application et j'en ai trouvé un lié à la déconnection des caméras. Je n'ai cependant pas eu le temps de le résoudre donc je finirai demain.

## 04/07/2024

Aujourd'hui, j'ai continué à travailler sur l'application. J'ai résolu le crash que j'avais trouvé hier. Il était encore une fois lié à des paramètres qui sont manipulés alors que la caméra est déconnectée. J'ai ajouté un try catch pour gérer le cas où la caméra est déconnectée. J'ai aussi ajouté un message d'erreur pour informer l'utilisateur de l'erreur. J'ai ensuite testé l'application pour vérifier que le crash ne se produisait plus et tout fonctionnait correctement. Pour continué, j'ai remarqué que le ligth theme qu'Armand avait ajouté hier ne marchait pas bien. Je lui ai donc indiqué pour qu'il corrige et après j'ai modifié un peu pour changer la couleur de fond du central widget.

J'ai ajouté ces changements de ligth theme dans le system documentation de l'application.

Comme d'habitude, j'ai continué à chercher des erreurs et autres crashs dans l'application; Après quelques recherches j'ai trouvé des erreurs dans le tableau qu'Armand a créé pour facilté la selection des marqueurs. Je lui ai montré mais je n'ai rien changé car il maitrise mieux cette partie que moi donc il est mieux placé pour corriger ces erreurs.

J'ai ensuite remarqué que l'action quitter dans le MenuBar ne marchait pas et n'était pas implémentée dans le code. J'ai donc changé le code pour que cette action soit écoutée et que l'application se ferme correctement quand on clique dessus.

J'ai ajouté ces changements dans le system documentation de l'application.

## 05/07/2024

Aujourd'hui, j'ai continué à travailler sur l'application. Je me suis attardé un peu sur le color mode. En effet lors de l'activation de celui ci, je désactive beaucoup de features de l'application pour éviter des problèmes du fait que les images sont en couleur plutôt qu'en noir et blanc. Cependant, je désactive beaucoup de ces features par précausion alors qu'elles ne posent pas vraiment de problème avec le color mode. Ainsi, j'ai décidé d'assouplir un peu les restrictions du color mode pour permettre à l'utilisateur de profiter de plus de features. J'ai donc modifié le code pour que certains menu du menu bar restent activée alors qu'avant ils étaient tous désactivés.

Pour continué j'ai encore cherché des problèmes dans l'application et j'ai remarqué que la rotation de l'environnement 3D de SC allait à l'envers dans certaines situations. J'ai donc fait de longues recherches et analyses des valeurs pour comprendre d'où venait le problème. En C++, une valeur float se traduit automatiquement en int si besoin. Le problème venait de là car la position de la souris qui est devenu un float avec le devicePixelRatio étaient enregistrée comme un int ce qui faussait les valeurs. J'ai donc fait en sorte que toutes les valeurs soient float et j'ai expliqué les changements dans le system documentation de l'application.