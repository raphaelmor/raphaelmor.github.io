---
layout: post
title: "Bug Report : UITableView, Autolayout, Translucent Navigation Bar et Rotation"
category : iOS
comments: true
---

iOS 7 a chamboulé les layouts (pour le mieux!) en rendant la Status Bar transparente, et en offrant la possibilité de rendre les `UINavigationBar` transluscides.

Cela complique le layout des `UIScrollView` et donc des `UITableView` qui doivent donc gérer un inset afin de caler le contenu au bon endroit, et de pouvoir le faire scroller sour la `UINavigationBar`.

Pour faciliter les choses, **iOS 7** a introduit la propriété `automaticallyAdjustsScrollViewInsets` (et son équivalent *Adjust Scroll View Inset* dans Interface Builder). Le `UIViewController` va ajuster "magiquement" la `UIScrollView` (ou sous classe) pour configurer les bons insets.

Le problème que j'ai rencontré c'est ce **"magiquement"**. Je n'utilise jamais d'`UITableViewController` (car je n'aime pas avoir le view controller en tant que `delegate`/`datasource` de la `UITableView`), j'ai donc souvent des `UIViewController` dont la vue principale est une `UIView` qui contient une `UITableView`.

Cette configuration ne fonctionne pas avec l'ajustement automatique des insets (notament lors de la rotation).

![tableview autolayout fail](/assets/tableview-autolayout-fail.png)

Après quelques recherches, le `automaticallyAdjustsScrollViewInsets` ne fonctionne que lorsque la scrollview ou la tableview est la vue principale du viewcontroller, ou qu'elle se situe à certains endroits de la hierarchie de vues (à condition qu'il n'y ait qu'une seule `UIScrollView` dans la hiérarchie de vue.

##Fix##
Dans mon cas, je me suis simplement contenté de mettre la tableview en vue principale :
![tableview main view](/assets/uitableview-main-view.png)
Mais je regrette qu'apple ne documente pas plus ce genre de comportement "magique".

