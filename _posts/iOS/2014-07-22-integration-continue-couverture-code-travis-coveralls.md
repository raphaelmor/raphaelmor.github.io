---
layout: post
title: "Intégration continue et couverture de code sur iOS avec Travis et Coveralls" 
category : iOS
comments: true
---

Depuis quelques années maintenant, j'accorde une grande importance à l'[intégration continue](http://fr.wikipedia.org/wiki/Intégration_continue) et aux [tests automatisés](http://fr.wikipedia.org/wiki/Test_unitaire) (unitaires et fonctionnels) dans mes développements.
J'ai très souvent eu la chance d'avoir des serveurs d'intégrations continue à disposition chez mes clients ([Hudson](http://hudson-ci.org) au début, puis [Jenkins](http://jenkins-ci.org)). 

Je ne reviendrais pas sur les avantages de l'intégration continue, des tests unitaires automatisés et des boucles de feedback rapides, mais ces outils sont très utiles pour cela. Malheureusement ils sont aussi complexes à installer, configurer, maintenir lorsque l'on veut les utiliser pour des projets personnels.

Heureusement, j'ai découvert récemment que [Travis CI](http://travis-ci.org) supportait les builds Xcode, et que [Coveralls](https://coveralls.io) avait une API qui permettait de supporter n'importe quel environnement.
Autre point important, **Travis** et **Coveralls** sont gratuits pour les dépots [GitHub](https://github.com) publiques.

##Travis CI##

**Travis** s'occupe de l'intégration continue. Il lance automatiquement un build à chaque nouveau ```push```. Il s'interface avec **GitHub**, détecte automatiquement les nouvelles branches et pull requests, et crée de nouveau "jobs" à la volée (Yeah feature branchs !).

###Configuration###

- Se connecter sur [https://travis-ci.org](http://travis-ci.org) avec son compte github.
- Sur la page de compte activer le repository : ![activation dépot](/assets/travis-ci-activate-repository.png)
- Ajouter un fichier ```.travis.yml``` à la racine de votre projet (voici un exemple basique qui build le projet et lance les tests unitaires) :
<script src="https://gist.github.com/raphaelmor/f052c96642f526b53205.js"></script>
- Pousser vos modifications sur GitHub.

**Travis** devrait détecter le nouveau ```push```, et commencer à construire votre projet. 
Par défaut, si votre projet contient un fichier ```Podfile```, **Travis** lance automatiquement ```pod install``` avant de construire votre projet.

Lorsqu'il a fini, **Travis** génère un rapport de build sur sa page. Voici un exemple : ![build history](/assets/travis-ci-build-history.png)

##Coveralls##

> Tout ce qui est mesuré et observé, s'améliore.
>
> -- <cite>Bob Parsons</cite>

Ecrire des tests est une bonne pratique, mais être conscient de sa couverture de test est encore mieux.
**Coveralls** s'interface avec **Travis** et **GitHub** afin de présenter le taux de couverture par les tests, ainsi que son évolution au fil des builds.

La mise en oeuvre étant un peu plus complexe, je me suis basé sur ce très bon article d'*Oliver Drobnik* : [Xcode Coverage](http://www.cocoanetics.com/2013/10/xcode-coverage/) 

###Configurer Xcode###
La génération de fichiers de couverture de code n'étant pas anodine, j'ai décidé de créer une configuration de build spécifique (```Coverage```), afin de ne pas polluer la configuration de ```Debug```.

- Dupliquer la config ```Debug``` et la nommer ```Coverage``` : ![Coverage Configuration](/assets/coveralls-build-config.png)

- Génerer les fichier de couverture pour la cible principale et la cible de test : ![Generate Coverage](/assets/coveralls-generate-coverage.png)![Generate Coverage Tests](/assets/coveralls-generate-coverage-tests.png)

- Instrumenter le flux d'execution pour les deux cibles : ![Instrument Program Flow](/assets/coveralls-instrument-flow.png)![Instrument Test Flow](/assets/coveralls-instrument-flow-tests.png)

- Ensuite, il suffit de créer un ```Scheme``` Coverage qui sera executé par **Travis**, et de configurer les parties ```Run``` et ```Test``` afin d'utiliser la configuration ```Coverage``` : ![Coverage](/assets/coveralls-coverage.png)

Suite à ces manipulations, notre projet Xcode est configuré pour générer des fichiers ```.gcda``` et ```.gcno``` qui nous serviront à calculer la couverture de code. Il nous reste ensuite à configurer **Travis** afin qu'il utilise ce nouveau ```Scheme```, et à uploader les résultats sur **Coveralls**.

###Configurer Travis et Coverall###

- Modifier le fichier ```.travis.yml``` en faisant pointer le script de test vers le scheme créé précédement. Comme par exemple : <script src="https://gist.github.com/raphaelmor/f052c96642f526b53205.js"></script>

Afin d'uploader les résultats sur **Coveralls**, j'utilise un script créé par *Oliver Drobnik* (disponible [ici](https://github.com/cocoanetics/ruby)). Il suffit de le placer à la racine de son projet (en s'assurant qu'il soit bien exécutable), et de modifier le fichier ```.travis.yml``` comme ceci (le script dépend de ```cpp-coveralls```) :
<script src="https://gist.github.com/raphaelmor/da53787731c3227a8a5d.js"></script>

Si tout est bien configuré, vos résultats de couverture de test seront bien uploadés. **Coveralls** vous les présentera en montrant la progression au fil des builds. 
![Coveralls History](/assets/coveralls-results.png)

##Bonus##

- Ces deux outils vous permettent d'inserer des badges dans votre ```README.md``` ou dans une page ou un site web. 
![Travis Coveralls Badges](/assets/travis-coveralls-badges.png)
- **Travis** et **Coveralls** s'intègrent parfaitement avec **GitHub** pour les pull requests. **Travis** affiche le résultat de build directement sur la page de la pull request, et **Coveralls** ajoute un commentaire indiquant la couverture de code et sa progression pour la pull request.
![Travis Pull Request](https://camo.githubusercontent.com/6e5cb0fc3f29a7d5b523cae506f7d936a06a7db9/68747470733a2f2f6769746875622d696d616765732e73332e616d617a6f6e6177732e636f6d2f736b697463682f7374617475732d6d657267652d627574746f6e2d32303132303833312d3133343835372e6a7067)
![Coveralls Pull Request](/assets/coveralls-pull.png)


##Conclusion##

Et voila, normalement, lorsque vous pousserez vos fichier sur **GitHub**, **Travis** se chargera de builder votre projet, et uploadera vos stats de couverture de test sur **Coveralls**. Vous n'avez plus d'excuse pour ne pas utiliser d'intégration continue sur vos projets personnels !
