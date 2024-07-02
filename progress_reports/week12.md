# Week 12

## 01/07/2024

Aujourd'hui, j'ai continué à travailler sur l'application. Je n'ai pas vraiment d'idée d'ajouts à faire donc j'ai regardé pour nettoyer le code et pour régler d'éventuels crashs. Comme on a pu le voir durant le développement, l'événement qui est le moins bien géré dans l'application est la déconnection des caméras. On a déjà résolu beaucoup de crash qui survenait à cause de cet événement. J'en ai trouvé un supplémentaire qui est provoqué par le fait qu'il est possible de stoper l'analyse des caméras et donc de leur déconnection. Dans la situation où l'analyse est stopée et qu'une caméra est donnectée, celle ci reste dans la liste des caméras et il est toujours possible d'ouvrir sa fenêtre. Si l'utilisateur appuie sur le bouton pour récupéré une image, l'application crash. Cela vient du fait que la récupération d'image touche aux paramètres de la camera qui est alors déconnectée dans notre situation. J'ai donc simplement ajouté un try catch qui fait que la fonction renvoie un nullptr si la caméra est déconnectée. Dans ce cas, rien n'est récupéré ni affiché.

Comme d'habitude, j'ai ajouté un tuto de comment corriger cette erreur dans le system documentation de l'application.

## 02/07/2024

Aujourd'hui j'ai continué à travailler sur l'application. J'ai fait un tour complet de toutes les fonctionnalités de l'application toujours à la recherche de crashs. J'ai tenté plusieurs combinaisons de features pour voir si certaines d'entre elles ne pouvait pas fonctionner en même temps mais au final j'ai rien trouvé et je n'ai pas trouvé de crash.