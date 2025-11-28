---
layout: default
title:  "Monter un serveur Webdav pour partager ses fichiers multimedia"
date:   2025-11-28 17:30:00 +0100
description: Si les lecteurs multimédias comme Kodi ou VLS peuvent accéder à des partages SMB (Windows), certains boitiers souffrent de lenteur. Voici comment mettre en place un partage Webdav, utilisant le protocole HTTP(S). Et se tirer les cheveux (s'il en reste) avec Kodi.
categories: 
- webdav
---
Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

- [La question de l'accès aux médias locaux](#la-question-de-laccès-aux-médias-locaux)
  - [Problématique](#problématique)
  - [Webdav](#webdav)
  - [Cahier des charges](#cahier-des-charges)
- [Configuration](#configuration)
  - [Installation initiale](#installation-initiale)
  - [Le firewall](#le-firewall)
  - [Configuration initiale](#configuration-initiale)
  - [Points de montage en lecture seule](#points-de-montage-en-lecture-seule)
  - [Configuration de Webdav](#configuration-de-webdav)
  - [Ajout du HTTPS](#ajout-du-https)
  - [Un certificat autosigné](#un-certificat-autosigné)
  - [Configuration Apache](#configuration-apache)
  - [Ajout du certificat dans le keystore](#ajout-du-certificat-dans-le-keystore)
    - [Windows](#windows)
    - [Linux](#linux)
    - [Android](#android)
- [Kodi](#kodi)
  - [En HTTP](#en-http)
  - [En HTTPS](#en-https)
  - [Plan B](#plan-b)
  - [Performances](#performances)
- [Bonus, montage sous Linux et Windows](#bonus-montage-sous-linux-et-windows)
  - [Linux](#linux-1)
  - [Windows](#windows-1)
- [Le mot de la fin](#le-mot-de-la-fin)

# La question de l'accès aux médias locaux

## Problématique

La plupart des téléviseurs multimédias et des boitiers de streaming, qu'ils soient sous Tizen, Android / Google TV, ou Apple TV, permettent de diffuser le contenu proposé par les diverses plateformes de streaming comme Netflix (ou autres) via des applications dédiées, mais proposent aussi, soit par une application intégrée, soit par des applications tierces, d'accéder à des médias (vidéos, images, son) locaux. Si les supports de stockage directement branchés en USB sont souvent bien reconnus (moyennant le type de système de fichiers), uPnP/DNLA est généralement proposé de facto. Il est parfois possible d'utiliser des partages de fichiers, depuis un NAS ou une machine de votre réseau local. C'est souvent SMB qui est mis en avant, mais il en existe d'autres, qui viennent avec leurs qualités mais aussi leurs défauts. 

J'ai un "faux" NAS, dans le sens où j'ai installé Ubuntu Linux 24.04 LTS sur un vieux (8 ans) Mac Mini (merci Apple et son obsolescence programmée, qui réussit à faire bien pire que Microsoft en arrêtant les mises à jour de MacOS sept ans après la sortie de ses machines). J'ai rattaché deux disques externes de 4 To en USB 3.0 sur ce Mac Mini. Côté configuration système et logicielle :

*  Les disques sont montés dans deux sous-répertoires du dossier /media, MacMiniNas1 et MacMiniNas2.
*  Ces deux répertoires sont partagés individuellement via Samba, afin de pouvoir y copier mes médias depuis une autre machine du réseau.
*  Un serveur Plex diffuse le contenu, optionnellement en DNLA.

Ça fonctionne bien, dans 95% des cas. Quid des 5% ? La lecture des fichiers **m2ts** (Blu-Ray, non reconnus par Plex), via les partages SMB, est quasiment impossible, notamment sur du 4K avec du DTS, sur mon Amazon Fire TV Cube. Que ce soit via **VLC** ou **Kodi**, et sans que j'en comprenne la raison, ça rame. Certes un fichier de test de 69 secondes prend 425 Mio, soit 6.15 Mio/s, mais les tests de débit en wifi sont excellents, de l'ordre de 70 Mio/s, la lecture depuis un quelconque autre PC (Windows, Linux) de la maison est parfaite, et les mêmes fichiers déposés sur un support USB sont lus impeccablement. Je sais que beaucoup de forums conseillent d'utiliser un adaptateur Ethernet USB (qui sera limité à 350 Mbit/s de par l'USB 2.0, donc je ne vois pas ce que ça changerait). Les moyens de débogage sur un Fire TV Cube sont limités, et je n'ai pas envie de passer des heures à comprendre pourquoi sans avoir de moyen de réparer de toute façon. Et puis mon petit doigt me dit que le souci vient de Kodi. Différentes réponses au même problème, dans les forums, proposent d'utiliser Webdav. Pourquoi pas ? 

## Webdav

**Webdav** *Web Distributed Authoring and Versioning* est une extension du protocole HTTP (il lui rajoute des méthodes), proposée via l'IETF et décrite par le [RFC 4918](https://datatracker.ietf.org/doc/html/rfc4918) (bonne lecture, ça fait 130 pages). Webdav permet à plusieurs utilisateurs de manipuler des fichiers sur un serveur Web. Autrement dit, c'est un protocole de gestion et de partage de fichiers basé sur le protocole HTTP. Un serveur Web va permettre d'accès et la manipulation distantes de fichiers, presque comme un système de fichiers normal. Il y a des avantages et des inconvénients, mais pour l'usage que je souhaite, je ne conserve que les avantages en limitant les inconvénients. Les avantages seront :

* La vitesse. Le protocole reste simple et servi via du HTTP, bien plus léger.
* Pas de gestion complexe des droits, des signatures, de l'encodage (chiffrage, sauf si HTTPS).
* Une mise en place simple via le serveur HTTP.

## Cahier des charges

Je me bornerai ici à décrire une implémentation simplifiée et suffisante :

1. Partage du ou des dossiers contenant mes médias.
2. Protection par mot de passe, un seul utilisateur.
3. Protection en écriture.
4. Si possible, utilisation de HTTPS

L'installation du serveur Webdav se fera sur mon mac mini, appelé slymini, sous Ubuntu 24.04 LTS. Toutes les manipulations peuvent théoriquement être effectuées sur les distributions Debian ou dérivées d'Ubuntu, comme Linux Mint.

# Configuration

## Installation initiale

Apache s'installe très simplement :

```bash
seb@slymini:~$ sudo apt install apache2
```

On active le service pour le rendre persistant au redémarrage du serveur :

```bash
seb@slymini:~$ sudo systemctl enable apache2
```

## Le firewall

J'utilise UFW. Cette section est inutile si vous n'utilisez pas de firewall dans votre réseau personnel. Mais ce serait une bonne idée. On autorise les ports http (80) et https (443) :

```bash
seb@slymini:~$ sudo ufw allow http
seb@slymini:~$ sudo ufw allow https
```

## Configuration initiale

On crée le répertoire qui va contenir le site, ainsi qu'une page basique pour le test.

```bash
seb@slymini:~$ sudo mkdir -p /var/www/webdav
seb@slymini:~$ sudo sh -c 'echo "Bienvenue sur mon serveur WebDAV" > /var/www/webdav/index.html'
seb@slymini:~$ sudo chown -R www-data:www-data /var/www/webdav
```

Maintenant, on crée la configuration initiale pour Apache. On crée le fichier **/etc/apache2/sites-available/webdav.local.conf** :

```bash
seb@slymini:~$ sudo /etc/apache2/sites-available/webdav.local.conf
```

On y place le contenu suivant :

```apacheconf
ServerTokens Prod
ServerSignature Off

<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        ServerName slynet.net

        DocumentRoot /var/www/webdav
        <Directory />
                Options FollowSymLinks
                AllowOverride None
        </Directory>
        <Directory /var/www/webdav/>
                Options Indexes FollowSymLinks MultiViews
                AllowOverride None
                Order allow,deny
                allow from all
        </Directory>
</VirtualHost>
```

Pour les explications détaillées, vous pouvez vous reporter à la documentation officielle d'Apache. Cette configuration crée simplement un site HTTP non chiffré, port 80, qui sera contenu dans **/var/www/webdav**, accessible à tous.

Petit aparté sur deux valeurs :

* **ServerTokens** : Une bonne pratique en production peut être de cacher la version du serveur http, dans le header. Ici, ce sera limité à « Apache ».
* **ServerSignature** : La ligne de pied de page par défaut est supprimée.

On active le site et on recharge la configuration d'Apache. Il faudra la recharger souvent par la suite, donc je ne l'indiquerai pas systématiquement :

```bash
seb@slymini:~$ sudo a2ensite webdav.local
seb@slymini:~$ sudo systemctl reload apache2
```

L'IP de mon serveur est 192.168.1.250. Si vous obtenez comme moi ceci depuis mon navigateur sur https://192.168.1.250, alors c'est bon :

![Ma page par défaut](/assets/img/2025-11-28/test-http-1.jpg)

## Points de montage en lecture seule

Cette partie devra être adaptée selon votre configuration. Mes disques sont montés sur **/media/MacMiniNas{1,2}** et accessibles en lecture/écriture via le groupe "media". 

```bash
seb@slymini:~$ mount|grep -i mac
/dev/sdc1 on /media/MacMiniNas1 type ext4 (rw,relatime)
/dev/sdb1 on /media/MacMiniNas2 type ext4 (rw,relatime)
```

Les médias seront accessibles via https://192.168.1.250/media/, et ce répertoire sera dans **/var/www/webdav/media**.

On veut les rendre accessible en lecture seule depuis le serveur Apache, mais qu'ils restent accessibles en écriture depuis le serveur lui-même et le partage SMB déjà en place. Il y a plusieurs solutions :

* S'arranger avec les droits des groupes et d'apache.
* Adapter la configuration d'Apache pour interdire l'écriture en bloquant certaines méthodes HTTP selon l'utilisateur.
* Interdire l'écriture via Linux et les options de montage.

Pour autoriser Apache à lire (et écrire) sur mes disques, on ajoute **www-data** dans le groupe **media**, qui a les droits de lecture et écriture sur mon arborescence de médias :

```bash
seb@slymini:~$ sudo usermod -a -G media www-data
seb@slymini:~$ id www-data
uid=33(www-data) gid=33(www-data) groups=33(www-data),5000(media)
```

Pour interdire l'accès en écriture, on va bloquer les écritures au niveau des points de montage. Mais les disques sont déjà montés en écriture, et je ne veux pas interdire l'écriture via mon utilisateur local et via SMB ! Alors on utilise un mode de montage bien pratique, le **bind mount** : il autorise le montage d'un répertoire, ou un système de fichiers déjà monté, à un autre endroit de l'arborescence, avec d'autres options, comme par exemple la lecture seule.

On crée les nouveaux points de montage :
```bash
seb@slymini:~$ sudo mkdir -p /var/www/webdav/media/MacMiniNas{1,2}
seb@slymini:~$ sudo chown -R www-data:www-data /var/www/webdav/media
```

On rajoute deux entrées dans **/etc/fstab** :

```
/media/MacMiniNas1      /var/www/webdav/media/MacMiniNas1 none  bind,ro 0 0
/media/MacMiniNas2      /var/www/webdav/media/MacMiniNas2 none  bind,ro 0 0
```

Le système de fichiers (ou le répertoire) /media/MacMiniNas1 sera aussi accessible via /var/www/webdav/media/MacMiniNas1, mais en lecture seule.

> **NOTE**
> J'ai rencontré des effets bizarres lors de mes tests, notamment via mount --bind qui m'ont forcé à redémarrer : disparition de mes points de montage initiaux. 

On remet à jour systemd (l'une des raisons qui font que systemd m'énerve parfois) et on monte les répertoires. Un petit test s'impose :

```bash
seb@slymini:~$ sudo systemctl daemon-reload
seb@slymini:~$ mount -a
seb@slymini:~$ mount|grep media
/dev/sdc1 on /media/MacMiniNas1 type ext4 (rw,relatime)
/dev/sdc1 on /var/www/webdav/media/MacMiniNas1 type ext4 (ro,relatime)
/dev/sdb1 on /media/MacMiniNas2 type ext4 (rw,relatime)
/dev/sdb1 on /var/www/webdav/media/MacMiniNas2 type ext4 (ro,relatime)
seb@slymini:~$ df -a |grep media
/dev/sdc1      3844549616 1686048120 1963134280  47% /media/MacMiniNas1
/dev/sdc1      3844549616 1686048120 1963134280  47% /var/www/webdav/media/MacMiniNas1
/dev/sdb1      3844549616 2869267932  779914468  79% /media/MacMiniNas2
/dev/sdb1      3844549616 2869267932  779914468  79% /var/www/webdav/media/MacMiniNas2
seb@slymini:/var/www/webdav/media/MacMiniNas1$ touch toto
touch: cannot touch 'toto': Read-only file system
seb@slymini:/var/www/webdav/media/MacMiniNas1$ cd /media/MacMiniNas1
seb@slymini:/media/MacMiniNas1$ touch toto
seb@slymini:/media/MacMiniNas1$ ls toto
toto
```

C'est tout bon !

## Configuration de Webdav

On retourne à la configuration du serveur Apache. On active le module Webdav :

```bash
seb@slymini:~$ sudo a2enmod dav_fs
``` 

Je souhaite que mon partage Webdav ne soit accessible que via un mot de passe, donc je veux que quand j'accède à /media, une authentification me soit demandée. Pour ça, il faut créer un fichier de mots de passe. Deux méthodes d'authentification sont possibles (en fait bien plus, mais on ne va commencer à implémenter de l'OAuth ou du Kerberos) :

1. **Basic** : les informations ne sont pas chiffrées (du simple Base64). Pas conseillé pour du http, mais OK pour du https
2. **Digest** : les informations sont encodées sous forme de hash MD5 (oui, oui...) basé sur le nom d'utilisateur, le « Realm », le mot de passe, et un « nonce » (une chaine fournie par le serveur) qui empêche théoriquement la réutilisation de la chaine d'authentification finale. C'est mieux dans tous les cas. C'est ce qu'on va utiliser.

On active le module d'authentification, et on crée un fichier contenant l'utilisateur et son mot de passe :

```bash
seb@slymini:~$ sudo a2enmod auth_digest
seb@slymini:~$ sudo htdigest -c /usr/local/apache2/webdav.passwords "Slymini Media Server" macmininas1
```

On ajoute la configuration Webdav et d'authentification dans le fichier **/etc/apache2/sites-available/webdav.local.conf**. Veillez bien à ce que la valeur de **AuthName** soit identique à celle passée à la commande **htdigest** :

```apache
Alias /media /var/www/webdav/media
<Location /media>
    DAV On
    AuthType Digest
    AuthName "Slymini Media Server"
    AuthUserFile /usr/local/apache2/webdav.passwords
    Require valid-user
</Location>
```

On relance Apache, et on teste sur http://192.168.1.250/media . Si tout va bien, ça va afficher ceci :

![Demande d'authentification](/assets/img/2025-11-28/test-webdav-1.jpg)

On saisit l'utilisateur et le mot de passe, et normalement, on a accès au partage dans lequel on peut naviguer :

![navigation webdav](/assets/img/2025-11-28/test-webdav-2.jpg)

## Ajout du HTTPS

Cette partie, c'est du bonus. L' « *encryption in transit* », soit le chiffrage du flux, est une mesure de sécurité de base sur un réseau public, on y pense moins souvent sur un réseau interne personnel.

## Un certificat autosigné

> **NOTE**
> Certains vont se dire, mais pourquoi ne pas utiliser **Let's Encrypt** ? Tout simplement parce que Let's Encrypt est pour les sites Web publics, et qu'il vérifie la présence d'un enregistrement DNS public. 
> Vous trouverez sur [let's Encrypt](https://letsencrypt.org/docs/certificates-for-localhost/) quelques détails à ce propos.

On va tout d'abord créer un certificat autosigné, c'est-à-dire qu'il ne sera pas signé par une autorité de certification reconnue par les systèmes d'exploitation et les applications. Ça suffit largement pour un réseau interne. Mais ça va poser quelques problèmes plus tard.

```bash
seb@slymini:~$ mkdir certificate && cd certificate
seb@slymini:~/certificate$ openssl req -nodes -x509 -sha256 -newkey rsa:4096 -keyout slynet.key -out slynet.crt -days 3650 -subj "/C=FR/ST=IDF/L=Paris/O=Slynet/OU=Slynet/CN=slynet.net" -addext "subjectAltName = IP:192.168.1.250,DNS:slynet.net,DNS:www.slynet.net"
seb@slymini:~/certificate$ ls
slynet.crt  slynet.key
```

Quelques détails importants :

* Le sujet **subj** : la seule valeur réellement importante est la valeur CN, qui doit correspondre au nom de votre site. 
* Les noms alternatifs, **subjectAltName** :
  * On met autant d'entrées **DNS** qu'on utilise de noms, FQDN. Un wildcard est aussi possible.
  * Si on accède au partage via une adresse IP, on ajoute un enregistrement **IP**.

On copie les deux fichiers obtenus : une clé privée et un certificat, dans un répertoire **/etc/apache2/ssl**. Notez que la clé ne doit pas être lisible publiquement. 

```bash
seb@slymini:~/certificate$ sudo mkdir /etc/apache2/ssl
seb@slymini:~/certificate$ sudo cp slynet.* /etc/apache2/ssl
seb@slymini:~/certificate$ ls -l /etc/apache2/ssl
-rw-r--r-- 1 root root 2082 nov.  27 07:21 slynet.crt
-rw------- 1 root root 3272 nov.  27 07:21 slynet.key
```

> **NOTE**
> Si vous vous demandez pourquoi les fichiers appartiennent à root, rappelez-vous que les ports inférieurs à 1024, donc le port 443, ne peuvent être ouverts que par un processus démarré par root...

## Configuration Apache
On ajoute le module SSL :

```bash
seb@slymini:~$ sudo a2enmod ssl
```

La configuration de Apache pour HTTPS est quasiment identique, sauf qu'il va falloir dupliquer tout le bloc **VirtualHost** pour le port 443 et ajouter la configuration du certificat :

```apacheconf
ServerTokens Prod
ServerSignature Off

<VirtualHost *:443>
        ServerAdmin webmaster@localhost
        ServerName slynet.net

        DocumentRoot /var/www/webdav
        <Directory />
                Options FollowSymLinks
                AllowOverride None
        </Directory>
        <Directory /var/www/webdav/>
                Options Indexes FollowSymLinks MultiViews
                AllowOverride None
                Order allow,deny
                allow from all
        </Directory>

        SSLEngine on
        SSLCertificateFile    /etc/apache2/ssl/slynet.crt
        SSLCertificateKeyFile /etc/apache2/ssl/slynet.key
</VirtualHost>
```

Il faut être malin : on remarque que cette configuration comporte une partie strictement identique au http classique. On peut donc couper cette configuration en deux et placer les morceaux dans deux fichiers qu'on inclura dans la configuration principale. Je vous laisse deviner quelles lignes placer dans chacun des fichiers, ce n'est pas bien compliqué.

```apacheconf
<VirtualHost *:443>
        include /etc/apache2/webdav_common.conf
        include /etc/apache2/webdav_ssl.conf
</VirtualHost>

<VirtualHost *:80>
        include /etc/apache2/webdav_common.conf
</VirtualHost>
```

On redémarre Apache :

```bash
seb@slymini:~$ sudo systemctl restart apache2
```

Et ... On teste ? Non ! Il manque une petite étape. Comme le certificat est autosigné, il n'est pas reconnu par nos systèmes d'exploitation, quel qu'il soit. Tout accès au partage Webdav produira au mieux un avertissement « Ce site n'est pas sécurisé », au pire une erreur. Il faut ajouter le certificat dans une base de données spécifique, dont les noms et formats diffèrent selon le système d'exploitation, voire pour chaque application (par exemple, les keystores de Java). On appelle généralement ça un **trousseau**.

## Ajout du certificat dans le keystore

### Windows

Mon serveur est sous Linux, mais mon PC est sous Windows (oui, je suis un traitre à la cause, et je m'en fiche). Donc, on va commencer par là. Le gestionnaire de certificats de Windows stocke une arborescence de clés pour divers usages. Notre certificat doit être importé comme « **Autorité de certification racine de confiance** ». 

1. On récupère le fichier slynet.crt, qu'on place par exemple sur le bureau. On peut aussi le récupérer directement depuis le navigateur web en l'enregistrant localement.
2. On double-clique dessus, et on clique sur « **Installer un certificat ...** »
3. Windows propose de l'installer pour tout le monde, ou pour soi-même. On choisit ce qu'on veut.
4. On place le certificat dans le magasin « **Autorités de certification racines de confiance** ».
5. On dit oui, suivant, blablabla, et c'est bon. Plus d'erreur ! 

![navigation webdav](/assets/img/2025-11-28/windows-certmgr.jpg)

### Linux

On critique Windows mais sous Linux ce n'est pas forcément aussi simple. D'autant plus que ça varie selon les distributions, les environnements de bureau, ... Alors, **keyring** sous GNOME, **wallet** sous KDE. Non mais, et après on se demande pourquoi Linux ne perce pas sur le marché du poste de travail !

Alors on va parler seulement de la méthode globale sous Ubuntu (et dérivés), disponible [ici](https://documentation.ubuntu.com/server/how-to/security/install-a-root-ca-certificate-in-the-trust-store/). Et le certificat sera valable pour tout le système. Ce n'est pas toujours l'idéal.

1. On copie le certificat dans **/usr/local/share/ca-certificates**
2. On met à jour
3. On vérifie

```bash
seb@slymini:~/certificate$ sudo cp slynet.crt /usr/local/share/ca-certificates/
seb@slymini:~/certificate$ sudo update-ca-certificates
Updating certificates in /etc/ssl/certs...
rehash: warning: skipping ca-certificates.crt,it does not contain exactly one certificate or CRL
1 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
Traitement des actions différées (« triggers ») pour ca-certificates-java (20240118) ...
Adding debian:slynet.pem
done.
done.
seb@slymini:~/certificate$ ls -l /etc/ss
ssh/ ssl/
seb@slymini:~/certificate$ ls -l /etc/ssl/
certs/       openssl.cnf  private/
seb@slymini:~/certificate$ ls -l /etc/ssl/certs/slynet.pem
lrwxrwxrwx 1 root root 43 nov.  27 19:49 /etc/ssl/certs/slynet.pem -> /usr/local/share/ca-certificates/slynet.crt
```

### Android

La méthode diffère selon les constructeurs, les versions d'Android et les surcouches. De manière globale, il faut aller dans les **Paramètres** et rechercher **Certificat**. Sur les Samsung, ça se présente comme sur la capture suivante, avec AC pour **Autorité de Certification** (donc CA).

![Certificat sous Android](/assets/img/2025-11-28/samsung-certs.jpg)

Sur Amazon Fire TV, il n'y a pas de menu ! Il faut passer le boitier (ou la clé) en mode Développeur, puis activer le mode de débogage **adb**. Ça se fait assez simplement, la méthode est décrite sur le [site officiel de Fire OS](https://developer.amazon.com/docs/fire-tv/connecting-adb-to-device.html).

On installe les outils de base de la plateforme Android : 

```bash
seb@slymini:~/certificate$ sudo apt install android-platform-tools-base
```

Une fois en mode Debug, on peut utiliser la commande **adb** pour se connecter et envoyer le certificat. Il faut rester à côté du boitier pour valider les différentes actions sur l'écran !

1. On récupère l'IP de votre boitier ou clé, chez moi c'est 192.168.1.105
2. On se connecte avec adb connect. On valide sur l'écran
3. On dépose la clé dans le répertoire Downloads de la Fire TV.
4. On lance l'importation avec la commande adb shell d'import : il faudra confirmer votre compte Amazon pour ça (avec mot de passe, voire MFA, histoire de ne pas injecter une clé non-voulue), sur l'écran.

```bash
seb@slymini:~/certificate$ adb connect 192.168.1.105:5555
seb@slymini:~/certificate$ adb devices
List of devices attached
192.168.1.105:5555      device

seb@slymini:~/certificate$ adb push slynet.crt /storage/emulated/0/Download
seb@slymini:~/certificate$ adb shell am start -a "android.intent.action.VIEW" -d "file:///storage/emulated/0/Download/slynet.crt" -t "application/x-x509-ca-cert"
```

Le certificat est importé.

Sauf que... ça nous amène au cas de Kodi.

# Kodi

> **ASTUCE**
> Après avoir configuré le répertoire des captures d'écran dans Kodi, appuyez sur Ctrl+S pour prendre une capture, même avec un clavier Bluetooth sous Android.

## En HTTP

Sous Kodi, il suffit d'ajouter une source de type Webdav Server (HTTP) et de remplir les champs. Rien de compliqué.

![Kodi Webdav HTTP](/assets/img/2025-11-28/kodi-webdav-http.jpg)

Une fois la source ajoutée, on peut demander à l'indexer, selon son humeur. On peut ensuite naviguer et lancer ses médias. Et ça marche très bien, tant sous Linux, Windows, Android (incluant Fire TV) !

![Kodi Webdav HTTP](/assets/img/2025-11-28/kodi-webdav-browse.jpg)

## En HTTPS

C'est ici je j'ai perdu beaucoup de temps, et je vous cache toute ma frustration devant mes nombreux essais infructueux. J'ai beaucoup appris sur Kodi, et accessoirement un peu plus sur Android et Fire OS lors de cette étape. Kodi est un superbe logiciel libre, mais des fois, il est énervant. Et la gestion des certificats est un de ces points de très grosse frustration. Faisons court : il ne se base pas toujours sur les trousseaux du système d'exploitation, selon le système d'exploitation. Par défaut, toute tentative de passer sur un partage Webdav en HTTPS se solde par l'erreur suivante :

![Kodi Webdav HTTPS](/assets/img/2025-11-28/kodi-webdav-https.jpg)

Ce qui explique pourquoi tout le monde, ou presque :
1. Dit d'utiliser Webdav en HTTP
2. Râle sur les forums comme quoi HTTPS ne fonctionne pas.

> **NOTE**
> Sous Windows, vous pouvez modifier le fichier **C:\Program Files\Kodi\system\certs\cacert.pem.**
> Sous Linux, utilisez le trousseau système, comme vu précédemment.

La documentation trop succincte pour la gestion du SSL se trouve [ici](https://kodi.wiki/view/SSL_certificates).

En fait, c'est comme toujours un peu plus compliqué que ça en a l'air. Kodi se base sur **libcurl**, et selon son humeur du jour va utiliser le trousseau système (distribution Linux), ou sa propre base de certificats qui provient de Mozilla (Windows, Android) via un fichier **cacert.pem**. Pratique pour être compatible avec tout, ou pour n'être compatible qu'avec rien : sous Android Kodi installe ce fichier, et une fois installé, on ne peut pas le modifier, car il faut être root. Et donc, il faudrait débloquer, **rooter** l'appareil Android :

 ```bash
seb@slymini:~/certificate$ adb shell
1|gazelle:/data $ ls /data/data/org.xbmc.kodi/cache/apk/assets/system/certs
ls: /data/data/org.xbmc.kodi/cache/apk/assets/system/certs: Permission denied
```

On peut récupérer une copie du fichier [cacert.pem](https://curl.se/docs/caextract.html) sur le site de curl. Il va nous servir par la suite. On peut aussi le récupérer dans un répertoire de Kodi (mais il semble plus ancien).

La [documentation avancée](https://kodi.wiki/view/Advancedsettings.xml) de Kodi nous informe que depuis la version 19 (j'ai retrouvé les rapports initiaux où les utilisateurs pleuraient), on peut placer le fichier cacert.pem dans un autre endroit en créant un fichier **advancedsettings.xml** avec une entrée **catrustfile**.

Cette démarche montre que si on peut en effet régler Kodi très finement, de nombreuses options ne sont pas accessibles par l'interface graphique, et que bien qu'on puisse théoriquement tout faire depuis un smartphone ou une tablette (pensez à ajouter un clavier et une souris via USB ou Bluetooth, voire à brancher sur un vrai écran), un ordinateur peut être nécessaire.

Ce fichier être placé dans dans le profil utilisateur, accessible depuis le gestionnaire de fichiers intégré à Kodi. Le chemin s'appelle **special://profile** (Kodi fournit des [protocoles spéciaux](https://kodi.wiki/view/Special_protocol)). On peut aussi y placer le fichier cacert.pem.

On va donc créer, ou modifier un fichier déjà présent **advancedsettings.xml** :

```xml
<advancedsettings version="1.0">
  <network>
    <catrustfile>special://profile/cacert.pem</catrustfile>
   </network>
</advancedsettings>
```

On ajoute notre certificat à la fin du fichier cacert.pem : 

```bash
seb@slymini:~/certificate$ cat slynet.crt >> cacert.pem
```

Et on copie les deux fichiers dans le profil utilisateur de Kodi sur la box. Il y a deux solutions :

1. Mettre les fichiers sur une clé USB, ou un partage, ou les créer directement sur la box, et les copier via le gestionnaire de fichiers intégré à Kodi.
2. Passer par un shell et adb.

On va utiliser adb. Le répertoire utilisateur Android est dans **/sdcard/Android/data/org.xbmc.kodi/files/.kodi/userdata**.

```bash
seb@slymini:~/certificate$ adb push cacert.pem /sdcard/Android/data/org.xbmc.kodi/files/.kodi/userdata/cacert.pem
cacert.pem: 1 file pushed, 0 skipped. 149.8 MB/s (281034 bytes in 0.002s)
seb@slymini:~/certificate$ adb push advancedsettings.xml /sdcard/Android/data/org.xbmc.kodi/files/.kodi/userdata/advance
dsettings.xml
advancedsettings.xml: 1 file pushed, 0 skipped. 0.4 MB/s (79 bytes in 0.000s)
```

Et on teste. Et ça fonctionne.


## Plan B

Si ça vous semble compliqué, il y a un plan B ! Il n'a pas fonctionné chez moi sous Windows, mais il a fonctionné sur Android. Dans la doc, il existe une dernière chance : ajouter « **verifypeer=false** » à la fin de l'URL. Et ça marche... Tout ça pour ça (mais c'était instructif). C'est l'équivalent d'un « no ssl check » qui ne va pas vérifier si le certificat, et donc le site, est de confiance. Mais le flux réseau reste chiffré.

![Kodi Webdav HTTPS avec verifypeer](/assets/img/2025-11-28/kodi-webdav-https-2.jpg)

## Performances

Dans les deux cas, avec ou sans SSL, les performances de lecture avec Webdav sont excellentes. Comme vous le constatez sur la capture, la mise en cache en cours de lecture (barre blanche) d'un fichier **m2ts** (Blu-Ray), de 425 Mio et 68 secondes, Ultra HD et DTS:X, ne pose plus aucun problème.

![Kodi Webdav HTTPS avec verifypeer](/assets/img/2025-11-28/kodi-webdav-perfs.jpg)

# Bonus, montage sous Linux et Windows

## Linux

Un site Webdav peut être monté comme un système de fichiers. Installez le paquetage **davfs2**. On aura à répondre à une petite question concernant SUID, j'ai mis oui, afin qu'un simple utilisateur puisse monter un partage dans son arborescence, plus tard (mais pas ici). 

```bash
seb@slymini:~ $ sudo apt install davfs2
```

On monte le système de fichiers :

```bash
seb@slymini:~$ sudo mkdir -p /mnt/webdav
seb@slymini:~$ sudo mount -t davfs https://192.168.1.250/media /mnt/webdav
Please enter the username to authenticate with server
https://192.168.1.250/media or hit enter for none.
  Username: macmininas1
Please enter the password to authenticate user macmininas1 with server
https://192.168.1.250/media or hit enter for none.
  Password:
```

On vérifie :

```bash
seb@slymini:~$ cd /mnt/webdav/MacMiniNas1/Demos/Ultra\ HD\ DTS/
seb@slymini:/mnt/webdav/MacMiniNas1/Demos/Ultra HD DTS$ ls
00000.m2ts  00001.m2ts  00002.m2ts  00003.m2ts  00004.m2ts  00005.m2ts  00006.m2ts  00008.m2ts  99999.m2ts
```

Je vous renvoie sur cette superbe [documentation Arch](https://wiki.archlinux.org/title/Davfs2) (comme toutes les documentations Arch) qui explique comment automatiser le montage et le stockage des identifiants et mots de passe.

## Windows

Windows supporte nativement Webdav. Ça se fait en trois clics depuis l'explorateur de fichiers : bouton droit sur **Mon PC** ou **Réseau**, puis **Connecter un lecteur réseau...**. On saisit l'URL du partage Webdav, puis on saisit l'utilisateur et le mot de passe, et c'est fini.

![Webdav sous Windows](/assets/img/2025-11-28/windows-webdav.jpg)

# Le mot de la fin

La création d'un partage Webdav est assez simple, et a résolu mes problèmes de performances avec mon boitier de streaming, avec les contraintes que je m'étais données. Je ne m'attendais par contre pas à galérer autant avec la gestion des certificats sous Kodi et sous Android. Je ne dirais pas que ça a failli être un échec, mais que ça a failli ne pas fonctionner. Quand je pense que j'ai perdu une heure parce que j'avais oublié un tag xml qui était devant mon nez (chut, ne le dites pas, je vais perdre toute crédibilité). 

La rédaction de cet article a été très instructive. Je maitrisais déjà beaucoup de choses, comme la création et la gestion des certificats sous Linux et Windows, le montage Webdav, la mise en place d'un serveur Apache, les montages bind, etc. Mais je ne manipule pas souvent adb et je n'avais jamais tenté de personnalisation de Kodi ou manipulé Fire OS. Je pense vraiment qu'il y a encore du travail côté Android et Kodi pour simplifier l'installation et l'utilisation de certificats personnels ou de nouvelles autorités de certification.



