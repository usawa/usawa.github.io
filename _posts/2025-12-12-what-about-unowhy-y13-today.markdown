---
layout: default
title:  "Retour sur les laptops Unowhy Y13 fournis aux lycéens d'Île-de-France par la région entre 2020 et 2022"
date:   2025-12-12 18:30:00 +0100
description: En septembre 2021, mon fils a reçu un PC portable par la région Île-de-France à son entrée en seconde. Que peut-on faire avec aujourd’hui avec le Unowhy Y13 ? Plein de choses ! Mais pourquoi pas avec les choix de l'époque ? Retour après cinq ans d’expérience, sur un gâchis.
categories: 
- unowhy
---
Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

- [Le Unowhy Y13](#le-unowhy-y13)
  - [Le matériel](#le-matériel)
  - [Système d’exploitation et logiciels](#système-dexploitation-et-logiciels)
  - [Rapidement inutilisable](#rapidement-inutilisable)
  - [La propriété](#la-propriété)
- [Ce qu’il aurait été possible de faire](#ce-quil-aurait-été-possible-de-faire)
  - [Pour tout le monde sans condition ?](#pour-tout-le-monde-sans-condition)
  - [De meilleurs choix, un meilleur système d’exploitation ?](#de-meilleurs-choix-un-meilleur-système-dexploitation)
- [Déverrouillage](#déverrouillage)
  - [Mot de passe du BIOS](#mot-de-passe-du-bios)
  - [Briquage](#briquage)
  - [Mot de passe administrateur Windows](#mot-de-passe-administrateur-windows)
  - [Retirer les bloatwares](#retirer-les-bloatwares)
- [Transformer le Unowhy en machine correcte](#transformer-le-unowhy-en-machine-correcte)
  - [Ajouter du stockage](#ajouter-du-stockage)
  - [Ajouter de la mémoire](#ajouter-de-la-mémoire)
  - [Installer Windows 11](#installer-windows-11)
  - [Installer Linux](#installer-linux)
- [Conclusion](#conclusion)

# Le Unowhy Y13

## Le matériel

La région IDF a choisi sa machine selon un cahier des charges dont le critère principal est le prix : la machine devait coûter maximum 400 euros. Elle a choisi le [Unowhy](https://www.unowhy.com/) Y13. Mon fils a reçu le modèle 2021 :
* Intel Celeron N4120, 4 coeurs 1.1 GHz (burst 2.6 GHz)
* 4 Go de RAM non extensible
* Ecran mate 13 pouces Full HD
* SSD (ahem… eMMC) de 64 Go, soudé
* Wifi, Bluetooth, webcam (basique), micro.
* 1xUSB-C (alimentation, affichage externe, …), 1x USB2, 1xUSB3
* mini HDMI, microSD, capteur d’empreinte
* batterie donnée pour 10 heures

Vous pouvez déjà voir le problème principal, celui qui va faire rapidement très mal. Ce n’est ni le processeur, ni la mémoire vive (RAM).

Le modèle qui nous a été fourni avait, et a toujours, un défaut, une image bleutée. Je n’ai même pas cherché à faire jouer le SAV, nous nous sommes tus. Il est toujours bleu aujourd’hui, mais il fonctionne toujours.

Le laptop est compact, léger, très fin. Il est fourni avec une housse et un chargeur USB-C (qui tombe en panne très vite). Pour un usage bureautique, internet, voir des vidéos, coder un peu, bref pas mal de choses sauf jouer, ce n’est pas si mal. 

Notez qu’il a fallu attendre le modèle 2023 pour avoir 8 Go de RAM et 128 Go de stockage, et 2024 pour disposer d’un modèle gen2, que la RAM soit extensible à 16 Go (SODIMM), que l’ajout d’un SSD soit officiellement supporté, et que la batterie soit facilement remplaçable. Enfin, mais trop tard pour mon fils.

Une dernière note sur le clavier : il n’a pas de rétro-éclairage, dommage. Mais surtout, pour faire l’économie d’un changement de format qui aurait coûté plus cher, bien qu’il s’agisse d’un AZERTY, le format standard n’est pas respecté, les touches > et <, normalement situées à côté de la touche Shift gauche, sont sur les touches X et W, accessibles via la touche Fn. Tellement pratique pour programmer… (Ironique, bien sûr).

## Système d’exploitation et logiciels

Là, ça fait mal. Ceux qui me connaissent savent que je ne suis pas sectaire, et je n'ai rien contre l'OS de Microsoft.

La machine est livrée avec Windows 10 Edition Pro Education (Windows 11 arrive sur les modèles 2023) et plusieurs logiciels pré-installés : la suite Office, un catalogue d’applications, etc. On a accès aux manuels en ligne, à des environnements et bibliothèques de développement (Python par exemple), et tout un tas d’applications pour les différentes matières. Je n’ai rien à redire sur ça.

Vous voyez le problème petit stockage + Windows + agents divers ? **Même pas démarré, l’ensemble occupe déjà 30 Go !**

La machine est liée à un domaine Microsoft, Azure AD / Entra ID (son nouveau nom). A l’allumage, il faut se connecter à un réseau Wifi et un réseau Internet pour la suite. C’est déjà un souci pour pas mal d’élèves (le COVID a montré l’énorme fracture numérique chez les élèves, j'y reviendrai). A la première connexion, l’utilisateur s’enregistre avec les identifiants fournis.

Une fois que tout est démarré, ma foi, ça reste potable. Windows 10 est fait pour tourner sur des machines moins performantes, et les surcouches (des agents sont installés) ne ralentissent pas trop l’ensemble. Mais la suite est moins glorieuse.

## Rapidement inutilisable

Très vite, les problèmes apparaissent. L’élève n’a aucun droit d’administration sur la machine. Sauf logiciels pouvant s’installer dans son profil, il ne peut pas passer outre le catalogue d’applications. C’est une machine captive. Ça peut se comprendre, j'en reparlerai plus loin.

Le BIOS/UEFI est verrouillé par un mot de passe, non fourni. On ne peut donc pas lancer d’OS alternatif ou booter sur une clé USB.

Lors de la présentation du Y13, le patron de Unowhy avait dit :  *« Sur les 30 Go restants, les élèves peuvent stocker leurs devoirs et c’est déjà pas mal », justifie Jean-Yves Hepp. « Si ce n’est pas assez, ils peuvent brancher une carte SD et avoir plus de place »*.

[Source](https://www.numerama.com/politique/656218-les-ordinateurs-offerts-aux-lyceens-dile-de-france-sont-ils-vraiment-inutiles.html)

Comme quoi, quand on y connaît rien, même quand on est patron de la boite qui fournit la machine, parfois on ferait mieux de se taire. Sur la carte microSD, bon, pourquoi pas, pour y mettre des documents (et pourquoi pas une clé USB). Mais ça coince ailleurs.

Les 30 Go effectifs de libre sont vites remplis par les manuels, applications, et mises à jour. Ainsi, il est arrivé qu’il soit impossible d’installer un nouveau logiciel demandé par le professeur : même sans installation par les utilisateurs, **l’espace libre fond tout seul** : tout simplement par les mises à jour de Windows. Même en désinstallant des applications, il reste des artefacts (notamment des fichiers temporaires), que les droits de l’utilisateur ne permettent pas de purger. Aucune procédure n’a été prévue pour nettoyer tout ça. 

**Après quelques mois, l’espace est rempli et il ne reste même plus de place ni pour travailler, ni pour mettre à jour Windows ou les agents. Le système devient quasiment inutilisable, et sans correctifs de sécurité.**

Le seul moyen est alors de demander une réinitialisation complète du système : réinstaller le système d’exploitation et redémarrer de zéro. Cette manipulation n’est pas possible par l’utilisateur. Il faut passer par le SAV. Je ne sais pas qui a essayé, pas moi.

Contribuables, c’est vous qui avez payé ! 180 millions d’euros sur deux ans !

## La propriété

Le laptop appartient à la région, et le lycéen est supposé la rendre s’il ne poursuit pas ses études au moins un an après le bac. Ce qui explique l'absence des droits d'administration, du bios verrouillé, de la machine captive. Il devient donc la propriété de l’ancien élève. Unowhy télédistribue alors deux payloads. L’un donne les droits administrateurs, et le second le moyen de déverrouiller le BIOS. Sauf que Unowhy a arrêté l’envoi des fichiers pour déverrouiller le BIOS, voire l’a même retiré des machines où il était déjà distribué… 

# Ce qu’il aurait été possible de faire

## Pour tout le monde sans condition ?

Devait-on, doit-on fournir un laptop aux lycéens ? C’est une question valable et politique. En 2020, 45 % des collégiens et lycéens avaient leur propre ordinateur (mais le pourcentage diffère selon l’origine sociale). C’est à la fois beaucoup et peu. Le COVID a montré l’étendue de la fracture numérique chez les jeunes et certaines classes sociales. En tant que parent d’élève élu au collège à l’époque, je l’ai vécu de l’intérieur. Dans certaines familles, le seul accès au numérique était un smartphone ou une tablette, voire … rien du tout, ni même d’accès Internet. Tout le monde ne peut pas payer, c’est aussi simple que ça. Et tout le monde ne sait pas l'utiliser. Le collège et les associations ont pu prêter des ordinateurs dans les cas les plus critiques. La question pourrait alors être « Doit-on fournir un laptop à tous les lycéens ? », la réponse serait alors peut-être pas, et on parlerait alors d’injustice ou d’inégalité, ou d’iniquité.

*Les parents de collégiens et lycéens ayant répondu à l’enquête sont 83 % à déclarer que leur enfant possédait au moment de la période de confinement son propre téléphone, 45 % à déclarer que leur enfant avait son propre ordinateur et 24 % à déclarer qu’il avait sa propre tablette (Document de travail 2020-E03)3. La possession par les élèves d’outils numériques personnels semble toutefois différer selon l’origine sociale des parents et leur établissement de scolarisation. Par exemple, on observe que 34 % des collégiens scolarisés dans un établissement privé ont leur propre ordinateur, contre 26 % pour ceux scolarisés en éducation prioritaire (Document de travail 2020- E06).*

[Source](https://www.education.gouv.fr/media/95365/download)

Peut-être aussi aurait-on pu fournir une formation aux élèves et aux étudiants ? Nommer des référents ? Parce que (mais peut-être que je me trompe), par chez nous, rien. On nous a fourni les laptops puis chacun a dû se débrouiller.

## De meilleurs choix, un meilleur système d’exploitation ?

Restons plutôt du côté technique, notamment sur les choix de système d’exploitation.
Livrer les machines avec plus d’espace disque ? Réponse facile ! Oui, mais ce n’était pas obligatoire.

**Remplacer Windows par Linux, oui !** Il n’y avait aucune raison technique, logicielle ou humaine de ne pas le faire. Le matériel est compatible, l’offre logicielle  était là. Côté « captif », l’utilisation de Azure AD était (est toujours) parfaitement possible (sssd-ad), l’application de règles de sécurité aussi (GPO via sssd-ad et samba, utilisation de règles distribuées pour Polkit, etc.), tout comme les mises à jour. Tout aurait très bien fonctionné avec Ubuntu, que ce soit avec Gnome ou KDE.

![Linux Mint sur le Y13](/assets/img/2025-12-12/y13_linux.jpg)

L’offre logicielle était (et est toujours là). Comme par exemple l’accès aux manuels scolaires via [lelivrescolaire.fr](https://www.lelivrescolaire.fr/applications) : Oh, il y a une version pour Linux ! Ou encore, Sketch, Mu, Dessin, Gimp, LibreOffice… On m’a dit à l’époque *« oui mais s’ils ont déjà un ordinateur avec Windows au moins ils seront habitués »*. Déjà, s’il y a déjà un ordinateur, ça revient à la discussion précédente. Ensuite, Rendre captif les jeunes générations à un système propriétaire ? C'était l'occasion de montrer une alternative viable.

https://lafibre.info/tutoriels-linux/pc-ile-de-france-y13/

# Déverrouillage

Je n’ai pas attendu la fin des trois ans pour débrider le Y13 de mon fils. Je l’ai fait moins d’un an après. Les scrupules, je ne les avais plus. Et la machine, j’ai considéré l’avoir payé (taxes et impôts) assez cher pour un machin si mal géré.

## Mot de passe du BIOS

Le graal n’est pas forcément d’obtenir le mot de passe de l’administrateur, mais de celui du BIOS afin de démarrer sur autre chose, ce qui est la base de tout. Les mots de passe du BIOS des premiers modèles ont rapidement fuité sur les forums et divers sites dont le fabuleux [sty1001.com](https://blog.sty1001.com/). 

Pour les modèles suivants, ça a été bien plus compliqué.

Il y a quatre méthodes pour déverrouiller le BIOS, j’en ai testé deux.

1. **Trouver les mots de passe** sur les sites. Ce que j’ai fait, évidemment. Une fois dans le BIOS, on supprime le mot de passe et c’est joué. On démarre en appuyant rapidement sur Echap, et hop.
2. Utiliser **la méthode du court-circuit**. Il faut démonter la machine, court-circuiter l’eMMC pour qu’il ne soit plus détecté, ce qui forcera le boot sur une clé USB contenant les UNOWHY TOOLS pour flasher un nouveau BIOS. Il faut avoir de la patience, de la dextérité et ne pas avoir peur de cramer la machine. [Procédure](https://blog.sty1001.com/2024/07/29/unlock-le-bios-nimporte-quel-y13-gen-1-2023-et-avant-avec-la-methode-du-court-circuit/).
4. **Flasher le BIOS avec les droits d’administration** fournis à la fin du lycée. Si vous avez eu ces droits, si vous n’avez pas raté le coche… [Procédure](https://blog.sty1001.com/2024/01/29/unlock-le-bios-de-nimporte-quel-unowhy-y13-si-vous-etes-admin-methode-ultime/)
5. La méthode ultime, mais la plus flippante : **le CH341A**. On flashe le BIOS via un programmateur d’EEPROM directement branché sur la puce de la carte mère. J’ai dû utiliser cette méthode suite à une manipulation hasardeuse qui a mal tourné, ayant briqué la machine. Pour cette méthode, il faut évidemment le kit CH341A, qu’on trouve pour [moins de 20 euros](https://www.amazon.fr/dp/B09MHH6D8H?ref_=ppx_hzsearch_conn_dt_b_fed_asin_title_1), une autre machine (le programmateur se branche en USB), les bons fichiers, et beaucoup, beaucoup de patience et de dextérité. Il s’agit d’effacer le contenu de l’EEPROM et de flasher un nouveau BIOS.

L’étape la plus critique consiste à faire tenir la pince sur la puce du Unowhy. C’est très petit. J’ai dû m’y reprendre une bonne trentaine de fois, tout en ayant failli griller la puce car j’avais oublié l’adaptateur 1.8V. Durant l’opération j’ai accidentellement arraché un composant minuscule d’un millimètre de long et eu la grande frayeur d’avoir flingué définitivement la machine. En fait, ça n’a eu, et n’a encore aujourd’hui aucune conséquence (mais à quoi ça pouvait bien servir?). [Procédure et fichiers](https://blog.sty1001.com/2024/02/12/unlock-le-bios-du-unowhy-y13-avec-le-ch341a/)

Vous trouverez dans les commentaires mes sincères remerciements à l’auteur.

![Le CH341A qui a sauvé mon Y13](/assets/img/2025-12-12/ch341a.jpg)

Oh, n’espérez pas obtenir des mises à jour de sécurité de l’UEFI. Déjà nos méthodes ne sont pas très recommandables, alors imaginez si Unowhy publiait ses mises à jour...

## Briquage

Un mot rapide sur la capacité de cette machine à être briquée (c’est à dire à se transformer en brique, inutilisable, cassée) : elle est importante. Il y a une légion de commentaires un peu partout sur la machine qui ne se réveille plus (bien que des voyants s’allument). Les pannes, ça arrive…

Par exemple, certains ont pensé que c’était une bonne idée d’ajouter le disque SSD sans avoir accès au setup de la machine, ou même en oubliant de l’éteindre. Mais parfois la machine ne se réveillait plus. Terminé, HS.

Un moyen radical de briquer le laptop est, une fois le BIOS accessible, d’aller régler le type d’OS sur Linux. Ça m’a semblé une bonne idée, vu que j’installais un Linux dessus. **Erreur monumentale !** Machine briquée, passage au flash via le programmateur d’EEPROM (unique raison de mon achat), qui a sauvé cette brave machine.

## Mot de passe administrateur Windows

Après avoir déverrouillé le BIOS, on peut enfin faire ce qu’on veut. Si on souhaite continuer à utiliser Windows, on peut effacer le mot de passe du compte Administrateur. Notez que ça fonctionne pour n’importe quel PC sous Windows si on peut booter sur un support externe. Je suis passé par [Rescue CD](https://www.system-rescue.org/).

1. Bootez sur la clé USB : appuyez plusieurs fois fur Echap pour ensuite changer l’ordre de démarrage sur la clé USB.
2. Choisir : Boot System Rescue using default options ⏎
3. Passer en français (selon votre pays) : 
   1. setkmap ⏎
   2. choisir : fr-latin9 ⏎
4. Trouver la bonne partition :
   1. Taper : gdisk -l /dev/mmcblk0 ⏎
   2. Dans la liste, trouver le numéro de la partition la plus grosse
5. Monter la partition Windows, taper : ```ntfs-3g -o remove_hiberfile /dev/mmcblk0pX /mnt```  ⏎ (X le numéro trouvé)
6. Se déplacer au bon endroit, taper ```cd /mnt/Windows/System32/config``` ⏎
7. Reset du mot de passe :
   1. Taper: ```chntpw -i SAM``` ⏎
   2. Choisir ```1 : Edit user data and Passwords```
   3. Trouver la valeur hexa de 4 caractères qui correspond à l’utilisateur, et la saisir ⏎
   4. Choisir ```1 – Clear (blank) user password```  ⏎  ça devrait afficher Password cleared)
   5. Choisir ```2 – Unlock and Enable User Account``` ⏎
   6. Sortir avec q ⏎
   7. Sortir encore avec q ⏎
   8. Il va afficher : ```Hives that have changed : # Name 0 <SAM> write hive files ?``` Taper y ⏎
   9. Il devrait afficher, en sortant du programme : ```0 <SAM> - OK```
8.  Taper ```cd``` ⏎
9.  Taper ```reboot``` ⏎

Et voilà, on redémarre normalement sous Windows, l’utilisateur choisi n’a plus de mot de passe. On peut retirer la clé.

## Retirer les bloatwares

Si vous pensez rester sous Windows, pour pouvez maintenant désinstaller tout ce que vous voulez, mais ce n’est pas suffisant. De nombreux agents sont présents, et la machine est rattachée à un domaine (et donc les GPO associées). L’outil [Unowhy-tools](https://github.com/STY1001/Unowhy-Tools
) permet de dégager l’ensemble des outils et paramètres pour rétablir le fonctionnement d’origine de Windows. J’ai obtenu de très bons résultats, retrouvant une installation presque propre.

Une fois tout effectué, on retrouve une machine propre, mais il reste encore le souci d’espace disque, voire le système d'exploitation.

# Transformer le Unowhy en machine correcte

Dans tous les cas, il faut déverrouiller le BIOS, et avoir les droits nécessaires ensuite pour formater ou partitionner le disque.

## Ajouter du stockage

Si le problème du stockage ne se pose pas pour les modèles de 2023 qui ont 8 Go de RAM et 128 Go d'espace disque, sur les modèles 2020-2022 c’est un enfer. Hors, la machine dispose d’un port d’extension au format M.2. auquel on accède très facilement via une trappe à l’arrière du laptop. On peut y placer un petit SSD SATA (et uniquement SATA) de toute taille. C’est donc la première chose à faire.

![Un SSD SATA au format 2280](/assets/img/2025-12-12/ssd_sata.jpg)

Le coût du stockage a explosé en cette fin 2025 (merci l’IA) mais on trouve encore des modèles 256 Go à 25 euros, neuf (Verbatim) qui suffisent largement. J’ai pour ma part opté pour un modèle 512 Go, bien plus que suffisant pour l’usage auquel je dédie mon Unowhy (enfin, celui de mon fils…).

S’il s’agit d’y mettre un Linux, alors les 64 Go peuvent suffire. Sur Linux Mint, ce que j'ai installé , pensez à désactiver Timeshift. En Privilégiez quand c’est possible les packages natifs plutôt que du Flatpak, Snap ou AppImage.

## Ajouter de la mémoire

A moins de jouer du fer à souder et d’avoir les compétences, **on ne peut pas rajouter de RAM sur les modèles de première génération**. Est-ce un souci pour l’usage prévu pour le Unowhy ? Non. Les 4 Go suffisent tant pour sous Windows que sous Linux. Sous Linux, c’est même l’abondance !

## Installer Windows 11

C’est possible, la machine étant équipée d’un module TPM 2.0. Mais la case SSD est obligatoire. Cependant, pensez plutôt à Linux.

[TPM sous Linux](https://wiki.archlinux.org/title/Trusted_Platform_Module)

```bash
$ cat /sys/class/tpm/tpm0/device/description
TPM 2.0 Device
```

## Installer Linux

Quelle bonne idée ! La meilleure ! D’autant plus que le matériel est compatible, à l’exception du lecteur d’empreinte (et on s’en fiche). La plateforme Intel est standard, le chip Wifi est un Intel AC-3165 (Wifi 5), le GPU un UHD Intel Graphics 600, etc. 

J’ai pu tester différentes distributions depuis des années : Fedora, Ubuntu, Linux Mint, KDE Neon, … Tout passe, tout fonctionne. Et même très bien. Mon choix final s’est arrêté sur [Linux Mint](https://linuxmint.com/), simple, rapide, réactif, compatible Ubuntu. Je ne rencontre aucun souci avec. La veille fonctionne bien, la batterie tient longtemps, et elle a supporté tant de mes tests (comme l’utilisation en second écran), le bonheur ! Si on a uniquement les 64 Go de l’eMMC, on pensera à désactiver les fonctions de type snapshot, ou timeshift.

# Conclusion

La livraison d’une machine aux lycéens avec si peu de stockage qu'elle en devient inutilisable en quelques mois juste en suivant les mises à jour  est clairement scandaleuse. Le lycée, c'est trois ans. Qu'espéraient les gens ayant fait ces choix ? Quel est l’andouille qui a pensé que 64 Go de stockage pouvait suffire quand le système prend déjà 30 Go d’office ? Bill Gates ? (vous avez la ref?) D’autant plus qu’au même moment, d’autres régions fournissaient des machines bien plus performantes, notamment côté stockage et ram ! De vrais modèles HP par exemple. Incroyable.

Le bridage, qui reste compréhensible dans certaines limites pour une machine appartenant à la région et pour une utilisation donnée, rend certaines tâches élémentaires impossibles, y compris le nettoyage nécessaire aux mises à jour, qui deviennent impossibles. On peut supposer que nombre de personnes, une fois le lycée terminé, n’ont même pas cherché à débrider leur Y13, même lorsque ça leur a été proposé.

J’imagine le gâchis : combien de machines sont parties à la casse sans être utilisées ? Combien traînent encore, bridées, dans des placards, tiroirs ? La région a mis 3 ans à rectifier le tir. C’était pourtant si simple pour un coût si ridicule ! Quel gâchis.

Combien de personnes savent qu’il est possible de débrider ces machines pour grandement les améliorer ? Qui d’ailleurs a les compétences nécessaires pour ça ? Un camarade de mon fils lui a dit qu’il avait de la chance d’avoir un père qui savait faire tout ça…

Mais une fois débridée, et linux installé, la machine est tout à fait correcte pour un usage standard quotidien. N’hésitez pas à les faire ressortir du placard et aider les gens à en faire un vrai ordinateur utilisable.