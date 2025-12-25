---
layout: default
title:  "Retour d'expérience, Ubuntu 24.04 sur un Macbook Pro 2015"
date:   2024-07-14 00:00 +0100
description: "Pour sauver un Macbook Pro de 2015 de l'obsolescence programmée, j'ai tenté d'installer une Ubuntu 24.04 dessus"
categories: 
- macbook
---
Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

Publié aussi sur LinuxFR.

- [Retour sur Linux](#retour-sur-linux)
- [Obsolescence programmée et linux](#obsolescence-programmée-et-linux)
- [Soucis liés au support matériel](#soucis-liés-au-support-matériel)
  - [La mise en veille](#la-mise-en-veille)
  - [les touches @/# et \<\> inversées](#les-touches--et--inversées)
  - [La touche cmd (ou menu Pomme)](#la-touche-cmd-ou-menu-pomme)
  - [La webcam](#la-webcam)
  - [Le zoom via le trackpad](#le-zoom-via-le-trackpad)
  - [Le curseur qui saute tout seul n'importe où](#le-curseur-qui-saute-tout-seul-nimporte-où)
  - [L'écran Retina](#lécran-retina)
  - [Le problème qui persiste, le bluetooth](#le-problème-qui-persiste-le-bluetooth)
- [Les soucis systèmes ou logiciels](#les-soucis-systèmes-ou-logiciels)
  - [Ubuntu, pour les débutant, vraiment ?](#ubuntu-pour-les-débutant-vraiment-)
  - [Google Drive](#google-drive)
  - [L'imprimante et le scanner](#limprimante-et-le-scanner)
  - [Applications ou fonctionnalités manquantes](#applications-ou-fonctionnalités-manquantes)
  - [Crashs](#crashs)
- [Bilan](#bilan)

# Retour sur Linux

J'avais arrêté d'utiliser quotidiennement un environnement bureautique sous Linux depuis 2013. Une fois par an je recréais une ou deux machines virtuelles pour tester les évolutions. Mais, pour être clair, entre les limitations de Gnome et les errements de KDE 4 et 5 (regrets éternels de KDE 3), j'étais passé sous MacOS (Macbook du travail et Hackintosh). Puis, en 2020, je suis revenu sous Windows, initialement pour jouer (bien que proton soit incroyable) puis j'y suis resté. J'avais à un moment donné un triple boot sur mon PC principal. Pas simple à gérer lors des mises à jours.

# Obsolescence programmée et linux

J'ai un Macbook Pro de 2015. Le support MacOS se termine cette année, en septembre. C'est, tout comme Windows 10, de l'obsolescence programmée. La preuve, avec Legacy Opencore Patcher les versions récentes de MacOS fonctionnent (mais avec des limitations et des soucis réguliers aux reboots - genre je dois rebooter trois fois avant d'arriver à ouvrir une session). Le mac vieillit mais tourne encore bien. Mais le support... Alors je me suis dit, Pourquoi ne pas tenter Linux ? J'ai pris le standard de base, Ubuntu 24.04, et j'ai installé. Et ça s'installe bien, et ça semble, dans les premiers instants, bien fonctionner. Et puis, passée la surprise (ouah ça marche direct), bah, non, pas sis bien que ça. Voici les divers soucis, soit liés au support matériel, soit liés à Ubuntu, soit liés aux applications (et moi) que j'ai rencontrés.

# Soucis liés au support matériel

## La mise en veille

Ou plutôt, le réveil ! Il met 20 secondes pour sortir de veille (deep). Il faut passer en s2idle (au prix d'une consommation batterie supplémentaire)

```bash
$ cat /sys/power/mem_sleep
[s2idle] deep
```

Pour le rendre persistant au boot ou modifie /etc/default/grub :

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash mem_sleep_default=s2idle"
```

## les touches @/# et <> inversées 

Alors là j'ai du mal à comprendre mais en 10 ans et malgré de nombreux rapports chez les éditeurs, les touches sont inversées. Il a fallu inverser deux touches dans la conf X en créant et modifiant des fichiers à la main. [C'est ici]( https://doc.ubuntu-fr.org/clavier_apple_usb_ultra_plat)


## La touche cmd (ou menu Pomme)

Il y en a qui vont hurler, mais je préfère 1000 fois le layout du clavier Apple à celui d'un clavier PC classique. Et notamment la touche CMD (ou Pomme si vous voulez). Surtout sous Unix/Linux. CMD va remplacer CTRL par exemple, pour les copier-collers. Et dans une console, c'est juste génial de ne pas mélanger un Ctrl+C (stopper) et un Copier. Pour ça j'ai trouvé [Toshy](https://github.com/RedBearAK/toshy). J'ai dû modifier un fichier de mapping pour l'adapter à un clavier Azerty (parce que CMD-A me faisait un CMD-Q), installer une extension, et hop. C'est juste incroyable. Ca remplace Kinto, qui ne semble pas fonctionner sous Wayland.

## La webcam

Bon c'est un peu le bazar, mais ça fonctionne... Selon les applications. Il faut se débrouiller sous Ubuntu (et là, on m'apprend que sous Fedora c'est tout installé par défaut...). Il faut installer le module facetimehd de Patrick Jakobson, basé sur de la rétro-ingénierie. Mais il faudrait surtout que je trouve le moyen plus simple, via kmod ou dkms, qu'il soit reconstruit à chaque mise à jour.

[facetimehd](https://github.com/patjak/facetimehd/wiki/Installation#get-started-on-ubuntu)

## Le zoom via le trackpad

Alors, c'est impeccable sous Gnome en général, mais non fonctionnel sous Chrome. Vous êtes sous Maps, ou autre, et ça marche pas, ou très mal. Il faut modifier les réglages de chrome car il n'a pas détecté Wayland, ce qu'il devrait pourtant faire. On force donc Wayland sur 

`chrome://flags/#ozone-platform-hint`

##  Le curseur qui saute tout seul n'importe où

Alors là j'ai cherché parce que c'était vraiment casse-pieds. Je tape du texte et d'un coup mon curseur saute ailleurs. C'est comme si quelque chose cliquait involontairement ailleurs. C'est assez surprenant et c'est aléatoire, mais plusieurs fois par heure. J'ai pensé que c'était Toshy mais... Même en le désactivant, ou en le désinstallant le souci persistait. Alors j'ai pensé à un souci de sensibilité du trackpad, et au "tap to click", j'ai désactivé, et depuis c'est bon. C'est tellement sensible que le simple fait de passer le pouce ou la paume sur le trackpad activait un click. Je n'avais pas ce souci sous MacOS (où le tap to click est activé). On doit pouvoir régler la sensibilité quelque part...

## L'écran Retina

Par défaut, on obtient un zoom à 200% et l'impression d'être en 1024x768. Perturbant. Dans les paramètres de Gnome, si on choisit 100% c'est trop petit. Alors on active "Fractional Scaling", et on passe à 150 ou 175% et on retrouve son affichage comme sous MacOS.

## Le problème qui persiste, le bluetooth

Le bluetooth ne fonctionne parfois plus en sortie de veille. Je dois faire un 

```bash 
rfkill block 0
rfkill unblock 0
```

pour que ça fonctionne. 

# Les soucis systèmes ou logiciels

## Ubuntu, pour les débutant, vraiment ?

Dès le début, on ne peut pas se passer de la console. Déjà, impossible de mettre à jour, on se tape une erreur comme quoi "snap blablabla". Recherche Google, et passage par la console. Ensuite, pleins de trucs qui devraient être installés par défaut, ne le sont pas. Par exemple, l'installateur de packages, le gestionnaire d'extensions Gnome, l'outil d'ajustement Gnome (tweak), ... Bref, on passe sa vie entre les recherches sur Internet et le Centre d'applications, qui d'ailleurs se fait un malin palisir de ne jamais fournir le bon truc. Rageant. Je plains les débutants.

## Google Drive

gvfs semblait faire le job, et ... Non. Outre que sur la moitié des applications il n'y a pas de translation gvfs (gio) vers des noms de fichiers cohérents (mais pourquoi ce choix ?), il faut utiliser la commande gio en console pour s'en sortir. Pire, je me suis retrouvé, en voulant sauver un fichier keepass via keepassxc (pas de panique, la clé est ailleurs et le MFA est activé), avec des centaines (vraiment) de popups d'erreurs, ça m'a créé des centaines de fichiers cachés das mon drive. Pour m'en sortir j'ai dû fermer la session (parce qu'impossible de chopper le focus dans la console pour tuer le processus). J'ai fini par payer une licence inSync. C'est le prix de la tranquilité et de la simplicité. 

[Insync](https://www.insynchq.com/)

##  L'imprimante et le scanner

J'ai une HP Smart Tank 7005 en réseau  Wifi (c'est cool les imprimantes à réservoir). Surprise, c'est détecté. Sauf que... Une nouvelle imprimante est détectée à chaque reboot de la machine, ou à chaque redémarrage de l'imprimante, et donc, le nom change sans arrêt. Quant au scanner, il n'est pas reconnu. Donc, installation de HPLIP, setup via l'icone qui va bien, et, hop, tout fonctionne. Je n'avais pas le souvenir d'avoir eu ça avec mes précédentes imprimantes (Wifi aussi).

## Applications ou fonctionnalités manquantes

Déjà, l'installation par défaut d'Ubuntu... Mais, purée, installez par défaut les extensions Gnome !

Je vais faire râler mais deux choses me manquent. La première c'est un vrai client Whatsapp qui supporte les appels audio et vidéo. Qu'on ne vienne pas m'expliquer que c'est pas libre et qu'il existe des alternatives : je m'adapte au reste des amis et de la famille, et ce n'est pas à eux de s'adapter à moi. Point.

La seconde, c'est... La suite Office 365. Oui. Là aussi inutile de m'embêter avec LibreOffice, parce que là encore, c'est moi qui m'adapte. Y compris à moi-même. Notamment Office 365 pour le travail collaboratif (vous savez, quand on érit un bouquin à deux, c'est sympa d'être à deux sur le même document en même temps). J'ai essayé d'installer WPS Office, mais ce truc plante. Alors on va rester sous LibreOffice, mais j'avoue que la tentation est grande de tester une installation Wine.

## Crashs

J'ai parfois quelques crashs à la réouverture d'une session, des divers outils en lancement automatique (autostart). En fait, je trouve que Ubuntu crashe pas mal. C'est assez surprenant. Jamais le kernel, non, mais des composants applicatifs Gnome, ou Wayland, avec une demande d'envoi de rapport... Je me demande s'il ne faudrait pas basculer sur X. Des fois je me retrouve avec la moitié de l'écran en surimpression orange, comme si j'en avais sélectionné la moitié. Des fois je perds le dock, ou les icônes du dock. 

# Bilan

On obtient quelque chose qui fonctionne dans l'ensemble, mais que c'est laborieux ! Dès le début, on se prend une erreur de mise à jour, une histoire de snap. Et ensuite, les outils pas installés par défaut, et très vite on se retrouve à devoir ouvrir une console. Pour l'expérience utilisateur "débutant", on repassera. Pour le reste, je conçois très bien qu'il ne doit pas être courant d'installer Ubuntu, ou même Linux, sur un Macbook. Et donc, j'accepte de devoir adapter mon installation à du matériel pouvant être considéré comme exotique ou propriétaire (notamment la webcam). Les touches inversées par contre, depuis le temps, vraiment...

Mais alors, je reste ? Eh bien, oui, c'est possible que je reste sous Linux. Remettre MacOS ? Oui, on peut avec Legacy OpenCore Patcher, mais les performances sont réduites et il y a des soucis, parfois. Passer sous Windows ? Le 10 c'est fini dans un an, et le 11, bah faut bidouiller, et j'aurai encore ce souci de layout de clavier. Le seul vrai moyen de continuer à avoir un support OS et applicatif, c'est Linux. Mais je tenterais bien une Fedora (ne serait-ce que pour les packages Webcam). Cependant, avec Ubuntu j'arrive à la fin à quelque chose de bien, et ça m'a vraiment étonné.
