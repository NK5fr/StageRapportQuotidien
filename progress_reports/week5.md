## 13/05/2024

Pour commencer la journée, nous avons chercher à voir si des fonctionnalités de notre application, que nous n'avions pas encore testées, marchaient correctement.
Nous avons donc commencer par vérifier si la sauvegarde le chargement des fichiers de configuaration marchaient, et après avoir vérifié, ça ne marche pas.

Nous avons cherché à comprendre pourquoi ça ne marchait pas, et nous avons trouvé que la sauvegarde des paramètres de fichiers était faite pour les caméras de base, pourtant nous utilisons des caméras avec Spinnaker et donc les paramètres ne sont pas les mêmes.
Nous allons donc devoir modifier la sauvegarde des paramètres pour qu'elle soit compatible avec les caméras Spinnaker.

Nous avons du faire une petite pause car le pc lag énormément et on ne pouvait plus travailler correctement.

Après avoir utilisé la save avec spinnaker nous avons pu sauvegarder les paramètres de la caméra. On a remarqué ensuite que le chargement des paramètres est également flou au niveau des caméras spinnaker et va devoir créer également des fonctions pour charger les paramètres de ces caméras. Le principal problème est que les paramètres ne sont pas les même que les caméras de base. De plus de nombreuses classes et méthodes existent dans le code pour les caméras spinnaker mais ne sont pas utilisées ou sont incomplètes.
Le chargement des paramètres de la caméra ne fonctionnait pas non plus, la raison étant que la méthode pour mettre a jour les paramètres se faisait un nombre de fois égale a une variable qui a laquelle nous ne touchions pas. Après avoir changé la variable, le chargement des paramètres fonctionne correctement, ce qui nous donne un chargement et une sauvegarde fonctionnelle.

Nous avons ensuite changé de pc car les bugs de celui habituel nous empêchait de travailler. Nous avons commencé à regarder toutes les parties inutiles du au fait que nous utilisons uniquement les caméras avec spinnaker. Nous avons mis ces parties inutiles en commentaire car nous ne savons pas si pouvons les enlever ou pas.

Nous avons également commenté des lignes à propos de threads qui provoquent une erreur à la fermeture de l'application. Finalement sans ces lignes les caméras ne sont plus repérées donc on va les remettre et regarder pourquoi ça ne marche pas.

Nous avons aussi une autre erreur qui est que lorsqu'on essaie de fermer la fenêtre d'une caméra l'application plante. L'erreur vient du fait que lorsqu'on envoie l'image on ne vérifie pas si le fenêtre est fermée ou non. Nous avons donc rajouté une vérification pour voir si la fenêtre est fermée ou non et si elle est fermée on n'envoie pas d'image.

Pour finir la journée nous avons corrigé une action d'un bouton qui ne fonctionnait pas. En effet il y avait un bouton permettant de gérer l'activation du tracker de la souris sur les images mais il n'y avait pas d'action associé à ce bouton. Après divers recherches nous avons trouvé quoi lancer à l'activation de ce bouton et nous avons donc associé une action.

## 14/05/2024

Pendant le début de cette journée nous avons principalement fait des recherches sur les rapports de stage et sur ce qu'on va mettre dedans. On s'est aussi renseigné sur markdown mais il semble finalement que certains aspect du rapport sont difficiles à faire et que google docs apporte également des outils efficaces pour un meilleur rendu.

On a ensuite fait un mini meeting avec Tomas Holt pour voir ce qui est à faire pour la suite du projet car nous avons corrigé de nombreuses fonctionnalité et on avait beaucoup de question sur qu'il nous restait encore à faire et à quel point pouvont nous modifier l'application. 

Après ce mini meeting nous avons cherché à compiler le projet sur windows sans succès. Il semblerait que le version du compilateur c++ n'est pas correct mais même après avoir changé la version et testé plusieurs options nous n'avons pas réussi à compiler le projet. Nous avions déjà eu cette erreur avant mais c'était avec un projet contruit différament donc nous ne pouvons pas corriger l'erreur comme on l'avait déjà fait.

On a passé ensuite le reste de la journée à résoudre ce problème. Cependant nous n'avons pas réussi à avoir de bons résultats et nous ne sommes même pas sûr de où provient vraiment le problème. Nous avons trouvé que le problème venait d'un fichier nommé qmake.stash il semblerait qu'il contient de fausses indications à propos du compilateur c++ donc on l'a supprimé temporairement pour voir si ça changeait quelque chose. Nous n'avons plus l'erreur mais nous en avons d'autres car nous devons régler de nombreux problèmes de librairies et de dépendances.