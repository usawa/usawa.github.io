---
layout: default
title:  "Port Knocking : les toqueurs ont la tactique"
date:   2024-06-29 00:00 +0100
description: "Présentation d'une méthode de sécurité par l'obscurité : le port knocking. C'est pas bien, mais que'est-ce que c'est chouette !"
categories: 
- securité
---
Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

Publié aussi sur LinuxFR.

- [Que l'obscurantisme retourne à l'obscurité](#que-lobscurantisme-retourne-à-lobscurité)
- [L'obscurité c'est pas bien: démonstration](#lobscurité-cest-pas-bien-démonstration)
- [Complètement toqué, ce mec-là !](#complètement-toqué-ce-mec-là-)
- [Les trois coups](#les-trois-coups)
- [Cerbère de la porte, ou maitre des clés ?](#cerbère-de-la-porte-ou-maitre-des-clés-)
- [De l'art de frapper à la porte](#de-lart-de-frapper-à-la-porte)
- [knockin on heaven's door](#knockin-on-heavens-door)
- [Obscurité, pas obscurantisme](#obscurité-pas-obscurantisme)
- [Et sinon, futés ou pas ?](#et-sinon-futés-ou-pas-)

# Que l'obscurantisme retourne à l'obscurité

La sécurité par l’obscurité, vous connaissez ? Ça se résume simplement : il faut savoir tenir sa langue. Chut, c’est un secret ! Pour s’assurer qu’un système, logiciel ou protocole de sécurité est sûr, il suffit de ne pas divulguer son fonctionnement. Pratique !  Et si on veut faire de la rétro-ingénierie ? Pareil, on chiffre le code, on fait de l’obfuscation, on rajoute volontairement des bêtises pour tromper l’ennemi, etc.

Est-ce que ça fonctionne ? Parfois oui, parfois non. Ça fonctionne jusque’à ce que quelqu’un trouve le secret ! Auquel cas la sécurité disparait totalement s’il est rendu public.

Est-ce une bonne idée ? Non, bien évidemment. C’est d’ailleurs un excellent argument pour les logiciels libres et Open Source, la disponibilité du code permettant de l’auditer. Les failles de certains protocoles réseaux, certains logiciels et certains systèmes d’exploitation, notamment Windows dans ses grandes heures, sont un exemple concret des dégâts qui peuvent être causés quand l’éditeur ou le concepteurs mentent sur le niveau de sécurité de leur produit ou pire, cachent volontairement les failles. C’est vraiment se moquer des hackers et autres experts en sécurité (coucou les redteamers !). 

# L'obscurité c'est pas bien: démonstration

Pour bien comprendre le principe général, imaginez que vous, Charlie, vouliez échanger des messages secrets avec votre ami Lulu (toute ressemblance avec des personnages réels ne serait que fortuite), mais via un forum ou un chat publics. Vous définissez un code secret : le A devient N, le B devient 0, et ainsi de suite. Et vous postez donc :

`Yn gevohar rfg ha ercnver qr qnatrerhk rkgerzvfgrf`

Votre ami connait le code, et peut le décoder. Mais les autres eux, n’y voient de du charabia. Pendant un certain temps, ça fonctionne. Un inconnu, frustré, décide de s’y attaquer. Il remarque des groupes de mots (les espaces, hein), et devine qu’un code de César (une substitution) est en place. Il essaie les combinaisons, et finit par trouver qu’il y a un décalage de 13 caractères par rapport à la position de chaque lettre dans l’alphabet. Il décode le message :

`La tribune est un repaire de dangereux extremistes`

Puis il évente le secret dans un message public : Hey, voici comment décoder les messages de Charlie et Lulu ! Et voila, tout le monde le sait, les messages passés et présents sont maintenant totalement lisibles. Fini la sécurité !

# Complètement toqué, ce mec-là !

Et pourtant, il existe quelques méthodes sympathiques ou l’obscurité peut rendre quelques services. Vous avez tous vu, dans des films, séries, dessins animés, ou lu dans un livre ou une BD, l’utilisation de codes secrets quand on frappe à la port : 

**Toc toc toctoctoc toc toc !**

Et on vous ouvre la porte ! Vous avez tapé une séquence, connue de vous un le gardien, et il vous ouvre la porte. Pratique ! C’est ce qu’on appelle le tocage de porte, ou **port knocking**.

Peut-être exposez-vous un port d’administration permettant l’accès à une machine ou à votre réseau, sur Internet. Prenons SSH. Si vous faites l’erreur d’utiliser des ports prédictives, comme tous ceux qui contiennent des séquences de 2 (22, 2222, 22022, mais un scanner de ports finira peut-être par trouver votre port) il est fort probable que vous voyez des tonnes de traces de tentatives de connexion. Bien sur, vous êtes plus malins que ça, vous avez du fail2ban, vous utilisez des clés de bonne taille, des passphrases, avec un SSH bien configuré (pas de root). N’est-ce pas ? 

# Les trois coups

Maintenant, imaginez que nous mettions en place un bocage de port (et non pas de porte) : on définit une séquence de ports prédéfinis , par exemple 1111, 2222 et 3333. SSH écoute comme d’habitude sur le port 22. Nous voulons que le port 22 soit accessible uniquement si on toque d’abord sur le port 1111, puis le port 2222, puis le port 3333, dans cet ordre uniquement, et dans un laps de temps donné. Alors, seulement, le port 22 sera ouvert et vous pourrez vous connecter.

Simple sur le papier, mais comment faire ? Eh bien, figurez-vous que le firewall intégré à Linux, Netfilter, le permet, sans besoin de service supplémentaire. Et voici comment faire.

On vérifiera évidemment que les ports sélectionnés ne sont ni déjà utilisés (quoi que…) ni dans dans la plage dynamique (`sysctl net.ipv4.ip_local_port_range`).

Je n’ai pas appris, ni compris tout seul, pour ça j’ai cherché des sources, comme tout le monde, et la plus sympa est celle de la documentation de Arch Linux, ici : https://wiki.archlinux.org/title/Port_knocking . Mais ce n’est pas simple, alors je vais essayer de vous expliquer. 

# Cerbère de la porte, ou maitre des clés ?

Si vous n'avez pas la référence [c'est ici](https://www.youtube.com/watch?v=O0MFVwALWsg)

Nous allons mettre nos règles dans un fichier de type **iptables.rules**. En premier lieu, assurons-nous d’avoir une règle qui autorise en entrée les connexions de type RELATED et ESTABLISHED. Il serait bête de se voir couper l’accès une fois le port fermé alors que la connexion est déjà établie. C’est généralement la première, ou l’une d’elles sur les tables INPUT et FORWARD. Ici c’est INPUT. On va créer une table TRAFFIC qui servira de source à INPUT et on va commencer à la remplir :

```
-A INPUT -j TRAFFIC
-A TRAFFIC —m state --state ESTABLISHED,RELATED -j ACCEPT
```

Attention ça devient plus compliqué : on autorise l’accès sur le port 22 uniquement si l’adresse IP est contenue dans une liste appelée SSH2. Notez la présence du module « recent », 

```
-A TRAFFIC -m state --state NEW -m tcp -p tcp --dport 22 -m recent --rcheck --seconds 30 --name SSH2 -j ACCEPT
```

Si la même IP a été refusée (la règle précédente n’a pas matché, par exemple plus de 30 secondes), alors on dégage tout, il faut recommencer la séquence.

```
-A TRAFFIC -m state --state NEW -m tcp -p tcp -m recent --name SSH2 --remove -j DROP
```

Bon, maintenant comment on met l’IP en question dans SSH2 ? Et bien, c’est l’IP qui aura effectué la séquence complète 1111, 2222, 3333, donc l’IP qui sera arrivée jusqu’à 3333. On rajoute une règle pour ce dernier port. Si ça passe on saute aux actions de la table SSH-INPUTTWO, sinon, bah comme précédemment, erreur dans la séquence, on vire l’IP, et il faudra recommencer :

```
-A TRAFFIC -m state --state NEW -m tcp -p tcp --dport 3333 -m recent --rcheck --name SSH1 -j SSH-INPUTTWO
-A TRAFFIC -m state --state NEW -m tcp -p tcp -m recent --name SSH1 --remove -j DROP
```

Il y a quoi dans SSH-INPUTTWO ? Tout simplement, on enregistre l’IP dans la liste SSH2 ! On mettra ça à la toute fin, il faudra y penser :

```
-A SSH-INPUTTWO -m recent --name SSH2 --set -j DROP
```

Comme c’est la dernière c’est qu’on est arrivé jusque là, et si on est dans les temps alors on peut ouvrir le port 22. Mais pour arriver là, il faut avoir toqué au port 2222, alors on fait pareil, en vérifiant que l’IP est dans SSH0, donc qu’on a toqué dans le port 1111:

```
-A TRAFFIC -m state --state NEW -m tcp -p tcp --dport 7777 -m recent --rcheck --name SSH0 -j SSH-INPUT
-A TRAFFIC -m state --state NEW -m tcp -p tcp -m recent --name SSH0 --remove -j DROP
```

Vous devinez bien ce qu’il y a dans SSH-INPUT : on enregistre l’IP dans SSH1, comme ça elle pourra aller toquer sur le port 3333 :

```
-A SSH-INPUT -m recent --name SSH1 --set -j DROP
```

Il reste à toquer sur le port 1111, pour remplir SSH0, comme c’est le premier, on a pas a effacer quoi que ce soit, de toute façon qu’on fasse 1111-2222-3333 ou 1111-1111-2222-3333, la seconde contient la première séquence. Si on toque sur 1111, on enregistre l’IP dans SSH0, et donc on sera autorisé à toquer sur 2222.

```
-A TRAFFIC -m state --state NEW -m tcp -p tcp --dport 1111 -m recent --name SSH0 --set -j DROP
```

Et c’est ainsi que si on toque 1111, on à notre IP dans SSH0, donc on peut toquer sur 2222 qui mettra l’IP dans SSH1, donc on peut toquer sur 3333, qui mettra l’IP dans SSH2, et si on a pas dépassé les trente secondes, on peut alors toquer le port 22, qui s’ouvrira. C’est joli ! 

Accessoirement, pourquoi a-t-on l’impression que les règles sont à l’envers ? Tout simplement parce que Netfilter fonctionne avec une règle de première correspondance : dès qu’il trouve une règle qui correspond (qui « matche »), il s’arrête là. Et donc, il commence par le port 22. Ça passe (IP dans SSH2) ? Non. Suivante, Port 3333, ça passe (IP dans SSH1) ? Non, Suivante, et ainsi de suite. Et donc, ça force par commencer avec 1111 (il s’arrête), puis 2222 (il s’arrête), puis 3333 (il s’arrête), puis le port 22 (il ouvre et s’arrête). Classe non ?

A final, on obtient un fichier potables comme ceci, honteusement pompé de la doc Arch Linux :

```
*filter
:INPUT DROP [0:0]
:FORWARD DROP [0:0]
:OUTPUT ACCEPT [0:0]
:TRAFFIC - [0:0]
:SSH-INPUT - [0:0]
:SSH-INPUTTWO - [0:0]
-A INPUT -j TRAFFIC
-A TRAFFIC -p icmp --icmp-type any -j ACCEPT
-A TRAFFIC -m state --state ESTABLISHED,RELATED -j ACCEPT
-A TRAFFIC -m state --state NEW -m tcp -p tcp --dport 22 -m recent --rcheck --seconds 30 --name SSH2 -j ACCEPT
-A TRAFFIC -m state --state NEW -m tcp -p tcp -m recent --name SSH2 --remove -j DROP
-A TRAFFIC -m state --state NEW -m tcp -p tcp --dport 3333 -m recent --rcheck --name SSH1 -j SSH-INPUTTWO
-A TRAFFIC -m state --state NEW -m tcp -p tcp -m recent --name SSH1 --remove -j DROP
-A TRAFFIC -m state --state NEW -m tcp -p tcp --dport 2222 -m recent --rcheck --name SSH0 -j SSH-INPUT
-A TRAFFIC -m state --state NEW -m tcp -p tcp -m recent --name SSH0 --remove -j DROP
-A TRAFFIC -m state --state NEW -m tcp -p tcp --dport 1111 -m recent --name SSH0 --set -j DROP
-A SSH-INPUT -m recent --name SSH1 --set -j DROP
-A SSH-INPUTTWO -m recent --name SSH2 --set -j DROP
-A TRAFFIC -j DROP
COMMIT
```

# De l'art de frapper à la porte

Pour toquer, rien de plus simple, il suffit juste de tester le port, ou d’y envoyer ce qu’on veut: **nmap**, **nc**, ce que vous voulez. De toute façon les DROP vont tout dégager, et il n’y a aucun service derrière. Donc :

```bash
nc -z 192.168.56.2 1111
nc -z 192.168.56.2 2222
nc -z 192.168.56.2 3333 
```

Et hop dans la foulée :

```bash
ssh seb@192.168.56.2
```

On peut aussi utiliser nmap :

```bash
for PORT in 1111 2222 3333 ; do nmap -Pn --host-timeout 201 --max-retries 0 -p $PORT 192.168.56.2; done
```

Ou même le faire en shell pur :

```bash
for PORT in 1111 2222 3333 ; do export PORT=$PORT ; timeout 1 bash -c 'cat </dev/null >/dev/tcp/192.168.56.2/$PORT'; done
```

On peut bien entendu mettre tout ça dans un script, ou créer des alias.

# knockin on heaven's door

Travailler avec règles Netfilter directement directement, ça peut faire peur, et surtout, ce n’est pas pratique ni forcément bien adapté à certaines distributions. On peut certes coder un générateur, en quelques lignes de code, ce n’est pas très compliqué. Pourquoi ne pas utiliser un service qui gère ces règles à notre place ? On va utiliser **knockd**, disponible dans toutes les bonnes distributions, au hasard, Ubuntu 24.04.

Une fois installé, nous modifions le fichier **/etc/knockd.conf** :

```
[openSSH]
	sequence    = 1111,2222,3333
	seq_timeout = 5
	command     = /sbin/iptables -I INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
	cmd_timeout = 10
	stop_command = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
	tcpflags    = syn
```

Cinq secondes pour la séquence, puis 10 pour accéder au port 22. On modifie aussi **/etc/default/knockd** pour mettre la bonne interface :

```
KNOCKD_OPTS="-i enp0s3"
```

Et on démarre :

```bash
sudo systemctl restart knockd
```

Knockd vient avec la commande **knock**, pour jouer la séquence. Normalement, on peut aussi utiliser nc, map, ou même bash pour toquer sur les ports, mais (j’ai eu la flemme de chercher pourquoi, en tout cas depuis mon Mac), ces andouilles envoient plus d’un SYN malgré les flags qui vont bien. Par contre, aucun souci avec knock : on teste la séquence :

```bash
knock 192.168.56.2 1111 2222 3333
```

Et on obtient :

```
juin 28 20:18:46 ubuntu knockd[2144]: 192.168.56.1: openSSH: Stage 1
juin 28 20:18:46 ubuntu knockd[2144]: 192.168.56.1: openSSH: Stage 2
juin 28 20:18:46 ubuntu knockd[2144]: 192.168.56.1: openSSH: Stage 3
juin 28 20:18:46 ubuntu knockd[2144]: 192.168.56.1: openSSH: OPEN SESAME
juin 28 20:18:46 ubuntu knockd[2368]: openSSH: running command: /sbin/iptables -I INPUT -s 192.168.56.1 -p tcp --dport 22 -j ACCEPT
```

Et voila ! Le port est ouvert ! Mais dépêchez-vous, sinon :

```
juin 28 20:18:56 ubuntu knockd[2368]: 192.168.56.1: openSSH: command timeout
juin 28 20:18:56 ubuntu knockd[2368]: openSSH: running command: /sbin/iptables -D INPUT -s 192.168.56.1 -p tcp --dport 22 -j ACCEPT
```

Alors on peut faire ça :

```bash
$ knock 192.168.56.2 1111 2222 3333 && ssh seb@192.168.56.2
Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.8.0-36-generic x86_64)
```

La commande knock est disponible sous Linux et MacOS et on trouve des outils adéquats pour Windows aussi. 

# Obscurité, pas obscurantisme

Et voila. Vous avez implémenté votre méthode de sécurité par l’obscurité. 

Un intrus peut-il déduire la séquence utilisée pour ouvrir le port ? S’il est malin, oui. Un sniffer permet d’intercepter le trafic réseau. Si le hacker est bien positionné, on peut imaginer que l’analyse des trames lui donne la réponse, notamment s’il remarque qu’elle aboutit systématiquement sur du trafic plus important sur le port SSH que vous aurez choisi. Cependant la plupart des bots sont heureusement stupides, ils tenteront juste de tester plusieurs ports et des paires d’identifiants et de mots de passe une fois trouvés.

Le port knocking n’est donc pas une solution de sécurité à utiliser seule. Dans l’exemple du code de la porte, le gardien aurait pu demander un mot de passe. Elle vient en complément d’autres méthodes de sécurité. On appliquera notamment le principe de Kerckhoffs : la sécurité repose sur le secret de la clé. Oui, la clé est un secret, mais ce n’est pas de la sécurité par l’obscurité : même si on dispose du code source, de l’algorithme de chiffrement, ou d’une capture des trames réseau, il est impossible, par rétro-ingénierie, de retrouver la clé, rendant impossible le décodage. Pour SSH, vous utiliserez donc le chiffrement asymétrique, avec une paire de clés publiques et privées. Ceci garantira la confidentialité des échanges et l’authenticité du correspondant. Clients et serveurs SSH négocieront ensuite l’utilisation d’un cipher, algorithme de chiffrement, cette fois symétrique, via la génération de plusieurs clés, mais c’est une autre histoire.

# Et sinon, futés ou pas ?

Et en pratique, quel a été l’effet du port knocker ? Et bien comme prévu les bots ne sont pas futés. J’ai routé les ports depuis ma box vers une machine virtuelle (pontée sur l’interface de l’hyperviseur). Et j’ai attendu. Lors de mes essais, sur d’autres ports et sur plusieurs jours, il n’y a eu plus aucune tentative de connexion à part les miennes. Plus aucune trace du service ssh, sauf mes connexions, plus rien via journalctl. Evidemment, ssh est lui-même sécurisé, réduisant le risque en cas de tentative d’intrusion : port peu commun, clés, passphrases, interdiction de root, etc. Mais, à terme, j’aimerais trouver quelque chose de mieux, peut-être faudrait-il pousser le concept plus loin. On pourrait imaginer des variations dans la temporisation, ou que l’appel au dernier port fournisse une valeur qui une fois décodée représenterait le port, variable, du service à ouvrir.

Si vous voulez aller plus loin, notamment si vous exposez vraiment un service SSH sur Internet, pensez à un **honeypot** : exposez fièrement un serveur SSH bidon sur les ports habituels, serveur totalement déconnecté du reste, rempli de trucs qui semblent appétissants. Ca attirera les méchants comme des mouches, vous laissant le temps de les détecter et d’agir. Il existe des kits complets pour ça (faites vos recherches, je ne contracte pas). Il y a aussi ce petit outil sympa qui va volontairement casser les pieds , https://github.com/skeeto/endlessh. 