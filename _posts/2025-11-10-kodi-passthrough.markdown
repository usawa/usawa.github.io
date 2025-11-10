---
layout: default
title:  "Kodi sur Amazon Fire TV et une barre de son Atmos et DTS:X"
date:   2025-11-10 16:30:00 +0100
description: Après mes aventures avec Samsung, voici les bons réglages pour que Kodi diffuse du Dolby Atmox et du DTS:X sur ma barre de son.
categories: kodi
---
Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

- [Kodi sur Fire TV, tous les formats audio](#kodi-sur-fire-tv-tous-les-formats-audio)
- [Les boitiers et clés Amazon Fire TV](#les-boitiers-et-clés-amazon-fire-tv)
  - [Défauts](#défauts)
  - [Qualités](#qualités)
  - [Comparé aux autres](#comparé-aux-autres)
- [Avec applications par défaut du catalogue](#avec-applications-par-défaut-du-catalogue)
- [Kodi via les applications additionnelles](#kodi-via-les-applications-additionnelles)
- [Les réglages](#les-réglages)
  - [Réglages sur le téléviseur](#réglages-sur-le-téléviseur)
  - [Réglages de la Fire TV](#réglages-de-la-fire-tv)
  - [Réglages de Kodi](#réglages-de-kodi)
- [Les résultats](#les-résultats)
- [Cadeau Bonux, Plex sur Kodi](#cadeau-bonux-plex-sur-kodi)

# Kodi sur Fire TV, tous les formats audio

Dans le précédent article, j’expliquais mes déboires concernant la gestion eARC/ARC de mon téléviseur Samsung. Mais j’indiquais aussi que j’arrivais à lire les formats Dolby Atmos et DTS:X depuis mon boitier de streaming Amazon Fire TV, vers ma barre de son. Ici je vous explique comment. Si tout fonctionne pour vous et que les formats Dolby vous suffisent ou sont les seuls supportés par votre équipement, ça peut tout de même être instructif, et au pire, ne changez rien.

# Les boitiers et clés Amazon Fire TV

Je dispose deux Amazon Fire TV: une Fire TV Stick 4K Max de seconde génération (2023), et un boîtier Fire TV Cube de 3ème génération (2022). Toutes les spécifications sont [ici](https://developer.amazon.com/docs/device-specs/device-specifications.html).  Les Fire TV ont des qualités compensant largement leurs défauts. 

## Défauts

Parmi les défauts je ne retiendrai que ceux-ci:
* Une alimentation **micro-USB** sur le Fire TV Stick, mais quelle idée,
* Un port **USB 2.0** seulement qui limite fortement le débit d’un adaptateur Ethernet ou hub USB,
* Un port (ou un adaptateur optionnel) **Ethernet 100 Mbit/s**
* **FireOS** n’est pas Android TV et le catalogue est plus réduit.

## Qualités

Mais parmi les qualités, on a:
* Un **excellent débit Wifi** (jusqu’au 6E), même en Wifi 5
* De **l’ethernet 400 Mbit/s** via un quelconque **adaptateur USB 2.0**, par exemple UGreen
* Un vrai support du **4K** et du **HDR**
* Un vrai support des formats **Dolby** (dont Atmos) et, théoriquement, DTS
* Un mode **Transfert** (Passthrough)
* Un catalogue applicatif exhaustif
* **Alexa** fonctionne bien (_Alexa, allume la télé et passe sur l’entrée antenne_ fonctionne impec).
* La possibilité d’installer des **applications Android** de manière alternative
* Un très bon support de, et par, **Kodi**

## Comparé aux autres

J’ai beaucoup hésité (et il m’arrive encore de le faire) à prendre un **NVidia Shield Pro**. Mais je trouve fou de continuer de vendre hyper cher fin 2025 un truc, certes superbe, sorti en 2019, au même prix que le jour de sa sortie. On m’a aussi parlé de **Roku Ultra** qui marcherait du tonnerre, mais, pareil, à 200 euros, ça fait cher pour les tests.

Avant les Fire TV, j’ai testé des **Mi Box S Gen 1** et **Gen 2**. Le truc drôle: ça fonctionnait mieux sur une Gen 1, par exemple, et j’ai vraiment comparé les images, en Gen 2 ni Kodi ni VLC ne me sortaient du 4K ! (On branche un clavier, on appuie sur S, et on voit que Kodi réduit en 1080p). J’ai aussi tenté de rester sur la Box 4K de Bouygues, qui s’en sortait pas trop mal, mais sans réussir à sortir du DTS, et le boîtier est très lent.

# Avec applications par défaut du catalogue

Avec **Netflix**, **Prime Video** ou **Disney+** fournis, les contenus sont joués sans aucun souci. **Dolby Digital**, **Dolby Atmos**, tout passe vers ma barre de son. Ceux qui pensent avoir du DTS:X avec Disney+ peuvent aller voir ailleurs: Disney le réserve à quelques matériels privilégiés.
**Plex** est supposé supporter le DTS en Passthrough. En pratique, je n’ai jamais réussi. Mes pistes DTS sont converties en Dolby, et ça râle sur les forums parce que c’est supposé fonctionner. Un exemple, d’ailleurs, est montré dans cette [vidéo Youtube](https://www.youtube.com/watch?v=583HCGSd2KE).
**VLC** est fourni dans le catalogue, sort bien du 4K sans souci, mais j’ai le même fonctionnement avec les pistes son. Dès qu’il s’agit d’autre chose que du Dolby, il n’y a pas de Passthrough et c’est un peu rageant. J'ai aussi testé **Vimu** (clairement basé sur VLC vu le design et les menus de préférences), et malgré leur [publicité]() alléchante, pareil, je n’ai pas de Passthrough pour le DTS. Je n’ai pas cherché plus que ça, puisque Kodi me suffit.

# Kodi via les applications additionnelles

Les Fire TV permettent l’installation d’applications additionnelles (APK) en activant les sources inconnues. Une petite application sympa appelée **Downloader** aide ensuite pas mal. Voici l’un des nombreux [tutoriels](https://www.cyberghostvpn.com/privacyhub/kodi-on-firestick/) pour installer Kodi. Installez la version **Omega 21.2** (fin 2025), qui fonctionne pas trop mal chez moi. Notez que lors de mes nombreux tests, j’ai fait planter plusieurs fois Kodi, jusqu’à devoir redémarrer mes Fire TV.

# Les réglages

Voici comment mes Fire TV et Kodi me sortent les formats souhaités.

## Réglages sur le téléviseur
Selon votre modèle, basculez votre sortie en priorité:
* Sur **eARC** si votre barre / ampli le supporte
* Sur **ARC**, dans les autres cas (mais Pas de Dolby Atmos/TrueHD lossless, ni de DTS-X/HD/MA).
* Voyez si le format de sortie audio numérique peut être mis sur **Auto** ou **Transfert** (**Passthrough**), sur le port HDMI sur lequel la Fire TV est connectée.

## Réglages de la Fire TV
Sur la Fire TV, et selon votre modèle :
* Dans **Affichage et Sons**, activez le **Passthrough**, en bas (Fire TV Cube)

![Fire TV Cube Passthrough](/assets/img/2025-11-10/01-firetv-passthrough.jpg)

*Passthrough activé sur un Fire TV Cube*

* Dans **Son Surround**, je mets sur **Digital Audio Plus**.

![Fire TV Surround](/assets/img/2025-11-10/02-firetv-son-surround.jpg)

*Digital Audio Plus active le mode transfert sur ma TV Samsung...*

## Réglages de Kodi
* Dans Kodi, allez dans les **Paramètres**, **Système**. En bas à gauche, passez en mode **Expert**.
* Dans les **Paramètres Audio**, passez le nombre de **canaux** à ce que vous avez, **7.1** est impeccable, puisque de toute façon, on sortira en mode Passthough (laisser-passer, transfert).
* Dans **Configuration de la sortie**, choisissez **Meilleure concordance**, ou **Optimisé**.

![Kodi réglages audio 1](/assets/img/2025-11-10/03-kodi-sound-1.jpg)

*Nombre de canaux, configuration de la sortie*

* En bas, activez le mode **laisser-passer (Passthrough)**.
* Le périphérique de laisser-passer va dépendre de vos résultats. Chez moi **Audiotrack (IEC)** fonctionne bien, notamment en ARC, tandis que Audiotrack (RAW) semble bien passer avec eARC. Faites attention, j’ai remarqué, lors de mes tests variés, que Kodi basculait parfois seul de l’un à l’autre.
* Enfin, activez les différents **codecs de sortie compatibles** avec votre matériel. 

![Kodi réglages audio 2](/assets/img/2025-11-10/04-kodi-sound-2.jpg)

*Passthrough, IEC, codecs de sortie*

# Les résultats

Il n’y a plus qu’à tester. Voici le résultat avec deux sources. Dolby Atmos:

![Du Dolby Atmos sur ma barre de son](/assets/img/2025-11-10/05-kodi-atmos.jpg)

*Du Dolby Atmos sur ma barre de son*

Puis DTS:X:

![Du DTS:X sur ma barre de son](/assets/img/2025-11-10/06-kodi-dts-x.jpg)

*Du DTS:X sur ma barre de son*

# Cadeau Bonux, Plex sur Kodi
L’application native Plex ne semble pas vouloir me sortir de DTS, mais Kodi le peut. Saviez-vous qu’on peut lier Kodi et Plex, de manière à accéder au catalogue Plex depuis Kodi ? Je ne rentrerai pas dans les détails, mais je vous conseille d’utiliser l’add-on **PKC**, **PlexKodiConnect**. Vous trouverez les détails d’installation [ici, PKC](https://github.com/croneter/PlexKodiConnect?tab=readme-ov-file#download-and-installation).
L’avantage ? Puisque c’est Kodi qui joue, il va aussi gérer les pistes audio DTS/DTS:X ! Plus besoin de l’application native !