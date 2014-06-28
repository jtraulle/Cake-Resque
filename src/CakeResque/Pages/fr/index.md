<div class="hero-unit headline">
<dfn>CakeResque</dfn> est un plugin CakePHP pour créer des tâches en arrière plan qui peuvent être exécutées hors-ligne plus tard.
</div>

<div class="pull-right">
<iframe src="http://ghbtns.com/github-btn.html?user=kamisama&amp;repo=cake-resque&amp;type=watch&amp;count=true"
allowtransparency="true" frameborder="0" scrolling="0" width="110" height="20"></iframe>


<iframe src="http://ghbtns.com/github-btn.html?user=kamisama&amp;repo=cake-resque&amp;type=fork&amp;count=true"
allowtransparency="true" frameborder="0" scrolling="0" width="95" height="20"></iframe>
</div>



## Introduction {#introduction}

Le but principal est de remettre à plus tard certaines tâches non-essentielles, ce qui permet de réduire le temps d'attente de l'utilisateur.

## La réalité

<img src="/img/workflow1.jpg" class="pull-right" alt="Processus classique" title="Processus classique" width=361 height=615/>
Disons qu'un utilisateur veut mettre à jour sa position, et que votre site a des fonctionnalités sociales avancées centrées autour de l'utilisateur.

Processus principal pour l'action de *mise à jour de la position* :

1. 	*Mettre à jour la position de l'utilisateur* dans la table des utilisateurs (où ailleurs, cela dépend de votre structure)  
	-> prend 0,2s
2.	*Regénérer différents caches*  
	-> prend 0,1s
3. 	*Envoyer des courriels*  
	-> prend 0,7s	
4.	*Envoyer des notifications* à tous vos amis   
	-> prend 3,7s
5. 	*Recommander de nouveaux amis* près de votre nouvelle position  
	-> prend 2s
	
**Temps total** : 3,7 secondes

Vos successions peuvent varier mais ce que vous dever retenir ici, c'est que les choses importantes ne prennent qu'une fraction du temps total de traitement.

## Le problème

Mais l'utilisateur ne se soucie pas de toutes ces étapes, et veut juste mettre à jour son emplacement.

Pourquoi l'utilisateur devrait-il attendre le rafraîchissement du cache ? Ce n'est pas grave du tout si le cache n'est pas rafraichi avant que nous renvoyons une réponse à l'utilisateur.

Et qu'arrivera t'il si une erreur survient lors du rafraîchissement du cache ?  

* Réponse A : Cela plante votre application <small>(vous devriez revoir votre code …)</small>  
* Réponse B : Ignorer cela, et continuer <small>(qui le fera alors ?)</small>

Le problème ici est qu'il y a certaines actions dans le processus principal qui :

* ne nécessitent pas une exécution immédiate
* sont susceptibles de générer des erreurs, qui peuvent mettre hors ligne la totalité de votre application
* peuvent prendre du temps à s'exécuter, donc ralentir le temps total d'exécution
* peuvent coûter des ressources système

## La solution

**L'utilisateur devrait simplement attendre 0,2s au lieu de 3,7s**

Une réponse devrait-être retournée immédiatement après le premier point, et *l'ensemble des autres tâches devraient être différées pour une exécution ultérieure, en dehors du processus principal*. Ce cette façon, les tâches "non essentielles" n'affectent pas le processus principal.

L'astuce est de dire à quelqu'un d'autre de le faire.

<img src="/img/workflow2.jpg" alt="Worflow with background jobs"  title="Worflow with background jobs" width=676 height=567/>

C'est ici qu'interviennent les tâches de fond.

Au lieu d'exécuter la tâche, nous la convertissons en *job* (travail), puis nous le plaçons dans une *queue* (file). Un *worker* (travailleur) qui s'exécute dans un autre processus php, viendra piocher dans cette file et exécuter le travail.
Instead of executing the task, we convert it into a *job*, then put it in a *queue*. A *worker*, which is running on another php process, 

Ce système de files est basé sur Resque. CakeResque est simplement une ensapsulation de Resque pour CakePHP.

> Resque (prononcer comme "rescue" [en]) est une librairie basée sur Redis pour créer des tâches de fond,
> en placant ces tâches dans plusieurs files, pour les traiter plus tard.

<div class="alert alert-info"><i class="icon-book"></i> Lisez la page officielle de <a href="https://github.com/defunkt/resque">Resque</a> [en] pour plus de détails à propos des tâches de fond, des travailleurs et des files.</div>




<div class="hero-unit">
Maintenant, dirigez vous vers le <a href="/install" class="btn btn-info">guide d'installation</a><br/> puis poursuivez avec le <a href="/usage" class="btn btn-success">guide d'utilisation</a>
</div>


## Ressources {#resources}

* 	[Page officielle de Resque](https://github.com/defunkt/resque)
* 	[php-resque : portage vers PHP de Resque (originellement en Ruby)](https://github.com/chrisboulton/php-resque)
* 	[Serie of 9 tutorials explaining the mechanic behind backgound jobs](http://www.kamisama.me/2012/10/09/background-jobs-with-php-and-resque-part-1-introduction/)


## Support {#support}

Le support, les rapports d'anomalies, et les propositions de nouvelles fonctionnalités se font sur <a href="https://github.com/kamisama/Cake-Resque/issues">Github</a>.


## Auteur {#author}

Wan Qi Chen

* Email : [kami@kamisama.me](mailto:kami@kamisama.me)
* Twitter : [@Wa0x6e_k](https://twitter.com/Wa0x6e_k)
* Github : [https://github.com/kamisama](https://github.com/kamisama)
* Google+ : [Google+](https://plus.google.com/116246394244628198627?rel=author)


## Notes de versions {#changelog}

Voir [les notes de verisons ici](https://github.com/kamisama/Cake-Resque/blob/master/CHANGELOG.md)

## Licence {#licence}

CakeResque is licensed under the <a href="http://www.opensource.org/licenses/mit-license.php">MIT License</a>.<br />

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Redistributions of files must retain the above copyright notice