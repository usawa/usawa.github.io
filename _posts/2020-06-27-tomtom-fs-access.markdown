---
layout: default
title:  "Tomtom, sdcard et système embarqué: accéder au système de fichiers"
date:   2020-06-27 20:58 +0100
description: "Comment accéder au système de fichiers d'une carte Tomtom, en mode inception."
categories: 
- gps

---

Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

Publié aussi sur LinuxFR.

Je suis depuis peu le propriétaire d'une automobile qui dispose du système embarqué R-Link Evolution qui équipe un certain nombre de modèles Renault. Ce système utilise Tomtom pour la navigation (système que Renault a abandonné pour iGo, on se demande bien quel est l'idiot qui a choisi un système plus pourri que le précédent...). Les cartes sont stockées sur des cartes SD (sdcard). Jusque là, tout va bien.

Comme c'est du Tomtom masqué derrière du Renault, l'outil de mise à jour, appelé R-Link Toolbox, est une copie ratée et bas de gamme de Tomtom Mydrive, qui ne permet pas de préparer ses trajets, ou même de gérer ses propres points d'intérêt. Les mises à jour sont gérées par Renault, pas par Tomtom, et donc obtenir une carte à jour dépend du bon vouloir de Renault (actuellement, la carte a plus d'un an et demi de retard). Et bien entendu, ça ne fonctionne que sous Mac ou Windows. On ne peut donc rien faire? Un passionnés a pondu un logiciel appelé **R-Link Explorer**, qui ne fonctionne que sous Windows, ou sous Linux et Mac à l'aide de Wine. Ce qui est absurde, car non seulement c'est codé avec qt, mais en plus, ça utilise des binaires d'origine linux (les outils ext2/3/4 par exemple). Vous allez comprendre pourquoi.

Du coup, je me suis amusé à voir comment ouvrir le système de fichiers d'une sdcard R-Link, et donc Tomtom pour accéder au vrai système de fichiers, et donc de pouvoir ajouter mes fameux fichiers ov2. Si vous ne le saviez pas, Microsoft a fait un procès à Tomtom en 2009 sur l'utilisation de FAT32. Les détails sont ici:
[https://en.wikipedia.org/wiki/Microsoft_Corp._v._TomTom_Inc.](https://en.wikipedia.org/wiki/Microsoft_Corp._v._TomTom_Inc.)

Tomtom a dû acheter des licences Microsoft pour avoir le droit d'utiliser FAT32 sur ses appareils, sachant que les GPS de tomtom tournent sous Linux et utilisent ext3. Stupide ? Et bien oui et non. Ni Windows, ni MacOS ne reconnaissent nativement une partition ou un système de fichiers ext3. Pour que la carte soit reconnue à l'insertion sur ces OS il a fallu trouver une solution "universelle". La carte reste au format FAT32. Mais où est le système de fichiers ext3 ? Mais dans des fichiers images, pardi !

Sur une sdcard, on va trouver un ou plusieurs fichiers TOMTOM.xxx, avec xxx allant de 000 à 007, par exemple. Chaque fichier fait au maximum 2 Go. pour obtenir l'image complète du système de fichier, il faut soit concaténer tous les fichiers, ce qui est lent et prend beaucoup de place, soit les associer en un volume RAID linéaire, avec mdadm par exemple :
1. on crée un périphérique de loopback (/dev/loop0 par exemple) pour chaque fichier,
2. on fait un joli mdadm create avec tous les périphériques de loopback, dans le bon ordre,
3. on monte le périphérique de type bloc ainsi créé, et on peut enfin accéder, en lecture et écriture, au contenu.

De là, on peut enfin y placer ses points d'intérêt (les POIs qu'on trouve un peu partout: zones de danger, stations services, magasins, restaurants, etc.). Mais que c'est laborieux ! Donc, j'ai décidé d'automatiser toute la séquence à l'aide d'un script shell, qui va dérouler toutes les étapes, y compris l'ouverture du gestionnaire de fichiers si on est sous X/Wayland. C'est un premier jet, il fonctionne pour moi, et ça m'a permis aussi de découvrir la commande bindfs pour mapper un point de montage et des utilisateurs/groupes vers un utilisateur quelconque pour qu'il puisse lire et écrire sans être root.

Pour ceux qui sont intéressés, c'est ici :
[https://github.com/usawa/tomtom-sdcard-mounter](https://github.com/usawa/tomtom-sdcard-mounter)

Le script est un peu plus complet que ce que j'ai décrit, j'ai tenté de le rendre un peu plus costaud avec quelques contrôles. Il a été testé sous Fedora 32, mais si les dépendances sont installées, il devrait fonctionner partout.

Si j'ai un jour un peu de temps, je pourrai vous expliquer le mécanisme de protection mis en place par tomtom et pourquoi on ne peut pas simplement copier une carte pour en faire un backup, sauf manipulations très particulières.