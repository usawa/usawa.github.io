---
layout: default
title:  "Le scanner hanté, wireshark et le wifi"
date:   2022-07-08 01:00 +0100
description: "Quand le scanner de l'imprimante démarre sans raison, il est temps d'appeler SOS Fantômes"
categories: 
- scanner

---

Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

Publié aussi sur LinuxFR.


Il m’est arrivé un truc. Dimanche dernier, le matin, j’allume la multiprise de mon petit espace perso, et ça démarre tout le bazar : écran, PC, imprimante. Je suis tranquille devant mon clavier quand **le démarrage intempestif du scanner de mon *HP Envy 6232* me fait sursauter**. C’est bizarre, tout le monde dort encore ici (dimanche, vacances scolaires, ados…). Je regarde sur mon PC, rien de spécial. Ca me perturbe un peu, mais bon, cette imprimante m'en a fait voir d'autres. 30 minutes après, rebelote. Là, je me dis que c’est vraiment bizarre. Et ça recommence, assez aléatoirement, plusieurs fois dans la journée.

Au risque de choquer les âmes Linuxiennes sensibles, mon PC perso tourne sous Windows 11, exclusivement, depuis le mois de mars, date de sa construction. Plus par paresse qu’autre chose, je n’ai pas encore eu le temps ni la volonté de remettre un Linux en double boot. J’aurais dû, ça m’aurait bien aidé. Mais bon, j’ai fait autrement.

À ce moment, je me demande si je n’ai pas à faire à un plaisantin, ou pire. Je coupe le Wifi Direct, sans effet. Je coupe le Bluetooth, pareil. Je coupe les services Web (ePrint, …), pas de changement. Je débranche le câble USB, idem. Je coupe le wifi, plus de numérisation fantôme. C’est donc ça, ça passe par le réseau. L'imprimante n'a pas de connexion Ethernet. Dommage. Sur mon Windows, je lance Wireshark. C’est là que la chasse devient intéressante. La chasse a duré quelques jours, quand j’avais le temps.

**Wireshark** me montre uniquement les communications de type mDNS (protocole Bonjour), IGMP, Netbios et IPP entre l’imprimante et mon PC, notamment des refreshs demandés par Windows toutes les 30 minutes, mais qui ne déclenchent pas le scan fantôme. L’imprimante étant en Wifi, le mode promiscuous ne fonctionne pas, logique.  J’ai basiquement deux réseaux Wifi configurés à la maison, qui correspondent aux deux bandes sur 2.4GHz et 5GHz. Si je veux trouver le coupable, il faut que je choppe les trames 802.11. Il faut donc passer l’interface wifi en mode Monitor. Sous Windows, c’est le pilote npcap qui permet ça. Ce n’est pas activé par défaut. Je réinstalle avec l’option activée, je coupe la connexion wifi, je lance wireshark, je vais sur la configuration des interfaces et… L’option est grisée. Eh oui, ça ne fonctionne pas avec tous les chipsets Wifi, dont le AX200. Dommage, mais je ne vais pas racheter un adaptateur Wifi compatible juste pour ça.

Je me décide à rallumer un vieux Nuc Intel de 10 ans, sous Ubuntu 21.10, que je mets à jour en 22.04. Je ne l’avais pas rallumé depuis 6 mois, et je voulais même le donner (j’en avais causé sur la tribune) à qui en voudrait. Personne n’en a voulu. Tant mieux, finalement. Avant même de lancer quoi que ce soit, je dois m’assurer que l’imprimante et le nuc sont sur le même SSID, et le même canal. Le nuc a un adaptateur wifi façon moyen âge, uniquement du wifi 802.11g, en 2.4 GHz, donc.

