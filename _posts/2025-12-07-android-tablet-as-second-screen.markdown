---
layout: default
title:  "Monter un serveur Webdav pour partager ses fichiers multimédias"
date:   2025-12-07 17:30:00 +0100
description: Si les lecteurs multimédias comme Kodi ou VLC peuvent accéder à des partages SMB (Windows), certains boitiers souffrent de lenteur. Voici comment mettre en place un partage Webdav, utilisant le protocole HTTP(S). Et se tirer les cheveux (s'il en reste) avec Kodi.
categories: 
- webdav
---
Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}


# Les solutions spécifiques à Windows

## Spacedesk

Spacedesk est la solution gratuite pour Windows la plus simple à mettre en œuvre et aux performances les plus rapides:
* Un pilote à installer sous Windows
* Un client à installer sur Android, iOS, Windows ou MacOS

La connexion se fait soit via réseau, soit directement en USB. En IP, j'ai pu constater un débit de 50 Mbit/s pour du 1080p, avec une faible latence. En USB, c'est encore plus rapide, la latence est presque inexistante.

En USB, j'ai eu du mal au début jusqu'à ce que je me rappelle que j'avais activé le débogage USB sur la tablette. En le stoppant, tout s'est mis à fonctionner.

Dans les deux cas, il y a une légère consommation CPU, assez pour parfois entendre les ventilateurs démarrer, à noter que cette consommation est supérieure en passant par le réseau.

Spacedesk n'est pas qu'une simple solution de partage d'écran: les entrées et sorties fonctionnent, on peut utiliser l'écran tactile, le stylet, le clavier virtuel, le son, le micro du client. On peut donc par exemple utiliser son logiciel de dessin ou de retouche favoris pour travailler avec son stylet, comme une tablette graphique.

## Superdisplay

Superdisplay est encore plus simple que Spacedesk. On installe le pilote côté Windows, le client côté Android, on branche le câble USB, et ça fonctionne tout seul. Il est aussi possible de passer par le réseau. Superdisplay est en fait un driver de tablette graphique et agit comme tel. Vu comme un écran ordinaire, il peut être étendu ou cloné, et les stylets fonctionnent. Quant à la latence en USB ? Aucune. C'est aussi simple que ça.

Problèmes techniques ? Aucun. C'est simple, rapide et efficace. Superdisplay utilise l'accélération matérielle, et un réglage adaptatif de la qualité, qui peut être optimisé en cas de besoin. C'est vraiment bluffant. Mais où est le loup ? Et bien, c'est payant. Après trois jours, vous devrez vous acquitter de 16,99€ pour continuer à l'utiliser. 

Et, comme indiqué, ce n'est que pour Windows et Android. Si vous avez 16,99€ à dépenser, c'est la solution la plus performante.


## Solutions 
# Emulateur d'affichage virtuel

Deskcreen ne pourra que dupliquer un écran existant, ou une fenêtre. Pour afficher un second écran, il faut faire croire à l'ordinateur que vous en avez un autre, et le partager à l'aide des outils présentés ici. Le plus simple est un petit adaptateur qui émule un écran externe.

J'ai testé avec deux adaptateurs. 

