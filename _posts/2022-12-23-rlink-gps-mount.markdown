---
layout: default
title:  "Renault et la carte R-Link 2023, la boulette"
date:   2022-12-23 01:00 +0100
description: "Les systèmes embarqués sur véhicules peuvent s'avérer problématiques. Ici il s'agira d'un témoignage de Usawa sur Renault, à l’origine de cette dépêche."
categories: 
- gps

---

Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

Publié aussi sur LinuxFR.

Les systèmes embarqués sur véhicules peuvent s'avérer problématiques. Ici il s'agira d'un témoignage de [Usawa dans son journal](https://linuxfr.org/redirect/111193) sur Renault, à l’origine de cette dépêche. 

[Page Wikipedia sur Renault R-Link](https://fr.wikipedia.org/wiki/Renault_R-Link)
[FAT32 documentation](http://elm-chan.org/docs/fat_e.html)
[Page Wikipedia du BIOS parameter block](https://en.wikipedia.org/wiki/BIOS_parameter_block)
[Microsoft FAT32 specifications](https://academy.cba.mit.edu/classes/networking_communications/SD/FAT.pdf)
[Précédent journal de l'auteur sur le même sujet](https://linuxfr.org/users/usawa/journaux/tomtom-sdcard-et-systeme-embarque-acceder-au-systeme-de-fichiers)

- [R-Link, Renault et moi, une histoire d’amour](#r-link-renault-et-moi-une-histoire-damour)
- [Ça marche, mais en vrai ça marche pas](#ça-marche-mais-en-vrai-ça-marche-pas)
- [Débogage](#débogage)
- [On tâtonne](#on-tâtonne)
- [Et ça fonctionne, mais, m’enfin ???](#et-ça-fonctionne-mais-menfin-)


# R-Link, Renault et moi, une histoire d’amour

Lors d’une précédente aventure, je vous avais expliqué comment j’ai pu, grâce à Linux, accéder au contenu de la carte SD contenant la carte de mon GPS embarqué, un Renault R-Link Evolution, basé sur Tomtom. Avec ça, j’ai pu ajouter, durant deux ans, des points d’intérêt, ce que l’appli fournie par Renault ne permet pas.  Las de ne pas disposer de la dernière mise à jour de carte, malgré un abonnement chèrement payé, j’ai acheté la dernière version sur une nouvelle carte SD, fournie par Renault.


Je passerai sur son tarif français de 50 euros, alors qu’il est de moins de 35 euros dans d’autres pays européens, tant nous sommes habitués, en tant que français, à être des vaches à lait. Je passerai aussi sur le fait que la carte SD datant du mois de juin propose une carte 10.85 datant de février, mais que le service de mise à jour en ligne ne propose qu’une version 10.75 datant d’août 2021. Très sympa quand on a payé 70 euros l’année de forfait pour les mises à jour et les services d’info trafic, sans recevoir une seule mise à jour, donc.


# Ça marche, mais en vrai ça marche pas

Tout heureux, la carte fonctionne bien sur le système R-Link, ça navigue. Chouette, je vais pouvoir ajouter mes points d’intérêt, et vérifier s’il y a des mises à jour. Malheur… Comme l’appli de Renault ne fonctionne que sur Windows (et MacOS, mais bon, j’aime le risque), j’insère la carte SD dans ma machine, et Windows me demande si je veux formater le disque ! Mais non ?! L’appli Renault ne détecte pas la carte. Aucune mise à jour possible, aucun ajout de nouveaux composants possible. 

Je suis un peu borné : c’est une carte SD officielle, elle fonctionne sur le R-Link (basé sur du Linux), c’est en principe un système de fichiers FAT32 qui contient plusieurs fichiers formant RAID Linear, ça doit fonctionner. Je passe sous Linux, pensant que la partition était passée en ext4. Que nenni ! Linux détecte automatiquement la partition et monte le système de fichiers qui est bien en FAT32. Je retrouve mes petits fichiers. Mais alors, quel est le problème ?

Avant que vous vous demandiez pourquoi je ne prends pas une autre carte SD pour y recopier les fichiers, ce n’est pas possible: le système vérifie le CID de carte SD, qui est liée à la carte tomtom (mais aussi au numéro de série de la voiture, inscrit à la première insertion de la carte). Il faudrait disposer d’une carte SD dont le CID peut être modifié, mais aussi d’un adaptateur spécial, car la manipulation n’est pas possible avec un adaptateur USB. Et si vous vous demandez pourquoi je n’ai pas juste reformaté et remis la carte : je me suis méfié, peut-être que la partition et le système de fichiers nécessitent des paramètres particuliers ? (non, mais je ne le savais pas encore) Il va falloir plonger les mains dans la mécanique.

# Débogage

Première étape, un petit ```fdisk -l```, qui ne montre rien de spécial, c’est une table MBR, compatible DOS, avec une partition de type c pour Windows 95 FAT32 LBA. Classique.

```
Disque /dev/sdc : 15,23 GiB, 16357785600 octets, 31948800 secteurs
Disk model: Multiple Reader
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 512 octets
taille d’E/S (minimale / optimale) : 512 octets / 512 octets
Type d’étiquette de disque : dos
Identifiant de disque : 0xa5eb573a

Périphérique Amorçage Début      Fin Secteurs Taille Id Type
/dev/sdc1    *         2048 31948799 31946752  15,2G  c W95 FAT32 (LBA)
```

Je passe à l’analyse du contenu du block device, qui révèle des surprises: table de partition invalide ? Mauvais offset ? Mais qu’est-ce qu’ils ont pu bien utiliser pour pondre un truc pareil ?

```bash
$ sudo file -s /dev/sdc
/dev/sdc: DOS/MBR boot sector MS-MBR Windows 7 english at offset 0x163 "Invalid partition table" at offset 0x17b "Error loading operating system" at offset 0x19a "Missing operating system", disk signature 0xa5eb573a; partition 1 : ID=0xc, active, start-CHS (0x0,32,33), end-CHS (0x3ff,254,63), startsector 2048, 31946752 sectors
```
Je fais la même chose sur le block device de la partition sdc1, et je suis assez surpris par les divers paramètres du système de fichiers : OEM-ID, Media descriptor, les diverses configurations des secteurs, et notamment les reserved sectors qui ne semblent pas suivre l’alignement. Là encore, il semblerait qu’un simple mkfs sous Linux, ou bouton droit+formater aurait un peu simplifié l'affaire.


```bash
$ sudo file -s /dev/sdc1
/dev/sdc1: DOS/MBR boot sector, code offset 0x58+2, OEM-ID "MSDOS5.0", sectors/cluster 64, reserved sectors 394, Media descriptor 0xfa, sectors/track 63, heads 255, hidden sectors 2048, sectors 31946752 (volumes > 32 MB), FAT (32 bit), sectors/FAT 3899, serial number 0xc9fd98, unlabeled
```

# On tâtonne #

Avant de continuer, j’effectue une sauvegarde complète de la carte avec un dd et je copie aussi les fichiers contenus sur la partition. J’utiliserai losetup ensuite sur le fichier image pour le lier à un block device à des fins d’analyse. J’aurai ainsi la possibilité de revenir en arrière si besoin.

Je détruis et recrée la table de partition. Sous fdisk, ça se fait avec la lettre o, pour recréer une table DOS (MBR, donc). Je crée ensuite une partition normale de même type, je passe ici sur les commandes, rien de spécial. Puis, je retente de créer exactement le même système de fichiers FAT32 selon les paramètres trouvés précédemment. J’en arrive au final, à cette commande :

```bash
$ sudo mkfs -t vfat -s 64 -R 394 -g 255/63 -i c9fd98 -a -M 0xfa /dev/sdc1
```

Bon, aucun souci de détection niveau kernel, ça monte bien sous Linux, je repasse sous Windows : il ne détecte rien, et propose de formater. Mais quoi ? Alors certes :

* Le paramètre -M permet de modifier le BPB_Media: les valeurs autorisées sont 0xF0, 0xF8 à 0xFF. La valeur standard est 0xF8 pour les périphériques fixes, et 0xF0 pour les amovibles, mais la référence dit que ce n’est plus utile ou très important, une histoire de compatibilité MSDOS 1.0…
* Le paramètre -a permet de ne pas tenir compte de l’alignement des secteurs. Avantage: on gagne de la place. Inconvénient, ça ralentit considérablement les performances des périphériques de type flash. Pas malin.
* Le paramètre -i est l’ID du volume, je conserve le même, si jamais le programme de protection vérifie ça.
* Le nombre de secteurs par cluster est à 64, ce n’est pas courant, mais je pense à un alignement quelconque.
* Le nombre de secteurs réservés, 364, semble trop important.
* La géométrie n’est pas cohérente avec la structure de la carte SD, mais ça ne devrait pas gêner le fonctionnement.
* Il est impossible avec les outils classiques (mkfs.vfat…) de modifier l’OEM-ID. C’est par défaut le nom du programme qui a créé le système de fichiers. Pourtant, après avoir lu quelques docs, ça a longtemps été un paramètre problématique. Ce ne sera pas le cas ici.

# Et ça fonctionne, mais, m’enfin ???

Je change des paramètres, et je finis par trouver que Windows ne comprend pas la valeur du BPB_Media. Au final, je crée le système de fichiers en remettant l’alignement (on vire le -a), et en spécifiant 0xf8. Et ça fonctionne ! 


```bash
$ sudo mkfs -t vfat -s 64 -R 256 -g 255/63 -i c9fd98 -M 0xf8 /dev/sdc1
```

Je recopie mes fichiers, je les vois sous Windows, je retourne à ma voiture, la carte est bien chargée. Ouf ! Je reviens sous Windows, l’appli la détecte (mais pas de mise à jour disponible). Mais alors, quel bilan ? Il est double :

1. Renault fournit à ses clients des cartes SD ne pouvant pas être détectées par Windows à cause d’un souci sur les paramètres lors de la création du système de fichiers FAT32. Je me demande pourquoi ils se cassent les pieds avec des valeurs aussi compliquées et quel outil ils utilisent. Mais c’est une boulette. Mon petit doigt me dit qu'ils ne feront rien, en tout cas pour ceux qui n’ont pas eu de chance si une nouvelle version corrigée est diffusée.
2. Le pilote FAT32 de Windows ne reconnaît pas ses propres paramètres du standard FAT32 créé par Microsoft. C’est pas joli joli ça. J’ai vérifié dans la doc officielle, et ça aurait dû fonctionner. Sur ce point Linux est carré.

Au final, j’aurais pu me contenter de recréer un système de fichiers FAT32 avec les paramètres par défaut, et ça aurait très bien fonctionné.