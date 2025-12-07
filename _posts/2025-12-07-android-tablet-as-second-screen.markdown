---
layout: default
title:  "Utilisez une tablette ou tout autre ordinateur comme second écran"
date:   2025-12-07 12:30:00 +0100
description: Vous avez une tablette Android ou iOS, un ordinateur sous Linix, Windows, MacOS ? Même un vieux modèle ? Ne jetez pas ! Pourquoi ne pas les utiliser comme second écran ?
categories: 
- ecran
---
Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

- [Besoin d'un second écran ?](#besoin-dun-second-écran-)
- [Emuler un écran](#emuler-un-écran)
  - [Pilote dédié à l'application](#pilote-dédié-à-lapplication)
  - [Emulateur d'affichage virtuel](#emulateur-daffichage-virtuel)
  - [Pilote d'affichage virtuel Windows](#pilote-daffichage-virtuel-windows)
  - [Affichage virtuel sous Linux](#affichage-virtuel-sous-linux)
- [Les solutions spécifiques à Windows](#les-solutions-spécifiques-à-windows)
  - [Spacedesk](#spacedesk)
  - [Superdisplay](#superdisplay)
- [Les solutions sous Linux](#les-solutions-sous-linux)
  - [Deskreen CE](#deskreen-ce)
  - [Weylus](#weylus)
  - [VNC, RDP and co](#vnc-rdp-and-co)
  - [Xpra](#xpra)
  - [Petit coup de gueule](#petit-coup-de-gueule)
- [Conclusion](#conclusion)

Il y a quelques jours, je posais une question sur la tribune de LinuxFR, et Bubar, sans répondre à ma question directement, s'est lancé dans l'aventure du streaming de jeux sur ses machines, et en a fait un journal. J'ai dit que j'écrirais un journal en réponse, avec des solutions plus simples pour les utilisateurs. Dont acte.

# Besoin d'un second écran ?

J'ai plusieurs machines, dont un PC fixe (sous Windows 11, mais avec WSL) raccordé à un écran 24 pouces Full HD accroché au mur. Mon espace de bureau personnel est relativement petit (1m30 de large). J'ai parfois hésité à passer à du 27 pouces 1440p, car j'aimerais avoir plus d'espace pour les fenêtres (par exemple VSCode d'un côté, le navigateur de l'autre, etc.) Mais c'est massif ! J'ai ensuite pensé à un second écran 15.6 pouces à mettre sur le côté et étendre mon affichage. Sauf que l'espace qu'il occuperait est déjà occupé par l'ordinateur portable de mon travail - quand je suis en télétravail, (j'ai un switch KVM qui me permet de partager écran, clavier et souris) et j'ai déjà un laptop et une tablette, alors est-ce qu'on pourrait les utiliser comme écran ?

J'ai donc cherché, et trouvé, comment utiliser l'écran d'un ordinateur portable ou une tablette, comme écran secondaire. C'est en fait assez simple, et la solution pour mon usage. Evidemment il y a quelques contraintes, notamment parfois de latence, et force est de constater que les solutions proposées sous Windows sont plus rapides que celles sous Linux. Mais on a aussi un avantage avec les solutions sous Linux : elles sont généralement compatibles avec tout car basés sur une diffusion via HTML5 ! 

Les possibilités ne s'arrêtent pas à la duplication ou l'extension de l'affichage sur un seul écran : on peut multiplier à volonté, comme par exemple pour créer un mur d'écrans. Voir mettre en miroir sur plusieurs écrans en même temps.

Pour du gaming, où on recherchera le moins de latence possible avec la gestion des manettes ce ne sera pas la solution. Je vous conseille cet excellent journal de Bubar sur [LinuxFR](https://linuxfr.org/users/bubar/journaux/cloud-gaming-at-home-2-le-retour) qui explique comment déporter son jeu sur une tablette ou autre. On doit pouvoir faire pareil pour de la bureautique mais ça doit être « overkill ».

# Emuler un écran

Pour étendre ou dupliquer l'affichage sur un second écran, il faut que le système détecte un second écran, qu'il soit réel (physique) ou virtuel. Il existe plusieurs méthodes que je vous expose ici.

## Pilote dédié à l'application

Sous Windows, les logiciels **spacedesk** ou **superdisplay** installent un pilote, utilisable uniquement par eux, pour étendre l'affichage. Rien à ajouter ici.

## Emulateur d'affichage virtuel

Pour faire croire à l'ordinateur que vous avez un autre écran, le plus simple est un petit adaptateur matériel qui émule un écran externe. J'ai testé avec deux adaptateurs, ou dongles, en USB-C et HDMI. Une fois branchés, le système détecte un nouvel écran visible dans les paramètres d'affichage.

![Dongles USB ou HDMI](/assets/img/2025-12-07/dongles.jpg)

Le [premier en USB-C](https://www.amazon.fr/dp/B088H367TF?smid=A12KOLKL8Y0ERS&ref_=chk_typ_imgToDp&th=1) est destiné aux machines supportant l'affichage via port USB-C, ce qui est le cas de beaucoup de PC portables laptops, mais qui n'est pas le cas de mon PC fixe. Sur mes laptops, le branchement fait immédiatement apparaitre un second écran 1080p dans les périphériques et réglages, tant sous Linux que Windows.

Le second est un [adaptateur HDMI](https://www.amazon.fr/dp/B09DKGJ2NF?ref=ppx_yo2ov_dt_b_fed_asin_title) à connecter sur la sortie idoine du laptop ou de la carte graphique. Il a fonctionné directement sur l'ensemble de mon matériel, me proposant des résolutions virtuelles jusqu'au 4K.

Vous réglerez la résolution virtuelle à ce qui correspondra à vos besoins et à l'appareil qui affichera votre second écran, et vous activerez l'affiche étendu ou en mode miroir, selon vos besoins.

## Pilote d'affichage virtuel Windows

Microsoft a introduit la notion de « **Indirect Display Driver** » dans son SDK, qui permet de se passer d'émulateur d'affichage USB. 

Quelques personnes sympathiques ont écrit un [pilote](https://github.com/ge9/IddSampleDriver/releases).

L'installation est décrite sur la page précédente et [ici](https://github.com/roshkins/IddSampleDriver/releases).

Si vous souhaitez désactiver la carte virtuelle après installation, ouvrez le **Gestionnaire de périphériques**, puis **Cartes graphiques**, bouton droit sur « **IddSampleDriver Device** » puis « **Désactiver l'appareil** » (ou l'activer, le cas échéant).

## Affichage virtuel sous Linux

> ! **X vs Wayland**
> Je n'ai pas réussi sous Wayland, dont les compositeurs graphiques semblent si différents qu'il ne semble pas y avoir de méthode standard.

Si vous êtes encore sous **X-Window (Xorg)**, la commande **xrandr** permet d'ajouter un écran virtuel, ou en tout cas de faire croire qu'il existe. D'abord, tapez xrand :

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

Le moniteur de mon laptop s'appelle eDP-1. On va l'étendre sur HDMI-1, en le mettant à gauche :

```bash
seb@Y13:~$ xrandr --addmode HDMI-1 1920x1080
seb@Y13:~$ xrandr --output HDMI-1 --mode 1920x1080 --left-of eDP1
```

L'écran principal peut se mettre à clignoter. Mais il sera étendu à gauche de votre affichage actuel. Pour l'arrêter :

```bash
seb@Y13:~$ xrandr --output HDMI-1 --off
```

Le défaut c'est que l'affichage reste vu comme déconnecté, notamment par les paramètres d'affichage du bureau. Pour éviter ça une modification de la configuration de Xorg sera nécessaire.

* Belle vidéo explicative sur [Youtube](https://www.youtube.com/watch?v=ftXv1Uo0daA).
* Un discussion sur ce problème sur [Reddit](https://www.reddit.com/r/ManjaroLinux/comments/sgkokh/
xrandr_showing_hdmi_output_as_disconnected_when/)
* Et comme toujours pour Arch, un excellente [documentation](https://wiki.archlinux.org/title/Multihead#RandR)

Pour **Wayland**, je n'ai pas trouvé de solution car il n'y a pas qu'un seul compositeur, notamment celui de Gnome qui n'accepte rien (une philosophie de Gnome: « *si vous ne comprenez pas nos choix, c'est que vous êtes trop stupide pour nous* ») et il ne semble pas d'y avoir d'outils communs. Utilisez une petite clé de simulation USB.

# Les solutions spécifiques à Windows

On commencera par ici, vu que mon desktop est sous Windows. Pas d'inquiétude, on passe à Linux juste après.

## Spacedesk

[Spacedesk](https://www.spacedesk.net/) est la solution gratuite pour Windows la plus simple à mettre en œuvre et aux performances excellentes :
* Un pilote à installer sous Windows
* Un client à installer sur Android, iOS, Windows ou MacOS

La connexion se fait soit via réseau, soit directement en USB. En IP, j'ai pu constater un débit de 50 Mbit/s pour du 1080p, avec une faible latence. En USB, c'est encore plus rapide, la latence est presque inexistante. Ça ne fonctionnait pas au début jusqu'à ce que je me rappelle que j'avais activé le **débogage USB** sur la tablette. En le stoppant, tout s'est mis à fonctionner. Pensez-y donc si ça coince.

![Spacedesk](/assets/img/2025-12-07/spacedesk.jpg)

Dans les deux cas, il y a une légère consommation CPU, assez pour parfois entendre les ventilateurs démarrer, à noter que cette consommation est supérieure en passant par le réseau.

Spacedesk n'est pas qu'une simple solution de partage d'écran : les entrées et sorties fonctionnent, on peut utiliser l'écran tactile, le stylet, le clavier virtuel, le son, le micro du client. On peut donc par exemple utiliser son logiciel de dessin ou de retouche favoris pour travailler avec son stylet, comme une tablette graphique. Mais pour aller au-delà des fonctions de base, il existe une version Pro, payante.

## Superdisplay

[Superdisplay](https://superdisplay.app/) est encore plus simple que Spacedesk. On installe le pilote côté Windows, le client côté Android, on branche le câble USB, et ça fonctionne tout seul. Il est aussi possible de passer par le réseau. Superdisplay est en fait un driver de tablette graphique et agit comme tel. Vu comme un écran ordinaire, il peut être étendu ou cloné, et les stylets fonctionnent. Quant à la latence en USB ? Aucune. C'est aussi simple que ça.

Problèmes techniques ? Aucun. C'est simple, rapide et efficace. Superdisplay utilise l'accélération matérielle, et un réglage adaptatif de la qualité, qui peut être optimisé en cas de besoin. C'est vraiment bluffant. Mais où est le loup ? Eh bien, c'est payant. Après trois jours, vous devrez vous acquitter de 16,99€ pour continuer à l'utiliser. 

![Superdisplay](/assets/img/2025-12-07/superdisplay.jpg)

Et, comme indiqué, ce n'est que pour Windows et Android. Si vous avez 16,99€ à dépenser, c'est la solution la plus performante.

# Les solutions sous Linux

## Deskreen CE

[Deskreen CE](https://deskreen.com) est compatible Linux, Windows, MacOS. Pour le moment en version CE (Community Edition), le [code source](https://github.com/pavlobu/deskreen/releases) est disponible. Sa licence est la GPL Affero, qui oblige les services utilisateurs (les programmes qui consomment Deskreen) à être eux-mêmes libres.

L'auteur travaille sur une version Pro qui sera payante.

Mon, premier essai a été malchanceux car la version 3.1.13 n'a jamais voulu fonctionner. J'ai laissé un rapport de bug sur le github de l'auteur, et en 24 heures, une version 3.1.15 puis 3.1.17 corrigeant le problème, a été diffusée.

Je teste sous Linux Mint via une [AppImage](https://en.wikipedia.org/wiki/AppImage).

```bash
seb@Y13:~/Téléchargements$ chmod +x AppImage...
seb@Y13:~/Téléchargements$ ./AppImage
```

**Deskreen** affiche un QR Code ou une URL à saisir sur l'autre machine, puis demande quel écran ou application à partager. Car l'affichage passe par un flux vidéo et une capture des événements via le navigateur web HTML5 distant.

Pour étendre l'affichage, il faut un adaptateur d'écran virtuel, comme le modèle USB ou HDMI qui simulent un second écran, ou les autres méthodes de type xrandr sous Linux ou IDD sous Windows.

Il n'y a aucune différence de fonctionnement entre les versions Linux et Windows. Par contre, je n'ai pas réussi à partager l'écran sous Wayland (Fedora 43 et KDE, ou Linux Mint 22.2 avec session Wayland). L'affichage est corrompu sur l'écran distant.

![Deskreen](/assets/img/2025-12-07/deskreen.jpg)

Ca fonctionne bien, mais avec une latence assez importante ! Comme vous le voyez sur la photo, j'ai dupliqué le second écran virtuel de ma machine sous Windows vers un laptop sous Linux Mint et une tablette Android. 

## Weylus

[Weylus](https://github.com/H-M-H/Weylus?tab=readme-ov-file#linux) est une alternative à Deskreen. Toujours basé sur HTML5, proposant lui aussi un QR Code ou une URL, j'ai été surpris de constater que la latence était plutôt faible même si j'ai des saccades. L'accélération matérielle est possible, mais dépend du matériel et/ou de la distribution. La seule façon que j'ai eu de la faire fonctionner est d'utiliser la version flatpak.

Par contre, Weylus a fonctionné avec Wayland ! J'ai pu étendre mon affichage ou dupliquer celui-ci depuis une Fedora 43 avec KDE. Sur la photo cependant vous voyez Linux Mint (donc, Xorg) partager son second écran virtuel sur le client HTML5 de Windows 11.

![Weylus](/assets/img/2025-12-07/weylus.jpg)

Weylus est capable de capturer la souris, les stylets et le multitouch sur le second écran. Pour ces derniers une configuration additionnelle est nécessaire côté udev pour autoriser la capture des événements.

Weylus semble le pur produit d'un linuxien à l'ancienne avec une interface graphique des plus "industrielles", mais cependant fonctionnelle. J'ai apprécié les différentes options et notamment le choix des moniteurs ou fenêtres à afficher sur l'écran secondaire.

Je vois cependant quatre problèmes avec Weylus. 
1. La dernière version sur le dépôt est la 0.11.4.
2. L'accélération graphique qui n'a pas fonctionné sur aucune de mes quatre machines (Intel IGP ou Arc), qui ralentit tout, 
3. Même si le projet semble actif avec commits côté git, la dernière version publiée date d'octobre 2021. 
4. L'absence de paquetages, seul une archive contenant le binaire statique est fournie.

On trouve cependant un [flatpak](https://flathub.org/en/apps/io.github.electronstudio.WeylusCommunityEdition qui semble plus récent). Mais quid du support de l'auteur ?

## VNC, RDP and co

Solution surprenante qui ne fonctionne évidemment qu'à moitié et pour une bonne raison. On peut lancer un serveur VNC ou RDP sur son bureau Windows ou Linux, puis y accéder via le client adapté. Gnome et KDE intègrent un serveur pour le partage de bureau, et donc d'un second écran, émulé ou non. Hors Gnome et KDE, il faudra installer un serveur VNC. Celui de KDE m'a retourné une erreur côté client, comme quoi j'avais une résolution de 0x0... Certes, Fedora tournait dans une VM, mais Deskreen et Weylus

Notamment, sous Linux, on peut utiliser Vino comme serveur VNC. En déclarant un écran virtuel avec xrandr, j'ai pu l'afficher avec un client VNC. Mais c'est une très mauvaise idée : l'écran du PC qui partage est sous le contrôle du clavier et de la souris du PC, tandis que la partie affichée par le client est sous le contrôle du client !

Autre souci, Vino ne semble prendre que le premier affichage, de gauche à droite. Donc, il faut que l'écran virtuel xrandr soit à gauche de votre écran principal, et que donc l'écran additionnel, lui, soit à droite !

Cette solution est donc à conserver uniquement si vous souhaitez afficher un miroir de votre écran.

```bash
seb@Y13:~$ sudo ufw allow 5900
seb@Y13:~$ sudo apt install vino
seb@Y13:~$ gsettings set org.gnome.vino require-encryption false
seb@Y13:~$ /usr/lib/vino/vino-server
```

![Via VNC](/assets/img/2025-12-07/vnc.jpg)

Via un émulateur (USC-C, HDMI), vino exporte les deux écrans d'un coup sans pouvoir n'en afficher qu'un seul...

## Xpra

Je parle rapidement de [Xpra](https://xpra.org/index.html) ici, car ce n'est pas à proprement parler d'une solution de partage d'écran, mais d'une solution client-server X. Xpra va démarrer un serveur X « Seamless » sur lequel n'importe quel client pourra se connecter. Tant client que serveur peuvent tourner sous Linux, Windows et MacOS.

La [documentation](https://github.com/Xpra-org/xpra/tree/master/docs) est complète, mais peut devenir complexe et fausse. Les exemples fournis (seamless, desktop) n'ont jamais fonctionné.

Et notamment par le fait que par défaut l'accès se fait via une socket et pas par le réseau, et que sauf côté client (et encore) il faudra passer par la ligne de commande. Et même là, même en démarrant l'écoute sur un port TCP, le client graphique ne le vois pas, et en ligne de commande, rien ne se passe, tant sous Windows que Linux. J'ai abandonné. Peut-être suis-je trop bête.

## Petit coup de gueule

Voyant des commits bien plus récents sur le dépôt de Weylus, j'ai tenté de reconstruire la dernière version avec les instructions fournies : Le nombre de paquetages lié aux dépendances est effroyable ! L'exemple oublie aussi l'installation de npm, et de cargo, qui va installer du Rust. Puis de nasm. Puis de plein de trucs.

Et à la fin, ça ne fonctionne pas. Clairement la maladie des machins à la mode comme npm et ses millions de dépendances, ou de Rust, qui évoluent trop vite et cassent la compatibilité. J'ai coincé sur Cargo qui demande "edition2024", qui est une version plus récente que celle de mes Ubuntu LTS. Donc il faut passer par rustup pour installer les nouvelles versions. Puis, erreur parce que nasm est manquant, puis que les paquetages de développement de xcb-dri3 est manquant, puis libavcodec, libavfilter, libva, libavdevice, ... 2 Go de dépendances ! J'ai lâché l'affaire. Ça n'a pas compilé, erreur à l'étape finale de génération du binaire, et après une heure, j'en ai eu marre.

**On marche sur la tête, avec ça on n'est pas prêt d'avoir un desktop Linux pour les utilisateurs basiques qui achètent leurs PC au supermarché.**

Certes, ces utilisateurs ne vont pas forcément tenter d'installer Weylus et prendront Deskreen, mais tout de même... Développeurs, pensez aux gens normaux.

# Conclusion

Deskreen est la solution la plus efficace sous Linux, mais fonctionne bien partout, comme client et serveur. Cependant le passage à Wayland va poser un vrai problème de compatibilité, sauf si les soucis se règlent, Weyfus étant laborieux.

Sous Windows, on se tournera cependant vers Spacedesk qui est gratuit, avec des clients pour Android, ou Superdisplay qui est encore plus rapide mais payant, uniquement si l'écran secondaire est sous Android.

Si vous voulez encore mieux, notamment pour faire du gaming, alors je vous conseille cet excellent journal de Bubar sur [LinuxFR](https://linuxfr.org/users/bubar/journaux/cloud-gaming-at-home-2-le-retour).

Je suis persuadé qu'il existe d'autres solutions. N'hésitez pas à partager dans les commentaires du journal sur LinuxFR si vous venez de mon site personnel.