Le [premier en USB-C](https://www.amazon.fr/dp/B088H367TF?smid=A12KOLKL8Y0ERS&ref_=chk_typ_imgToDp&th=1) est destiné aux machines supportant l'affichage via port USB-C, ce qui est le cas de beaucoup de PC portables laptops, mais qui n'est pas le cas de mon PC fixe. Sur mes laptops, le branchement fait immédiatement apparaitre un second écran 1080p dans les périphériques et réglages, tant sous Linux que Windows.

Le second est un [adaptateur HDMI](https://www.amazon.fr/dp/B09DKGJ2NF?ref=ppx_yo2ov_dt_b_fed_asin_title) à connecter sur la sortie idoine du laptop ou de la carte graphique. Il a fonctionné directement sur l'ensemble de mon matériel, me proposant des résolutions virtuelles jusqu'au 4K.

Vous réglerez la résolution virtuelle à ce qui correspondra à vos besoins et à l'appareil qui affichera votre second écran.



Pilote d'affichage virtuel Windows

Microsoft a introduit la notion de « Indirect Display Driver » dans son SDK, qui permet de se passer d'émulateur d'affichage USB. 

https://github.com/ge9/IddSampleDriver/releases

L'installation est décrite sur la page précédente et ici:
https://github.com/roshkins/IddSampleDriver/releases

Si vous souhaitez désactiver la carte virtuelle après installation, dans le gestionnaire de périphériques, Cartes graphiques, bouton droit sur « IddSampleDriver Device » puis « Désactiver l'appareil » (ou l'activer, le cas échéant).

Affichage virtuel sous Linux

Si vous êtes encore sous X-Window (Xorg), la commande xrandr permet d'ajouter un écran virtuel, ou en tout cas de faire croire qu'il existe. D'abord, tapez xrand:

```bash
seb@Y13:~$ xrand
eDP-1 connected primary 1920x1080+1920+0 (normal left inverted right x axis y axis) 294mm x 165mm
   1920x1080     60.00*+  60.00
   1680x1050     60.00
   1400x1050     60.00
...
HDMI-1 disconnected 1920x1080+0+0 (normal left inverted right x axis y axis) 0mm x 0mm
DP-1 disconnected (normal left inverted right x axis y axis)
HDMI-2 disconnected (normal left inverted right x axis y axis)
```

Le moniteur de mon laptop s'appelle eDP-1. On va l'étendre sur HDMI-1, en le mettant à gauche:
```bash
seb@Y13:~$ xrandr --addmode HDMI-1 1920x1080
seb@Y13:~$ xrandr --output HDMI-1 --mode 1920x1080 --left-of eDP1
```

L'écran principal peut se mettre à clignoter. Mais il sera étendu à gauche de votre affichage actuel. Pour l'arrêter:

```bash
seb@Y13:~$ xrandr --output HDMI-1 --off
```

Le défaut c'est que l'affichage reste vu comme déconnecté, notamment par les paramètres d'affichage du bureau. Pour éviter ça une modification de la configuration de Xorg sera nécessaire.

Belle vidéo explicative sur [Youtube](https://www.youtube.com/watch?v=ftXv1Uo0daA).
Un discussion sur ce problème sur [Reddit](https://www.reddit.com/r/ManjaroLinux/comments/sgkokh/xrandr_showing_hdmi_output_as_disconnected_when/)
Et comme toujours pour Arch, un excellente [documentation](https://wiki.archlinux.org/title/Multihead#RandR)



Pour Wayland, j n'ai pas trouvé de solution car il n'y a pas qu'un seul compositeur, notamment celui de Gnome qui n'accepte rien (une philosophie de Gnome: « *si vous ne comprenez pas nos choix, c'est que vous êtes trop stupide pour nous* ») et il ne semble pas d'y avoir d'outils communs. Utilisez une petite clé de simulation USB.



Deskreen

Compatible Linux, Windows, MacOS.

https://deskreen.com

Attention la version 3.1.13 est cassée. J'ai testé avec la version 3.1.11.

https://github.com/pavlobu/deskreen/releases


Licence libre GPL Affero, qui oblige les services utilisateurs (les programmes qui consomment Deskreen) à être eux-mêmes libres.

La version Windows (3.1.13) n'a jamais voulu fonctionner sous Windows. J'ai laissé un rapport de bug sur le repo github des auteurs. J'ai testé la version antérieure 3.1.11.

Sous Linux Mint:
AppImage.

```bash
seb@Y13:~/Téléchargements$ chmod +x AppImage...
seb@Y13:~/Téléchargements$ ./AppImage
```

Deskreen affiche un QR Code ou une URL à saisir sur l'autre machine, puis demande quel écran ou application à partager. Pour étendre l'affichage, il faut un adaptateur d'écran virtuel, comme le modèle USB qui simule un second écran.

Ca fonctionne bien ! Mais avec une latence importante 

Sous Windows, Deskreen fonctionne exactement de la même manière. Aucune différence.

Weylus

https://github.com/H-M-H/Weylus?tab=readme-ov-file#linux



VNC, RDP and co

Solution surprenante qui ne fonctionne évidemment qu'à moitié et pour une bonne raison. On peut lancer un serveur VNC ou RDP sur son bureau Windows ou Linux, puis y accéder via le client adapté.

Notamment, sous Linux, on peut utiliser Vino comme serveur VNC. En déclarant un écran virtuel avec xrandr, j'ai pu l'afficher avec un client VNC. Mais c'est une très mauvaise idée : l'écran du PC qui partage est sous le contrôle du clavier et de la souris du PC, tandis que la partie affichée par le client est sous le contrôle du client !

Autre souci, Vino ne semble prendre que le premier affichage, de gauche à droite. Donc, il faut que l'écran virtuel xrandr soit à gauche de votre écran principal, et que donc l'écran additionnel, lui, soit à droite !

Cette solution est donc à conserver uniquement si vous souhaitez afficher un miroir de votre écran.

```bash
seb@Y13:~$ sudo ufw allow 5900
seb@Y13:~$ sudo apt install vino
seb@Y13:~$ gsettings set org.gnome.vino require-encryption false
seb@Y13:~$ /usr/lib/vino/vino-server
```