L’imprimante HP dispose d’un petit panneau tactile LCD noir et blanc. Je la configure sur le même SSID. Zut, l’imprimante se met sur le 5GHz. L’imprimante dispose d’une interface web accessible via son IP (et sans mot de passe, sympa la sécurité). Je trouve le réglage et je force le passage en 2.4GHz. Si j'éteins, l’imprimante repasse en 5GHz. Pourtant le réglage dans l’interface web indique 2.4GHz, c’est un bug. Je trouve le réglage sur le panneau LCD, hop c’est bon. Les deux réglages ne sont pas synchros entre le panneau lcd et l’interface web. Dommage.

On passe sur Ubuntu. Un **iwlist** confirme que la carte supporte le mode monitor.

```bash
$ iwlist
        Supported interface modes:
                 …
                 * monitor
                 …
```

On lance un scan pour isoler le canal. Bon, ça aurait été pareil depuis l’interface de l’imprimante, mais c’est plus fun sous Linux.

```bash
$ iwlist wlp2s0 scan
wlp2s0    Scan completed :
          Cell 01 - Address: B9:77:58:XX:XX:XX
                    Channel:1
                    Frequency:2.412 GHz (Channel 1)
                    Quality=44/70  Signal level=-66 dBm
                    Encryption key:on
                    ESSID:"MON_SSID"
```

Ce sera donc le canal 1. On stoppe la connexion wifi. Je l’ai fait depuis l’interface Gnome, c’est aussi simple. Ensuite, on passe l’adaptateur en mode monitor :

```bash
$ sudo iwconfig wlp2s0 mode monitor
```

On active manuellement l’adaptateur :

```bash
$ sudo rfkill unblock 1
```

On active le lien sur l’interface :

```bash
$ Sudo ip link set wlp2s0 up
```

Et on commute sur le bon canal :

```bash
$ sudo iwconfig wlp2s0 channel 1
```

