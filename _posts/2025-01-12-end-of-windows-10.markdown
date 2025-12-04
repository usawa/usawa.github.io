---
layout: default
title:  "La fin de Windows 10 : une opportunité pour Linux ?"
date:   2025-01-12 17:30:00 +0100
description: Le 14 octobre 2025 est la fin du support officiel et gratuit de Windows 10 Home et Pro. L’utilisateur qu’on qualifiera de standard, sur un PC ou laptop classique, devra faire des choix.
categories: 
- linux
---
Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

Initialement publié sur [LinuxFR](https://linuxfr.org/users/usawa/journaux/la-fin-de-windows-10-une-opportunite-pour-linux)

- [Problématique](#problématique)
- [Quels choix ?](#quels-choix-)
- [Étendre le problème aux autres OS propriétaires](#étendre-le-problème-aux-autres-os-propriétaires)
- [Passer sous Linux ?](#passer-sous-linux-)
  - [Un sujet que je connais depuis longtemps](#un-sujet-que-je-connais-depuis-longtemps)
  - [L'odyssée](#lodyssée)
  - [Quelle distribution ? Quel bureau ?](#quelle-distribution--quel-bureau-)
  - [Allo, voisin ?](#allo-voisin-)
  - [Comment j'installe mes logiciels ?](#comment-jinstalle-mes-logiciels-)
  - [C'était mieux avant !](#cétait-mieux-avant-)
  - [Mais c'est quoi, un shell ?](#mais-cest-quoi-un-shell-)
  - [Ma webcam, elle marche pas](#ma-webcam-elle-marche-pas)
  - [Qui va m'aider ?](#qui-va-maider-)
- [Le jeu comme avenir](#le-jeu-comme-avenir)
- [Êtes-vous prêts pour les autres ?](#êtes-vous-prêts-pour-les-autres-)
- [Quelques liens sur ma réflexion](#quelques-liens-sur-ma-réflexion)

Attention journal fleuve.

# Problématique
Le 14 octobre 2025 est la fin du support officiel et gratuit de Windows 10 Home et Pro. L’utilisateur qu’on qualifiera de standard, sur un PC ou laptop classique, devra faire des choix.

Alors que faire ? Il y a plusieurs possibilités, selon que le matériel est supporté ou non pour un passage à Windows 11. Par matériel non supporté, on comprend les machines ne respectant pas les préconisations minimales, essentiellement en termes de processeurs et de module TPM. Par exemple, les processeurs Intel Core antérieurs à la 8ème génération ne sont pas supportés, laissant en plan des machines de plus de 6 ans. On parle de plus de 240 millions de PC à se retrouver sur le carreau. On fait quoi ? 

# Quels choix ?

J’en vois six :

1. Migrer sous Windows 11 avec un PC supporté : c’est la solution la plus logique pour l’utilisateur lambda de Windows 10 et 11 lui conviennent. Nous ne sommes pas ici pour juger de la pertinence d’utiliser Windows, les gens sont libres.
2. Migrer sous Windows 11 avec un PC non supporté : c’est techniquement possible via la modification de quelques clés de registre. Mais si la manipulation est possible, elle pose des soucis de mise à jour, et, bien entendu de support. Notamment, rien ne pourra empêcher Microsoft de stopper l’installation des mises à jour sur ces machines, et il faudra donc bidouiller.
3. Acheter une nouvelle machine, et je rajouterais, tenter de rentre l’actuelle compatible : ce n’est pas très écolo, en tout cas pour une nouvelle machine complète, surtout si l’ancienne fonctionnait parfaitement, et c’est aussi faire le jeu de Microsoft et de l’obsolescence programmée. Mettre à jour son matériel est aussi possible, souvent moins cher et même plus écologique en passant par le marché de l’occasion. Parfois par exemple l’ajout d’un module TPM à trente euros, ou via des kits carte-mère/CPU/RAM, qu’on peut aussi trouver d’occasion pour un prix intéressant, peuvent être la solution. 
4. Acheter une licence LTSC, pour Long Term Support Channel : pour continuer à obtenir un support officiel de Windows par Microsoft. Car la fin du support de Windows 10 Home et Pro ne signifie pas la fin du support de Windows. La version LTSC sera mise à jour jusqu’en janvier 2027, tandis que la version IoT, jusqu’en 2032. On peut alors acheter une licence, au coût officiel prohibitif, mais il existe de nombreux sites de revente de clés (issues de l’OEM, d’entreprises vendant leur parc, etc.) où on peut trouver une licence pour moins de 20 euros.  On peut aussi payer Microsoft via un contrat de maintenance. Cependant, la version officielle reste la 21H2.
5. Rien : Windows continuera de fonctionner, la plupart des applications aussi, jusqu’à une probable future annonce de la fin du support de la version obsolète de l’OS à un moment donné. Vous avez tout intérêt à installer un antimalware de qualité jusqu’à l’annonce inéluctable de l’arrêt définitif du support. Plus aucun patch de sécurité n’étant proposé, la solution n’est pas tenable à terme.
6. Passer sous Linux : c’est une solution qui nous semble évidente, en tant que Linuxiens, et proposée sur beaucoup de sites, avec un engouement intéressant, et des comparatifs du genre « quel est la meilleure distribution pour remplacer Windows 10 ». Évidente, vraiment ? c’est comme souvent « un eu plus compliqué que ça ». Nous en reparlerons.

# Étendre le problème aux autres OS propriétaires

On pourrait blâmer Microsoft sur son choix de fin de support de Windows 10, et de l’impossibilité d’installer Windows 11 sur des machines qui pourraient, techniquement parlant, le supporter. Je ne reviendrai pas sur ces raisons, qui sont parfois justifiables, mais j’insiste sur le problème posé : Windows est en position dominante. En décembre 2024, plus de 62% des installations de Windows sont sous Windows 10. C’est énorme. Contrairement aux machines sous Windows XP, 7 ou 8 qui pouvaient passer sous Windows 10 moyennant l’ajout de RAM ou d’un plus gros disque, là, ce n’est plus officiellement possible. Comment passer de 34% de Windows 11, à 100%, en 10 mois ? C’est impossible. Ceci signifie qu’en octobre, de nombreux utilisateurs vont se trouver exposés à des risques importants. 
Le souci n’est pas que sous Windows, il existe aussi sur les machines Apple de plus de 6 ans, où nombre d’utilisateurs se retrouvent avec un système d’exploitation MacOS obsolète depuis quelques mois. C’est même pire car Apple dispose à la fois de l’écosystème matériel et logiciel, piégeant l’utilisateur. On pourrait aussi parler de Android et les smartphones obsolètes au bout de trois ans, etc. Mais restons sous Windows. 

# Passer sous Linux ?

Passer sous Linux ! Voilà une belle idée, et peut-être une belle opportunité ! Matériel obsolète sous Windows ? Mais certainement pas pour Linux. La question de 01net sur la choix de la distribution Linux pour remplacer Windows est très intéressante sur le fond. Elle pose la question de l’adaptation de la distribution à la machine et aux usages. Elle met indirectement en lumière ce qui peut être de vraies gageures : la fragmentation, le changement des habitudes, et le support. La fin de Windows 10 remet en lumière le débat éternel de l’adaptation de Linux en tant qu’environnement de bureau (desktop) pour le commun des mortels. Linux est-il prêt pour le Desktop ?

## Un sujet que je connais depuis longtemps
C’était le sujet de mon mémoire de fin d’année en 1999, et pour moi la réponse était évidemment oui : je pouvais presque tout faire sous Linux, y compris taper mon mémoire (ah, Staroffice…), imprimer (ah, les imprimantes compatibles PostScript, gage de qualité), aller sur Internet (ah, kppp, les modules hackés pour les modems ADSL Sagem, …), etc. J’étais (je suis toujours) un passionné, et c’était là le biais cognitif : il était adapté pour moi, mais si je l’avais mis dans les mains d’un autre, l’expérience aurait probablement tourné en désastre. Mais du temps a passé, et j’ai moi-même lâché l’affaire Desktop Linux fin 2013, et Desktop Apple au début du Covid en 2020 (je voulais jouer, alors…).

## L'odyssée

Alors, imaginons l’incroyable utopie : 240 millions de PC passent sous Linux entre 2025 et 2026. Ces gens viennent de Windows (dans une moindre mesure de Apple), y ont toujours été habitués, et il va y avoir du travail, voire, des problèmes à la pelle ! Si l’utilisateur vient de Windows 10 sans souffrir de problèmes de performances, on peut déjà passer outre le problème des ressources nécessaires : si Windows 10 passe, Linux passera. Mais un commentaire d’un lecteur de l’article de 01net met le doigt là où ça peut faire mal : « passer à Linux peut s'apparenter à une odyssée pour quiconque n'ayant jamais connu que les OS Windows ». Il a bien raison.

## Quelle distribution ? Quel bureau ?

Déjà, l’article propose cinq distributions, dont une que je ne connaissais pas (Zorin OS), et ChromeOS flex, qu’on ne peut pas vraiment identifier comme une distribution Linux.  Le premier choix semble, historiquement, et pour beaucoup de linuxiens, évident : Ubuntu. Utilisateurs de Windows, si vous débarquez directement sous Ubuntu, vous risquez une surprise, l’interface graphique ne ressemble pas à ce que vous trouvez sous Windows, mais plutôt à celle d’un MacOS (leurs utilisateurs seront moins perdus), sauf à installer de nombreuses extensions absentes par défaut. Pas vraiment pertinent, car venant de Windows, un environnement de type KDE serait peut-être mieux adapté. Ensuite, s’ils avaient l’habitude de lancer Firefox en moins d’une seconde (sur un SSD), ils seront surpris de le voir démarrer en 10 secondes, à cause de Snap. Allez leur expliquer qu’il faut plutôt passer par un package natif plutôt qu’un snap : c’est parler chinois.

La seconde distribution est un peu plus pertinente, puisqu’il s’agit de Mint, dont le bureau, avec son pseudo menu Démarrer, devrait moins dépayser les gens venant de Windows. ZorinOS et Elementary OS pourraient sembler pertinents en ne tenant compte que de la première expérience utilisateur. L’emballage est joli. 

Pour ma part, j’aurais plutôt proposé Kubuntu, ou même, plutôt, Fedora KDE, si on vient de Windows (note: j'ai mis finalement Linux Mint). Un quelque chose à base de Gnome si on vient d'Apple. Problème ici : je ne suis déjà pas d’accord avec moi-même. Comment conseiller les autres si la fragmentation des distributions et interfaces me perd moi-même ? 

## Allo, voisin ?

Imaginons le dialogue entre deux voisins :

- Bonjour Père Lustucru, j’ai un souci sur mon ordinateur, je ne trouve pas comment faire pour xxxxxxx (remplacez par une action nécessitant quelques manipulations) 
-	J’arrive Mère Michel !
Quelques instants après :
-	Ah mais Mère Michel,  vous êtes sous Ubuntu et Gnome, et mois sous Fedora et KDE, je ne sais pas !
-	Comment ça Père Lustucru, je croyais que vous étiez sous Linux comme moi !
-	Bon, il va falloir demander à xxxxx (mettez ici un voisin, un linuxien, un site de support, …)

## Comment j'installe mes logiciels ?

Passé la découverte de l’interface et des menus. Voici le temps d’installer des logiciels. Coup de bol, Ubuntu et Fedora viennent avec un « Application Center », qui rappelle les « stores » des OS propriétaires. Jusque-là, ça va. Les soucis vont démarrer avec les logiciels absents, qu’il faudra installer manuellement. Or, et ça a bien été décrit par Linus Torvalds dans un de ses interviews, c’est un gros problème : il n’existe pas d’installation à la Windows, (setup.exe, msi) ou à la MacOS (dmg). Il existe des systèmes de packaging, ou d’images, et de gestion des dépendances, qui sont propres à chaque distribution. Et comme c’est plus rigolo que ça, des distributions utilisant les mêmes systèmes de packaging, n’utilisent pas les mêmes packages, et les versions et dépendances changent à chaque mise à jour de la distribution.  Un package pour Ubuntu 22.04 ne s’installera probablement pas sur une 24.04. Les snaps et flatpak sont là pour ça, en théorie. En pratique, leur utilisation s’avère lourde et lente, malheureusement. Et donc, un package par distribution, par version de distribution, en espérant qu’il soit disponible, avec une interface d’installation différente, des dépôts à activer. Aie. Avec le risque de devoir passer par le shell, finalement. 

Passer d’un simple « clic-clic, suivant, suivant, terminé » à ça : l’expérience utilisateur en prend un coup. Votre vieux logiciel sous Windows XP fonctionnera probablement parfaitement sous Windows 10 et 11, par contre, par rétrocompatibilité. Sous Linux, même en disposant des sources, il n’est même pas certain de pourvoir reconstruire une vieille application (librairies obsolètes, compatibilité glibc, différences de compilateurs, …). La seule garantie qu’on a sous Linux, c’est que le noyau lui-même ne doit pas casser quoi que ce soit. Mais le reste, par contre…

## C'était mieux avant !

On ne pourra pas passer par les comparaisons entre « ce que j’avais avant » et « ce que je peux avoir maintenant », notamment en termes de logiciels. Heureusement les plus courants sont disponibles sous Linux : Firefox, Chrome, VLC, LibreOffice, Thunderbird, etc. Donc, pour une utilisation classique, ça pourrait passer. Et puis pour jouer, Steam, via Proton ou nativement, propose un grand nombre de jeux compatibles. L’adaptation sera nécessaire. Mais il y a pléthore de cas où ce ne sera pas possible, soit que la solution de remplacement soit absente, soit qu’elle ne soit pas aussi adaptée, intuitive, ou quelle soit plus limitée. Un exemple est le support de solutions « cloud », comme le stockage et la synchronisation des fichiers (Google Drive, OneDrive, …) sans solution native, ou même le travail collaboratif via les outils installés : LibreOffice n’est pas MS Office, et la version Office 365 en ligne n’est pas aussi complète. Enfin, parlons compatibilité : Les utilisateurs s’envoient des fichiers, donc compatibilité et interopérabilité sont nécessaires, et ce sont généralement les leaders qui dictent leurs choix. Il faut donc que les outils gèrent les mêmes formats. 

## Mais c'est quoi, un shell ?

Peut-être suis-je un cas à part, mais il n’existe pas une seule fois où je n’ai pas dû aller plusieurs fois sur un shell (console) bash pour régler un problème sur une distribution Linux, y compris suite à des bugs post-installation de distributions pourtant réputées. Et ceci arrive souvent très vite après l’installation, notamment pour tenter de régler des soucis de compatibilité matérielle. Combien de fois un utilisateur ordinaire de Windows a dû ouvrir un cmd ou un Powershell pour régler un problème ? Sous Linux, ça devient souvent un réflexe pour les utilisateurs un peu plus avancés, mais si vous avez acheté votre PC au supermarché, non. Un truc en noir et blanc où taper des commandes sur un PC acheté au XXIème siècle ? "Mais on vient de Windows ou de MacOS, on a jamais vu ça !"

## Ma webcam, elle marche pas

Le support matériel est une vraie question aussi. On blâme beaucoup Windows pour son empreinte sur le disque, de plusieurs dizaines de Go. Le monde du PC est incroyablement hétérogène, matériellement parlant, et il faut supporter le maximum de matériel par défaut, il vient donc avec un maximum de pilotes et de couches d’interopérabilité et de rétrocompatibilité. Ce qui permet un grand support de matériel et de logiciels. Comme Windows domine, tous les constructeurs fournissent des pilotes pour Windows. Certains jouent le jeu sous Linux, la communauté fait des choses magnifiques, mais c’est ainsi : tout pourrait ne pas fonctionner. L’expérience de passage d’un Macbook « obsolète » sous Linux m’a assez marqué : webcam dont le package n’est disponible que sous Fedora, port SD jamais reconnu, gestion impossible de l’économie d’énergie, malgré des dizaines d’heures à tenter de régler les problèmes. Combien d’utilisateurs se retrouveront avec un matériel partiellement incompatible et sans solution ? Doit-on blâmer les fabricants pour ne pas supporter un système représentant moins de 1.5% du marché actuel des environnements de bureau ?

## Qui va m'aider ?

Tous ce gens auront besoin d’aide. Nombreux étaient les LUG (Linux User Groups) au début de Linux, mais aujourd’hui ? Qui se rappelle les Install Parties aux débuts des années 2000 ? Vont-ils aller sur les forums, quels réflexes vont-ils avoir ? Mais, aussi, comment vont-ils être accueillis ? Aurons-nous la patience de répondre à des questions dont les réponses nous semblent souvent évidentes ? Les « éditeurs » de Ubuntu, Fedora, Mint, ElementaryOS, etc., ont-ils les ressources nécessaires pour répondre aux questions de dizaines de milliers de nouveaux utilisateurs ? Comment ces nouveaux utilisateurs, perdus, vont-ils gérer leur frustration s’ils n’y arrivent pas ? Ce n’est pas leur métier ni leur passion, l’effort peut être important, et le risque est l’abandon, simplement.

# Le jeu comme avenir

Bizarrement, peut-être, comme je l’espère depuis 1999, nous aurons un jour une vraie solution. Dans un article en 2013 dans Planète Linux, le dernier article que j’y ai écrit, j’expliquais que le Linux n’était pas prêt pour le Desktop (et je m’étais fait clairement défoncer), ma position n’a pas encore changé, mais je concluais en disant que l’avenir du desktop Linux passerait peut-être par le jeu et les gamers. SteamOS fait tourner Steam Deck, c’est du Linux, Valve compte diffuser SteamOS comme solution pour tout PC, et Valve ne pourra pas accepter la fragmentation et un OS réservé aux utilisateurs avertis.

# Êtes-vous prêts pour les autres ?

Finalement, êtes-vous prêts à supporter les utilisateurs qui vont tenter la migration de leurs potentiels 240 millions de PC d
vers le desktop Linux ? Sans vous énerver ? A titre personnel, je vais migrer la machine de belle-maman sous Fedora et KDE : l’utilisation de la machine est limitée aux usages ordinaires : consulter des sites web, imprimer des documents, taper des courriers, et l’ensemble du matériel, imprimante/scanner compris, est supporté. Mais surtout : je suis là ! Et c’est moi qui vais maintenir maintenant machine et système d’exploitation, comme je le faisais pour Windows, parce que je que suis un Linuxien. Et je continuerai jusqu’à la panne de la machine, ou un accident de la vie. Si je n’avais pas été là, la solution aurait été soit de ne rien faire, soit d’aller acheter une nouvelle machine, pour repartir pour quelques années. Parce que ni belle maman, ni mon épouse, ni mes belles-sœurs et beaux-frères, ou toute autre personne autour, n’ont les compétences nécessaires. Et, si je devais conseiller un quidam au supermarché, qu’est-ce que je lui dirais ? De tenter Linux, ou je lâche l’affaire et je lui conseille une petite machine sous Windows 11 ?

Nous ne sommes par Légion, pensez-y.

# Quelques liens sur ma réflexion

* [Quelle distribution pour remplacer Windows 10](https://www.01net.com/astuces/fin-du-support-de-windows-10-quelle-distribution-linux-choisir-pour-votre-pc.html)
* [L’avis de Linus sur le Desktop](https://www.youtube.com/watch?v=Pzl1B7nB9Kc)
* [Vérifier la compatibilité de la machine avec Windows 11](https://support.microsoft.com/fr-fr/windows/comment-v%C3%A9rifier-si-votre-appareil-r%C3%A9pond-%C3%A0-la-configuration-syst%C3%A8me-requise-pour-windows-11-apr%C3%A8s-une-modification-du-mat%C3%A9riel-de-l-appareil-f3bc0aeb-6884-41a1-ab57-88258df6812b)
* [Processeurs supportés](https://learn.microsoft.com/fr-fr/windows-hardware/design/minimum/windows-processor-requirements)
