---
layout: post
title: "Build an iOS Swift App, Part 1 : L'App Definition Statement" 
category : iOS
comments: true
---

Cette série d'articles est un prétexte pour relater mes expérimentations avec [Swift](https://developer.apple.com/swift/). 
Le but est de créer une appli de toute pièce, en suivant les recommandations d'Apple dans le [iOS Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/)

## L'App Definition Statement

> An **app definition statement** is a concise, concrete declaration of an app’s main purpose and its intended audience.

Etant passionné de photo argentique, j'ai envie de faire une simple application qui permet de lister les lieux liés à la photographie argentique (Labos, Magasins, Galleries, etc).

### Brainstormer les fonctionnalités
Listons dans un premier temps les fonctionnalités telles qu'elles me passent par la tête.

* Voir les lieux sur une carte
* Voir une liste des lieux proches
* Voir le détail d'un lieu
* Rechercher un lieu selon des critères
* Laisser un avis sur le lieu
* Lire les avis sur un lieu
* Poster une photo d'un lieu
* Noter un lieu
* Partager un lieu sur les réseaux sociaux

### Déterminer les utilisateurs type

L'utilisateur type est un(e) jeune photographe de 20 à 30 ans, ayant découvert la photo argentique récement, et cherchant à découvrir les lieux liés à cette pratique.

### Filtrer la liste des fonctionnalités

En basant notre liste de fonctionnalités sur le profil de notre utilisateur type, on peut mettre de côté toute la partie avis / photos / notes.

Nous pouvons donc définir un [M.V.P.](http://fr.wikipedia.org/wiki/Produit_minimum_viable) simpliste qui nous servira à valider le concept.

Voici donc notre produit minimum, les fonctionnalités ont été réordonnées selon leur valeur: 

* Voir une liste des lieux proches
* Voir le détail d'un lieu
* Voir les lieux sur une carte
* Rechercher un lieu selon des critères

Dans cette première version, nous nous concentrerons sur un seul type de lieu, les labos photos, afin de simplifier le problème.

Nous pouvons donc formuler notre App Definition Statement comme suit :

> Une application qui aide les nouveaux photographes argentiques à trouver les lieux dédiés à leur passion.

Cette phrase sera notre filtre lorsque l'on décidera d'ajouter ou non une nouvelle fonctionnalité.

## Conclusion

Maintenant que nous savons quoi construire, le prochain article sera dédié au prototypage de l'interface. 
La session "Prototyping: Fake It Till You Make It" de la [WWDC 2014](https://developer.apple.com/videos/wwdc/2014/), est une mine d'or d'informations sur le sujet.

