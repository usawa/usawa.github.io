---
layout: default
title:  "Reconnaissance faciale : connectez-vous sous Linux avec votre visage"
date:   2026-07-12 07:00 +0100
description: "Il y un truc chouette sous Windows, iOS et Android, c'est la reconnaissance faciale: une caméra spécialisée reconnait votre visage et ouvre ou déverouille votre session. Pratique ! Et sous Linux ? C'est possible, mais moins intuitif. Démonstration"
categories: 
- authentification

---

Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

- [La reconnaissance faciale](#la-reconnaissance-faciale)
- [Linux et ses limites](#linux-et-ses-limites)
  - [Les caméras](#les-caméras)
  - [La fragmentation](#la-fragmentation)
- [Implémentation](#implémentation)
  - [Modèle de test](#modèle-de-test)
  - [Trouver sa webcam](#trouver-sa-webcam)
  - [isoler le chemin du fichier périphérique](#isoler-le-chemin-du-fichier-périphérique)
  - [Howdy](#howdy)
    - [Installation](#installation)
    - [Configuration](#configuration)
    - [Droits et SELinux](#droits-et-selinux)
    - [PAM](#pam)
  - [Test et petit souci](#test-et-petit-souci)
- [BioPass](#biopass)
  - [Un beau projet](#un-beau-projet)
  - [Installation](#installation-1)
  - [Configuration](#configuration-1)
    - [Capture du visage](#capture-du-visage)
    - [PAM et SELinux](#pam-et-selinux)
- [Changements généraux](#changements-généraux)
- [Conclusion](#conclusion)

>**Note**
>Testé sous Fedora 44 et un Dell Latitude 5290 2-in-1.


# La reconnaissance faciale

Face Id sur les iPhones, Hello sous Windows, reconnaissance faciale dans les réglages d'Android (selon les modèles)... On trouve des solutions biométriques sur pas mal d'OS et de matériel grand public. C'est une option bien pratique: on enregistre son visage, et hop, on se connecte juste en regardant son écran ou son smartphone. Votre visage, ou votre œil, est tout comme une empreinte digitale, une méthode d'authentification biométrique. Je ne reviendrai pas sur les notions d'identification, d'authentification et ses trois principales méthodes, déjà expliquées dans l'article sur FIDO2. La reconnaissance faciale est de type *Something you are* / **Quelque chose que vous êtes**.

Comment ça fonctionne ?

Selon le fabricant et/ou l'éditeur, les méthodes peuvent être différentes. Parfois une simple webcam suffit, mais souvent ce sont des caméras infrarouge (IR) qui sont utilisées. Dans tous les cas, le système va établir un **motif** (*pattern*) de votre visage, en enregistrant un certain nombre de points de celui-ci, puis va stocker ce motif dans un fichier. Lors de l'authentification, un composant (matériel ou logiciel) va de nouveau  étudier le motif de visage devant la caméra, et le comparer avec celui préalablement stocké. S'il y a correspondance, l'utilisateur sera authentifié.

Certains systèmes plus évolués apprennent et reconnaissent les évolutions du visage: barbe, lunettes, maquillage, vieillissement, etc. Les caméras infrarouge reconnaissent l'utilisateur dans le noir.


# Linux et ses limites

Allez, je vais encore prendre le risque d'en froisser certains, mais bon, c'est comme ça.

## Les caméras

Quand on achète un Mac, on est (quasiment) certain que par défaut, il va parfaitement fonctionner: OS (MacOS) et matériel sont parfaitement synchrones et le support matériel optimal (jusqu'à la décision d'Apple d'abandonner le modèles qu'elle décrète trop anciens). Bon, mauvais exemple car à la date de la rédaction de cet article, la reconnaissance faciale n'est toujours pas supportée sur les Macs. Mais vous voyez l'idée. Face ID est arrivé avec l'iPhone X, avec un support matériel dédié (et sécurisé : iOS n'a pas accès aux motifs de reconnaissance, tout est matériel). Android garantit une compatibilité qu'avec le matériel supporté, les constructeurs fournissent des machines compatibles avec une ou plusieurs fonctions de l'OS, dont la reconnaissance faciale. Et Windows Hello, ça fonctionne, c'est tout.

Linux, son matériel, ses services et ses environnement graphiques, notamment bureaux, sont généralement libres et dépendent tant de la bonne volonté des développeurs (souvent bénévoles, mais pas toujours) et des fabricants de périphériques à fournir les spécifications et API de leur matériel. Et c'est ainsi que le fonctionnement de certains périphériques sous Linux relève parfois da la loterie. Dont les caméras (webcams).

## La fragmentation

Même quand les caméras fonctionnent, on se retrouve parfois perdus entre plusieurs « standards ». Celui par défaut sous Linux se nomme  **V4L**, *Video For Linux*. Sauf que... de nombreux logiciels et matériels ne sont pas compatibles, pour diverses raisons, comme la complexité des modèles, des protocoles, du type de bus, les spécifications propriétaires inconnues etc. Alors, certains ont trouvé malin de recréer une nouvelle librairie, libcamera. Mais comme souvent, ça rajoute de la complexité, et le support applicatif ne suit pas.

Rajoutons à ça qu'une solution de reconnaissance faciale doit s'appuyer sur le matériel, le support V4L / libcamera ou autre, mais aussi sur l'écosystème bureautique et sécurité de Linux : **Pluggable Authentication Module** *PAM*, mais aussi le support, par exemple, des "Display Manager", comme **KDM**, **GDM**, **SDDM**, etc. On a déjà vu ça avec la clé FIDO2, d'ailleurs.

Rajoutons enfin que chaque distribution Linux est différente, et qu'en fonction des implémentations, des dépendances et des choix de configuration, une solution fonctionnera, ou ne fonctionnera pas, selon que vous soyez sous Ubuntu, Fedora, Arch, ou autre.

Bref, la fragmentation complexifie tout. 

Pour finir, il faut bien évidemment une implémentation (logicielle par exemple) de la reconnaissance faciale, avec ses algorithmes de détection, de modélisation, etc. Et sous Linux, il n'y avait jusqu'à il y a peu, qu'un seul projet, appelé **Howdy**. Sauf que la dernière version stable a trois ans, et la version beta (3.0.0) n'est toujours pas officiellement sortie, et n'avance pas. Les distributions Linux avancent et les incompatibilités (bibliothèques, Python) apparaissent. Cependant, je vous montrerai qu'on peut y arriver, et que ça fonctionne.

Depuis peu une autre solution arrive, appelée **Biopass**. Elle est très prometteuse. Nous tenterons aussi de la mettre en œuvre.

# Implémentation

## Modèle de test

Mon modèle de test est un laptop 2-en-1 de type Tablet PC avec clavier amovible, un Dell Latitude 5290 2-in-1. Il est équipé de deux caméras :
1. Une caméra frontale Intel IPU3 (*Image Processing Unit*) sur bus PCI, couleur, HD, avec un capteur de type OV5670. Après des mois, j'ai pu la faire fonctionner partiellement, mais le support noyau de base est déficient et j'ai eu la chance de tomber sur le dépôt git d'un gars qui a pu la faire fonctionner.
2. Une caméra frontale infrarouge USB Realtek, qui est reconnue d'office, driver uvcvideo c'est celle que je vais utiliser.

Pour disposer de la caméra infrarouge (fonctionnelle aussi sous Windows avec Hello), mon modèle n'ayant pas été fourni avec, j'ai dû la commander sur Aliexpress puis démonter l'antenne du modem 4G intégré au laptop. C'est ou le support de la 4G, ou la webcam infrarouge, au choix.

Je travaille actuellement avec une distribution Fedora Linux 44, c'est celle qui intègre (jusqu'à présent) le mieux du support des Tablet PC sans avoir trop à bidouiller. Mais les manipulations doivent pouvoir être transposées sur d'autres distributions, sauf la partie SELinux propre aux distributions Redhat et dérivées.

## Trouver sa webcam

Si comme chez moi le laptop a plusieurs caméras, il faut trouver la bonne. Deux outils sont utiles:
1. **v4l2-ctl** pour lister les caméras et en fournir des détails
2. **ffmpeg** pour tester la caméra

Tout d'abord, on liste les périphériques caméras disponibles:

```bash
$ v4l2-ctl --list-devices
ipu3-imgu (PCI:0000:00:05.0):
	/dev/video0
	/dev/video1
	/dev/video2
	/dev/video3
	/dev/video4
	/dev/video5
	/dev/video6
	/dev/video7
	/dev/video8
	/dev/video9
	/dev/media0

Intel IPU3 CIO2 (PCI:0000:00:14.3):
	/dev/video10
	/dev/video11
	/dev/video12
	/dev/video13
	/dev/media1

Integrated_Webcam_HD: Integrate (usb-0000:00:14.0-5):
	/dev/video14
	/dev/video15
	/dev/video16
	/dev/video17
	/dev/media2
	/dev/media3
```

C'est un peu, la multiplication des petits pains ici: j'ai deux caméras mais une vingtaine de fichiers spéciaux. Je n'ai pas encore cherché le pourquoi du comment, mais je dois maintenant tester les fichiers périphériques qui concernent la caméra USB, listés à la fin. J'utilise FFMPEG jusqu'à avoir l'affiche souhaité, ici /dev/video16

```bash
$ ffplay -fflags nobuffer -flags low_delay -framedrop -i /dev/video16
...
Input #0, video4linux2,v4l2, from '/dev/video16':
  Duration: N/A, start: 6180.757779, bitrate: 61036 kb/s
  Stream #0:0: Video: rawvideo (YUY2 / 0x32595559), yuyv422, 340x374, 61036 kb/s, 30 fps, 30 tbr, 1000k tbn, start 6180.757779
6207.08 M-V: -0.023 fd=   2 aq=    0KB vq=    0KB sq=    0B 
```
Admirez le résultat !

![IR avec ffmpeg](/assets/img/2026-07-12/howdy-capture.png)

Je peux afficher quelques détails:

```bash
$ v4l2-ctl -D -d /dev/video16
Driver Info:
	Driver name      : uvcvideo
	Card type        : Integrated_Webcam_HD: Integrate
	Bus info         : usb-0000:00:14.0-5
	Driver version   : 7.0.14
```

## isoler le chemin du fichier périphérique

Le nom du fichier périphérique /dev/video16 peut évoluer selon les boots et reboots, le changements liés à l'énumération des périphériques, notamment si certains sont enlevés ou rajoutés, etc. On doit pouvoir jouer à rendre ça statique (udev ?) mais je ne m'y lancerai pas. Par contre, s'agissant d'un laptop, il est fort probable que l'ordre d'énumération (PCI, USB) des composants internes, change. Les périphériques V4L sont listés dans /dev/v4L, par id ou par path. Chez moi, pas de succès avec les id, mais je trouve ce que je cherche dans **by-path**:

```bash
$ ls -l /dev/v4l/by-path/ | grep video16
lrwxrwxrwx. 1 root root 13 12 juil. 15:28 pci-0000:00:14.0-usb-0:5:1.2-video-index0 -> ../../video16
lrwxrwxrwx. 1 root root 13 12 juil. 15:28 pci-0000:00:14.0-usbv2-0:5:1.2-video-index0 -> ../../video16
```

Le chemin de périphérique que j'utiliserai sera donc:

    /dev/v4l/by-path/pci-0000:00:14.0-usbv2-0:5:1.2-video-index0

## Howdy

### Installation

Comme indiqué plus haut **Howdy** commence à prendre de la bouteille et le support devient compliqué. J'ai dû me baser sur plusieurs documentations et dépôts avant de trouver le mix qui fonctionne bien sur Fedora 44, notamment le support d'un Python récent:

[Le dépôt officiel de Howdy](https://github.com/boltgolt/howdy)
[Le dépôt additionnel Principis](https://copr.fedorainfracloud.org/coprs/principis/howdy/)
[Le dépôt additionnel Starfish](https://copr.fedorainfracloud.org/coprs/starfish/howdy-beta/)
[L'inestimable documentation Arch](https://wiki.archlinux.org/title/Howdy)

J'ai tenté via le dépôt Principis, si l'installation fonctionne, l'interface gtk prévue par la version Beta me retourne une erreur (bien que Howdy lui-même fonctionne). J'ai préféré me rabattre sur Starfish.

On active le dépôt Starfish et on installe Howdy Beta:

```bash
sudo dnf copr enable starfish/howdy-beta
sudo dnf --refresh install howdy
```

### Configuration

On édite la configuration de Howdy et on modifie la ligne **device_path** avec le chemin trouvé précédemment: 

```bash
EDITOR=vi howdy config
...
device_path = /dev/v4l/by-path/pci-0000:00:14.0-usbv2-0:5:1.2-video-index0
```

Il faut maintenant ajouter un motif (modèle) de son visage. On peut en ajouter plusieurs pour un compte, et ce n'est pas une mauvaise idée: plusieurs distances, angles, avec et sans lunettes, etc. On lance la commande et on suit les instructions. Chez moi la led rouge de la webcam infrarouge s'allume et clignote rapidement.

```
$ sudo howdy add
Adding face model for the user seb
Enter a label for this new model [Model #0]: seb

Please look straight into the camera

Scan complete
Added a new model to seb
```

Maintenant qu'on a notre reconnaissance, on peut tout d'abord tester, ce qui ouvrira une petite fenêtre graphique. On se place devant la caméra, on lance la commande: vert le visage est reconnu, rouge il ne l'est pas.

```
$ sudo howdy test
```

![moi en test howdy](/assets/img/2026-07-12/howdy-test.png)


Joli non ?

### Droits et SELinux

Le fichier périphérique appartient au groupe **video** avec les droits de lecture et d'écriture.

    rw-rw----+ 1 root video 81, 18 12 Juil. 18:25 /dev/video16

Il est donc normal d'ajouter l'utilisateur gdm dans ce groupe.

    $ sudo usermod -aG video gdm

Sur pas mal de distributions, ça suffirait, mais sous Fedora, SELinux est activé par défaut, et il faut des permissions explicites pour que les xDM (Display Manager) puissent accéder au périphérique vidéo. La suite vient de la documentation sur le dépot principis: on crée une politique de sécurité SELinux pour que xdm (et donc tout ce qui en dérive) pour autoriser certaines actions sur V4L et les fichiers périphériques.

1. Créez le fichier **howdy.te** avec le contenu plus bas
2. Compilez le module
3. Packagez-le
4. Insérez-le dans les politiques de sécurité SELinux

Fichier:

```
module howdy 1.0;

require {
    type lib_t;
    type xdm_t;
    type v4l_device_t;
    type sysctl_vm_t;
    class chr_file map;
    class file { create getattr open read write };
    class dir add_name;
}

#============= xdm_t ==============
allow xdm_t lib_t:dir add_name;
allow xdm_t lib_t:file { create write };
allow xdm_t sysctl_vm_t:file { getattr open read };
allow xdm_t v4l_device_t:chr_file map;
```

Séquence de commandes:

```bash
checkmodule -M -m -o howdy.mod howdy.te
semodule_package -o howdy.pp -m howdy.mod
semodule -i howdy.pp
```

### PAM

Le module PAM de Howdy doit être intégré à la configuration d'authentification PAM de GDM (dans me cas, Fedora classique). On a déjà abordé ça aussi dans le cas d'une clé FIDO2.

Le fichier PAM est **/usr/lib64/security/pam_howdy.so**, /lib64 est un lien vers /usr/lib64. Le chemin étant celui par défaut des modules PAM, on a pas besoin de le préciser dans la configuration des modules PAM. On modifie le fichier /etc/pam.d/gdm-password, en ajoutant, en seconde ligne:

    auth        sufficient     pam_howdy.so

**Sufficient** indique que la reconnaissance faciale suffit pour s'authentifier. Un jour j'écrirai peut-être quelque chose sur les subtilités de PAM et ses possibles terribles conséquences, mais c'est comme le "Vultech" sur le différentiel de feu Vilebrequin, qu'on a jamais vu. Qui sait ?

Il faut maintenant redémarrer le service, et attention ça va vous déconnecter violent ! 

    sudo systemctl restart gdm

## Test et petit souci

Si tout va bien, à l'écran de connexion de GDM, la led (si présente) de la caméra IR va flasher, reconnaitre votre visage, et vous connecter. Si ça ne fonctionne pas, ou en cas de temps dépassé (timeout) il reste toujours le mot de passe pour se connecter. L'action PAM sufficient n'empêche pas les autres de fonctionner.

Cet ensuite que j'ai rencontré un « petit » problème: le « keyring », le gestionnaire de mots de passe de Gnome, mais le souci pourrait être le même sous d'autres produits comme celui de KDE. Le gestionnaire se déverrouille via le mot de passe entré sous GDM. Or ici on ne le saisit pas. Et donc, au premier accès, Gnome vous le demandera... Le mot de passe servant de clé de décodage de trousseau, il faut soit envisager de supprimer ce mot de passe (et de perdre éventuellement l'encodage du fichier associé), soit chercher comment faire autrement. Une solution semble avoir été présentée dans l'issue associée, mais je ne l'ai pas testée.

[Une tentative avec ke keyring via PAM](https://github.com/boltgolt/howdy/issues/1092)


# BioPass

## Un beau projet

**Biopass** est une expérience biométrique multi-modale calquée sur le modèle de Windows Hello, et en cela va plus loin que Howdy. En effet Hello n'est pas uniquement pour la caméra, mais propose aussi, par exemple, la reconnaissance des empreintes digitales. C'est un sujet que j'aborderai probablement à terme dans un autre article. Biopass se propose d'intégrer plusieurs méthodes biométriques dans un même module PAM, et aussi dans une vraie interface de configuration.

[Lien vers Biopass](https://github.com/TickLabVN/biopass)

Biopass va donc plus loin que Howdy et bien que s'agissant d'un projet récent (avril 2025), il est très actif, avec plusieurs contributeurs, et il avance vite. Il propose des packages pour plusieurs distributions, et je dois admettre que l'intégration, bien qu'encore incomplète notamment sur Redhat / Fedora, est très prometteuse. C'est un projet à surveiller de près, qui peut clairement « casser la barraque ». On pardonnera à Biopass ses défauts de jeunesse.

## Installation 

On télécharge le package [RPM ici](https://github.com/TickLabVN/biopass/releases).

La version testée est la 1.4.1, au format RPM. 

L'installation via l'installateur graphique de Fedora n'a pas fonctionné comme prévu. En effet en post-installation un script se charge de télécharger des modèles IA de détection et de reconnaissance de visage, sauf que via l'interface le script ne fonctionne pas: il ne sait pas quel utilisateur a effectué le sudo.

On installe donc en ligne de commande avec sudo et rpm. (via sudo on récupère l'information de l'appelant, par exemple on sait via les variables d'environnement que c'est seb qui a appelé sudo).

```bash
sudo dnf install ./biopass-1.4.1-1.x86_64.rpm
```

En post installation le script se lance bien et installe dans mes répertoire personnels les modèles en question.  Chez moi ils sont dans **/home/seb/.local/share/com.ticklab.biopass/models**. Il serait plus intelligent, et une issue est ouverte à ce propos, de les placer dans un répertoire commun (/usr/share/...)

>**Note**: ça veut dire que si vous avez plusieurs utilisateurs sur votre machine, il faut lancer manuellement le script depuis chaque profil: **/usr/share/com.ticklab.biopass/download_models.sh**, ou aller les copier puis les chercher via l'interface, dans un dossier /usr/share...

## Configuration

### Capture du visage

Biopass, et c'est une évolution importante, propose une belle interface graphique. Je jette un œil tout d'abord côté modèles IA, ils sont bien là. Dans **configuration**, **authentication methods**, je sélectionne le bonne caméra (/dev/video16), je clique sur **Start Camera**, puis autant de fois que nécessaire sur **Capture**. Comme ma caméra infrarouge flashe, l'image s'affiche mal, c'est un défaut par rapport à Howdy, mais j'ai quatre visages enregistrés à la fin, c'est suffisant.

![Capture du visage Biopass](/assets/img/2026-07-12/biopass.png)

*J'ai une tête à faire peur, hein ?*

### PAM et SELinux

>**Attention avec SELinux !** Là aussi il faut autoriser les services xDM à accéder à V4L et au périphérique. De ce fait, c'est exactement la même configuration qui s'applique, celle pour Howdy. Dont acte.

La partie en haut **Strategy settings** indique que l'application ne modifie pas les réglages PAM, il faut le faire manuellement comme pour Howdy. La documentation est là:

[Intégration Biopass avec PAM](https://github.com/TickLabVN/biopass/blob/main/docs/PAM.md)

Le fichier **/usr/share/pam-configs/biopass** contient la configuration basique nécessaire à PAM:

    Name: Biopass - face and fingerprint authentication
    Default: no
    Priority: 260
    Auth-Type: Primary
    Auth:
      sufficient	libbiopass_pam.so

C'est assez parlant, sauf que ce fichier est pour les distributions de type Debian (et donc Ubuntu, Mint, ...) mais pas pour Redhat qui utilise **authselect**. Et la configuration authselect, c'est une autre paire de manche. Contrairement à ce que dit la documentation, on ne voit pas l'option sur Fedora, et donc, il faut comme pour howdy, y aller à la main. Je dois admettre que sur Redhat / Fedora, c'est plus compliqué que sur les distributions de type Debian.

Et donc, on peut soit tenter de modifier globalement la configuration pour tous les services (par exemple via les fichiers inclus dans les divers profils PAM), soit tester d'abord avec gdm comme avec Howdy.

On va essayer, en ajoutant dans **/etc/pam.d/gdm-password** la ligne en seconde position.

    auth sufficient libbiopass_pam.so

On redémarre le service:

```bash
sudo systemctl restart gdm
```

On sélectionne son utilisateur sur l'écran de login, la led clignote, et ça fonctionne.

Et là... On a exactement le même problème avec le trousseau de Gnome (keyring). Il faudra que je me penche sur le problème, plus tard avec une piste intéressante dans le point suivant.

# Changements généraux

Que ce soient Howdy ou Biopass, on peut appliquer les réglages PAM pour l'ensemble des composants nécessitant une authentification, par exemple sudo, login, etc. Sur Ubuntu et Debian, on peut par exemple ajouter la ligne dans system-auth, quasiment inclus dans toutes les configurations. Sur Redhat et Fedora c'est un peu plus casse-pied, car selon le service, ça peut être dans system-auth ou password-auth, et là encore, il faudrait que j'y passe du temps, et que je teste avec pamtester.

Concernant les trousseaux; que ce soient ceux de Gnome ou de KDE, le problème est le même pour Howdy et Biopass, et probablement d'ailleurs pour d'autres méthodes d'authentification. Sur les deux projets, des rapports (issues) sont ouverts, demandant de l'aide sur des solutions génériques. Par exemple [un test fou](https://gist.github.com/kizzard/166470fefe8fa64d2aa65e0235115318) propose l'utilisation du module TPM associé à un service utilisateur, pour déverrouiller automatiquement les trousseaux à la connexion. A tester.

# Conclusion

On a beau critiquer les OS propriétaires, mais leurs solutions fonctionnent généralement « out of the box ». Windows Hello, en tant que service biométrique, ne m'a jamais fait défaut. Les solutions comme Howdy et Biopass fonctionnent après de nombreuses étapes manuelles, car pour le moment les distributions n'incluent pas ces mécanismes d'authentification par défaut, et ne proposent aucune méthode d'automatisation. Il y a là un clair retard par rapport à la concurrence. Cependant, Biopass montre ce que peut être, et sera, si le soufflé ne retombe pas, l'intégration biométrique sous Linux : simple, efficace, pratique.