Là, on a l’interface en mode monitor, sur le canal 1. On lance wireshark, et la capture sur l’interface wifi.  A mon premier essai, j’ai vu en effet passer des centaines de trames 802.11 y compris lors de l’activité fantôme. C’est cool mais j’avais juste oublié un truc : le décodage. Zut. C’est dans **Preferences->Protocols->IEEE 802.11** (pourquoi ce n'est pas dans 802.11 tout court ? Aucune idée). On coche **Enable decryption**, on ajoute la clé liée au SSID au format: `password:SSID` 

![Il faut donner les clés de son wifi à Wireshark](/assets/img/2022-07-08/wireshark-pwd.png)

On relance. Arf, toujours pas de décodage. Ah oui, il n’a pas encore choppé la resynchronisation entre l’imprimante et le point d’accès. Allez, j’éteins et je rallume l’imprimante. C’est mieux ! J’ai les trames 802.11, les paquets TCP, ça décode. J’attends. Et après moins de 25 minutes, le scanner hanté est de retour. Cette fois, mon petit, je t’ai eu, j’ai capturé la transaction ! Oh, bonjour 192.168.1.16, qui es-tu ?

![Wireshark décode les trames 802.11 et c'est cool](/assets/img/2022-07-08/wireshark-capture.png)

C’est un ordinateur portable Apple, sous MacOS. Arf… L’ordi a été remis en charge et rallumé dimanche dernier après plusieurs semaines (ou mois ?) sans activité ! Lorsqu’il avait été éteint, correctement, la case « *relancer automatiquement toutes les applis ouvertes…* » était cochée. À la reconnexion, MacOS a donc relancé toutes les applis, dont l’outil de numérisation intégré (qui est très pratique, il faut l’avouer). Et c’est ainsi qu’aléatoirement, probablement à chaque réveil (ordinateur en veille, verrouillé, écran rabattu), Monsieur MacOS décidait de lancer une numérisation d’aperçu… Je trouve ça assez perturbant. Pas vous ? 

![Le coupable, c'est lui](/assets/img/2022-07-08/macos-scan.png)

L’application fermée, le problème disparait. Mais j’ai trouvé autre chose d’intéressant lors de la capture. C’est que l’imprimante répond sur le port 8080 à quelques requêtes http. C’est ainsi que j’ai découvert le protocole eSCL, un protocole de numérisation sans pilote basé sur http(s) et XML. Ça vient d’Apple, Airprint Scan, c’est pratique, c’est aussi supporté par SANE. Mais pas par Windows. Par contre, il semble que tout ceci soit en http par défaut, et je ne vois aucun réglage pour passer en HTTPS. En fait, l’imprimante semble papoter en clair quel que soit le protocole. Je trouve aussi des ports filtrés dont j’ignore l’usage pour le moment. Ce qui est aussi rigolo, c’est un scan de ports nmap lance une impression « garbage », il y a intérêt à être là pour l’annuler sur l’imprimante. 

```bash
80/tcp   open     http       nginx
|_http-title: Site doesn't have a title (text/html).
443/tcp  open     ssl/http   nginx
|_http-title: 400 The plain HTTP request was sent to HTTPS port
| ssl-cert: Subject: commonName=Imprimante-HP.lan/organizationName=HP/stateOrProvinceName=Washington/countryName=US
| Not valid before: 2020-03-02T03:21:16
|_Not valid after:  2030-02-28T03:21:16
|_ssl-date: TLS randomness does not represent time
| tls-alpn:
|_  http/1.1
| tls-nextprotoneg:
|_  http/1.1
631/tcp  open     http       nginx
| http-auth:
| HTTP/1.1 401 Unauthorized\x0D
|_  Digest qop=auth opaque=a4271cd240bc10759ec9cde12b92623d realm=IPP-Print nonce=00000000000000000000000000000000 algorithm=MD5 username=guest
3000/tcp filtered ppp
5003/tcp filtered filemaker
5862/tcp filtered unknown
5906/tcp filtered unknown
8080/tcp open     http       nginx
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Site doesn't have a title (text/html).
9100/tcp open     jetdirect?
```

Cette chasse au scanner fantôme m’aura appris plusieurs choses :

* Comment passer une interface Wifi en mode Monitor sous Linux, mais aussi sous Windows.
* Que toutes les cartes Wifi ne sont pas compatibles avec le mode Monitor.
* L’utilisation de Wireshark pour capturer et décoder le trafic wifi 802.11.
* Que laisser l’application de numérisation ouverte sous MacOS lance des prévisualisations aléatoires, et ce n’est pas bien, hein. C’est donc MacOS qui est hanté.
* Que mis à part la protection wifi, le trafic de numérisation lié au protocole eSCL passe en clair sur le réseau, et c’est pas bien.
* Que j’ai râlé sur l’imprimante HP, qui n’était pas en cause.
* Que mon modèle d’imprimante HP n’est pas très sécurisé, et que je n’ai pas de réglages pour ça.
* Que mon imprimante est boguée, entre les choix sur le panneau LCD, et son interface web qui ne sont pas synchronisés.
* Qu'il faut réfléchir un peu plus avant de se débarrasser de son vieux Nuc.
* Que je ne m’étais pas fait hacker, finalement, et c’est le principal.

Mais finalement, cette imprimante, comment va-t-elle ? Et bien, il semble qu'elle n'a pas apprécié d'avoir été autant manipulée. Là, elle est bloquée sur ce message. De nouvelles recherches en perspective !

![C'est en panne](/assets/img/2022-07-08/hp-error.png)

[Le modèle d'imprimante HP, j'ai payé bien moins cher](https://www.lesnumeriques.com/imprimante/hp-envy-photo-6232-p62025/test.html)
[Le protocole eSCL](https://gist.github.com/markosjal/79d03cc4f1fd287016906e7ff6f07136)
[Wireshark et décodage wifi](https://wiki.wireshark.org/HowToDecrypt802.11)
[Linux, wifi, monitor](https://blog.rottenwifi.com/wifi-monitoring-mode/)