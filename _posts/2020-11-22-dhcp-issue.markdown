---
layout: default
title:  "DHCP et heure système"
date:   2020-11-22 12:48 +0100
description: "Je viens de passer plus d'une heure à résoudre un souci avec mon client DHCP, je pense que ça pourra aider certains."
categories: 
- dhcp

---

Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

Publié aussi sur LinuxFR.

Cher journal,

Je viens de passer plus d'une heure à résoudre un souci avec mon client DHCP, je pense que ça pourra aider certains. Je n'avais pas rallumé mon Chromebook, un Asus C300s, depuis des semaines. J'ai viré ChromeOS dessus, j'ai installé GalliumOS. Ancien, mais fonctionnel. Bien entendu, la batterie s'est entièrement vidée.

Une fois branché, impossible d'obtenir une connexion wifi. J'ai cru à un souci matériel, Vu que le ChromeBook s'était éteint sur batterie vide, j'ai pensé à une corruption de système de fichiers (quelques messages fsck au boot), mais non... 

En poussant un peu plus, j'ai remarqué que, pour le wifi, l'association avec le point d'accès s'effectuait bien, mais que le problème était à l'acquisition de l'adresse IP. j'ai branché un adaptateur USB/Ethernet avec le même résultat. Une fouille des logs indique :

```bash
DHCPDISCOVER on wlp2s0 to 255.255.255.255 port 67 interval 3 (xid=0x995990e)
Unable to set up timer: out of range
```

Même message avec un dhclient manuel pendant l'association avec le point d'accès.

```
Version du client: 4.3.5
Package: isc-dhcp-client 4.3.5-3ubuntu7.1
```

Malgré des modifications du fichier dhclient.conf, rien à faire. Puis mon oeil a été attiré par l'heure système: 05h07, alors qu'il était 11h07. Rien de grave en théorie. Mais... En voulant modifier l'heure, je remarque que je suis le 13 novembre (nous sommes le 20) non pas 2020, mais **2120** !!!  En remettant manuellement l'heure, DHCP refonctionne. Je suis tombé sur ce rapport de bug:

https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=801805

Donc, sachez-le : pour pouvoir utiliser un client DHCP, l'heure courante de votre système ne doit pas avoir de décalage important dans le futur par rapport à l'heure du serveur DHCP.