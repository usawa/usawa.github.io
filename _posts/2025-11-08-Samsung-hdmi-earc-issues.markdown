---
layout: default
title:  "La gestion désastreuse du HDMI ARC/eARC par Samsung"
date:   2025-11-08 16:30:00 +0100
description: Quand on voit ça, on se demande comment Samsung peut être le leader des ventes de téléviseurs.
categories: divers
---
Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

- [Les télés et moi](#les-télés-et-moi)
- [Les barres de son et moi](#les-barres-de-son-et-moi)
- [Quelques définitions pour comprendre](#quelques-définitions-pour-comprendre)
  - [HDMI](#hdmi)
  - [CEC](#cec)
  - [ARC et eARC](#arc-et-earc)
  - [Transfert, Passthrough](#transfert-passthrough)
- [Le début des problèmes](#le-début-des-problèmes)
  - [Panasonic SC-HTB15, pourquoi changer ?](#panasonic-sc-htb15-pourquoi-changer-)
  - [Et puis y'a la Samsung...](#et-puis-ya-la-samsung)
  - [Tizen, les applications internes](#tizen-les-applications-internes)
  - [La barre de son Sony HT-A3000](#la-barre-de-son-sony-ht-a3000)
- [Les emmerdes, ça vole toujours en escadrille](#les-emmerdes-ça-vole-toujours-en-escadrille)
  - [eARC, pas le bon son en TNT, perte de son aléatoire](#earc-pas-le-bon-son-en-tnt-perte-de-son-aléatoire)
  - [ARC, eARC, Passthough, oui mais des fois seulement](#arc-earc-passthough-oui-mais-des-fois-seulement)
- [Entre deux maux, il faut choisir le moindre](#entre-deux-maux-il-faut-choisir-le-moindre)
- [Est-ce seulement Samsung ?](#est-ce-seulement-samsung-)
- [Que faire ?](#que-faire-)
- [Quelques sources](#quelques-sources)

Les télés et moi
================

J'ai une sorte de malédiction avec les téléviseurs. En 20 ans, j'ai dû en changer plusieurs fois, parce qu'à chaque fois, des soucis apparaissent. 2005, une des premières Philips LCD 32 pouces ? L'image bave, souci avec le port DVI (image verte), conflit avec Philips qui finit par me la remplacer par un autre modèle après appel à mon assistance juridique (la nouvelle, une Ambilight, a fonctionné plus de 13 ans, mais la moitié de l’Ambilight est tombée en panne … 3 jours après la fin de la garantie). Achat en 2009  d’une Panasonic 37 pouces Plasma ? L'image s'assombrit mois après mois, des brûlures apparaissent. Achat en 2016 d’une Sony Led 40 pouces ? Quelques mois après la fin de la garantie, le côté gauche de la dalle se met à "fuir" laissant apparaître des halos de lumière, on ne voit que ça, zéro support de Sony la garantie étant dépassée (beau papa a la même depuis 2016 aussi, aucun souci). Et on arrive à décembre 2022 : l'achat d'une Samsung 43 pouces. Avec son lot de problèmes.


Les barres de son et moi
========================

Un contexte additionnel: À l'achat du Plasma Panasonic en 2011, je lui ai adjoint une barre de son Panasonic SC-HTB15, compatible ARC. Je l'ai utilisée pendant 14 ans, une belle machine compatible Dolby Digital et DTS (formats de base). Alors qu'elle fonctionnait parfaitement (mais j’avais mes raisons, on en reparlera plus loin), petit caprice de ma part, je décide de prendre une barre Sony HT-A3000, à un tarif plus qu'intéressant, compatible avec à peu près tout, eARC, HDMI 2.1. La barre Panasonic sera donnée à une recyclerie qui l'a revendue depuis. Les problèmes que je vais présenter ont en fait démarré avant le passage à la barre de son Sony, mais à l'achat du téléviseur Samsung. Mais il m'a fallu ce changement pour en comprendre les origines. 

Quelques définitions pour comprendre
====================================

Avant tout, il faut aborder quelques notions simples, ou les simplifier. Les puristes devront m'excuser, je ne rentrerai que peu ou pas dans les détails. Quatre définitions doivent être abordées.

HDMI
----

**HDMI**, *High Definition Multimedia Interface*, est une norme d'interface audio/vidéo numérique. Elle définit à la fois les formats des connecteurs, mais aussi des flux audio et vidéo qui y transitent. La vidéo n'est pas compressée (si elle l’est elle est décompressée avant l’envoi en signaux HDMI), le son peut l'être, ou pas, avec perte de qualité, ou pas (lossless). Les flux peuvent être chiffrés, ou non. Grosso-modo, c'est ce qui a remplacé, depuis plus de 20 ans, la prise Péritel, ou les connecteurs divers, qui étaient analogiques et non adaptés à la haute définition et les nouveaux formats audio. Comme toute norme, elle évolue dans le temps et plusieurs versions sont apparues, apportant chacune le support de nouvelles fonctionnalités, résolutions vidéo, comme l'UHD (Ultra HD), le Dolby Vision, la gestion des couleurs HDR (High Definition Range), ou de format sonores, comme les formats multicanaux DTS-HD/MA, Dolby Atmos, et ainsi de suite. C'est aussi l'apport de nouvelles fonctions, comme l'Ethernet (eh oui) 100 mbits, le CEC (Consumer Electronic Control), et l'(e)ARC (Audio Return Channel). Plus on monte dans les versions de normes, plus on a de nouvelles possibilités, et plus le débit de données nécessaire est important. La norme 2.1 datant de 2017 offre un débit total de 48 Gbit/s. C'est fou. Il faut utiliser un câble compatible appelé High Speed ou Ultra High Speed. La dernière version HDMI en 2025 est la 2.2. Les équipements le supportant ne sont pas , en cette fin de 2025, courants. Chaque version décrit ce qui est supporté ou pas, et la rétrocompatibilité dépend de vos équipements: si vous connectez un équipement en version 2.1 sur un vieux téléviseur en HDMI 1.x, ça pourrait marcher, ou pas, à vous de vous renseigner. L'inverse, par contre, devrait toujours fonctionner. En pratique, c'est le bordel.

Comme HDMI est une norme, si un fabricant vend de l'équipement avec le support d'une version d'une norme donnée, il doit bien évidemment être compatible avec cette norme. N'est-ce pas ? N'EST-CE-PAS (j'insiste) ? Et bien, on découvre très souvent de petits arrangements. Et on les découvre souvent trop tard. N’est-ce pas Samsung ?

CEC
---

**CEC**, *Consumer Electronic Control* permet le pilotage combiné des appareils HDMI. C'est en gros ce qui va permettre ce petit monde de dialoguer, et dans la pratique, c'est ce qui permet d'utiliser, par exemple, la même télécommande pour changer le volume de la télé et qui va en fait le modifier sur la barre de son, ou de naviguer dans les menus de votre box Android avec la télécommande de votre TV, ou d'allumer et d'éteindre tous les équipements en appuyant sur le bouton off de cette même télécommande. CEC étant peut-être trop compliqué pour l'utilisateur, pour le prendre encore un peu plus pour un idiot chaque fabricant lui donne un autre petit nom à lui:
* **Anynet+** chez Samsung
* **Bravia Sync/Link** chez Sony
* **Viera link** chez Panasonic
* **Easylink** chez Philips
* etc. Sauf UN: Hitachi, qui utilise **HDMI-CEC** (merci à eux)

Pour utiliser ARC et eARC, CEC doit être activé.

ARC et eARC
-----------

**ARC**, *Audio Return Channel*, qui va grandement nous intéresser, je reprends la définition de Wikipedia: cette fonctionnalité dite de « canal de retour audio » permet un échange de signaux (son) numériques entre tous les appareils interconnectés. Ainsi, avec des appareils compatibles HDMI ARC, un seul câble HDMI compatible suffit pour retransmettre le signal audio en complément de la vidéo, même à travers plusieurs appareils interconnectés, de bout-en-bout. En pratique, via un port HDMI dédié, vous raccordez votre téléviseur à une barre de son compatible, et le son de tous les équipements sera acheminé sur cette barre de son. C'est une chouette fonctionnalité : plus besoin de câble optique, plus besoin de connecter les équipements sur la barre de son (switch/hub HDMI), mais uniquement sur le téléviseur, par exemple. Une évolution, appelée **eARC**, *extended ARC*, accepte une vitesse plus grande, et donc plus de formats audio. Pour profiter du eARC, il faut entrer en HDMI 2.0b ou plus (donc, dire qu'il faut du 2.1 est faux). Pour profiter du Dolby TrueHD, qui est du Dolby Atmos sans perte, il faut être compatible eARC (ARC est suffisant pour du Dolby Atmos avec perte, encore un mythe qui s'écroule).

Transfert, Passthrough
----------------------

**Passthrough**, ou **transfert**, est l’action de renvoyer le signal tel quel à l’équipement sans lui faire subir aucune modification. Il arrive souvent qu’un téléviseur, un lecteur DVD/BR ou un boîtier de streaming, puissent décoder et convertir le format en quelque chose de plus simple (par exemple du DTS vers du Dolby, ou du PCM, qui est généralement le format de base, sur deux canaux - stéréo par exemple - ou plus) que la télé ou la barre de son puissent comprendre et donc jouer. En activant le transfert, on indique juste “ne fais rien, renvoie le signal tel quel”. Ça se règle sur le téléviseur, ou sur le périphérique connecté, par exemple le boîtier de streaming. Si le support audio/vidéo dispose par exemple d’une piste Dolby TrueHD, alors le flux est envoyé tel quel à la barre de son. Si elle décode, tant mieux (et souvent elle le dit), sinon au pire on a pas de son du tout.

Le début des problèmes
======================

Panasonic SC-HTB15, pourquoi changer ?
--------------------------------------

Jusqu'à il y a un mois, j'utilisais une barre de son Panasonic compatible ARC, HDMI 1.4. Le fonctionnement de cette barre a toujours été admirable. Du Dolby ? Pas de souci ! Du DTS ? Pas de souci. Pourquoi changer ? Le caisson de basse prend de la place, les deux entrées HDMI intégrées au caisson ne supportent pas le 4K et sont donc inutiles, et les codecs (formats audio) supportés accusent leur âge, ne permettant pas de disposer du son tel que fourni soit par la télé, soit par les fournisseurs de contenus. Après tout, si un film, comme le Hobbit par exemple, est supposé être écouté avec les variations sonores proposées par un format (codec) donné, pourquoi ne pas en profiter ? Donc… Cette barre, sur les trois télés, est reconnue comme ARC et comme supportant, par exemple, le format Dolby. 

Ça tombe bien, la plupart des chaînes TNT diffusent en Dolby Digital. Dans ce cas, le voyant de la barre de son est sur Dolby, c’est bon. Si je veux jouer un film en Bluray ou via un extract (mkv, mp4) depuis un boîtier de streaming comme Android TV (ou Apple TV, mais je n’ai pas), j’active le HDMI Passthrough - moyennant quelques réglages adéquats sur le boîtier ou dans l’application (comme Kodi). En mode ARC, jamais aucun souci avec Panasonic, ni avec Sony, ni avec la petite Philips de la chambre qui a remplacé le modèle de 13 ans qui a fini par lâcher. Puis, la télé Samsung est arrivée, et pendant un petit moment, tout semblait bien aller.

![En TNT, on doit avoir du Dolby](/assets/img/2025-11-08/01-tnt-dolby.jpg)

*Une chaine TNT diffusant en Dolby Digital Plus*

Et puis y'a la Samsung...
-------------------------

Le modèle **Samsung UE43AU7025** est un téléviseur UHD d’entrée de gamme de 43 pouces, acheté fin 2022 à 390€ chez BUT, que vous retrouverez [Samsung UE43AU7025](https://www.samsung.com/fr/tvs/uhd-4k-tv/au7025-43-inch-uhd-4k-smart-tv-ue43au7025kxxc/ "Samsung UE43AU7025")
Sur le descriptif elle est chouette: jolies couleurs avec le HDR, applis intégrées (on y reviendra), Dolby Digital Plus, 3 ports HDMI 2.0 dont un eARC. C’est d’ailleurs là que j’ai appris que bien que eARC soit normalement prévu par la norme 2.1, les constructeurs ont mis à jour leurs modèles pour que leurs ports 2.0 le supportent. Au quotidien, l’image est chouette, surtout en 4K. La sortie son native est … déplorable. Les téléviseurs sont devenus tellement fins qu’on ne peut plus espérer grand chose. Le son natif des haut-parleurs des anciennes TV LCD épaisses ou même cathodiques était bien meilleur. D’où l’utilisation (poussée, forcée par les constructeurs ?) d’une barre de son additionnelle. J’y connecte ma barre de son. En ARC, ma petite barre Panasonic m’affiche bien Dolby. Jusque là tout va bien. Sauf que… en branchant mon boîtier de streaming (à l’époque une BBOX 4K avec Kodi bien réglé), je n’arrive pas à passer en DTS: pas de son si je sélectionne la pise adéquate. En le branchant en direct sur la barre de son, le DTS s’active. Dans les réglages du téléviseur, le format de sortie audio numérique est réglé sur Auto, et l’entrée “Transfert” (Passthrough) est grisée. Ah, mais pourquoi ? Bon, fin 2022/début 2023, je me suis dit que je chercherai plus tard, d’autant plus que les plateformes de Streaming comme Netflix ou autres diffusent en Dolby, que tout fonctionne en Dolby. et que mes fichiers rippés, je les regarde essentiellement sur mon PC, ma tablette et mon casque. Merci Plex, d’ailleurs.

Tizen, les applications internes
--------------------------------

Un aparté sur les applications internes: elles tournent sous **Tizen**, un système d’exploitation basé sur un noyau Linux (et ses pilotes) et proposant des applications soit natives Tizen, soit HTML5, soit hybrides, soit Android (si publiées) via une VM Dalvik. Tizen est une initiative de Samsung, puis d’Intel, et supportée par la Linux Foundation. A la base, c’est chouette. Dans les faits, sur mon téléviseur je suis rapidement tombé sur une limite. La télé supporte uPNP/DNLA. Là on se dit que c’est super, qu’on a plus besoin de boitier de streaming. Quelle naïveté. Je tente de lire du contenu avec du Dolby, c’est bon. Je tente du contenu avec un piste DTS, un message d’erreur me dit que ce n’est pas supporté. Ah. Cherchons donc le mode Transfert alors ! Surprise, confirmée par une discussion sur le forum de support de Samsung: le mode Transfert n’est supporté que via le HDMI, pas pour les applications internes. C’est mort. Sauf Plex (oui, il y a une appli Tizen Plex, d’une grande lenteur d’interface), qui va encoder le flux audio en autre chose (par le serveur). Seconde surprise qui aurait pu s’avérer agréable, Tizen propose les applications Orange TV, OQEE TV (Free) et B.TV (Bouygues). Étant chez Bouygues, et tant Bouygues que Samsung vantant leur partenariat, c’était l’une des raisons de mon choix de ce téléviseur (quand l’antenne est cassée suite à une tempête, quand on a la fibre, on est content de pouvoir regarder la télé par là). Nouveau drame avec Bouygues: l’appli ne peut fonctionner que si on a l’abonnement adéquat, qui implique de renvoyer tous ses autres boitiers et donc de n’avoir que des télés Samsung (Bouygues n’a pas d’application TV officielle sur Android TV) ! Et pour plus cher. Mais que c’est débile, mais débile ! Ce fut l’occasion de joutes intéressantes sur leur forum et au téléphone. Je suis resté chez Bouygues, mais je me suis longtemps demandé si je ne devais pas aller chez Orange…

La barre de son Sony HT-A3000
-----------------------------

On saute en octobre 2025. Pour des raisons personnelles, j’ai eu beaucoup de temps libre à la maison (c'est fini). Je regardais de temps en temps les barres de son, car la petite Panasonic ne supportait pas les formats récents, et je constatais la présence de beaucoup de choses stupides, notamment les barres de moins de 300€ ne supportant que le PCM ou le fameux Dolby Atmos. Pour des formats plus évolués, il fallait mettre bien plus cher. Et puis trouver une barre de son correcte sans caisson de basse (qui je le rappelle, prend de la place en plus de risquer de faire sauter son voisin quand c’est fort), c’est compliqué. Je tombe par hasard sur une description du modèle **Sony HT-A3000** (et ses sœurs A5000 et A7000). Descriptif [Sony HT-A3000](https://www.sony.fr/electronics/barres-de-son/ht-a3000 "Sony HT-A3000") Et là je me dis qu’il y a un truc. Notamment, des basses suffisantes pour mon usage dans un petit format, ce que je confirme par ailleurs, le support de tous les formats Dolby et DTS les plus courants, et la compatibilité eARC pour les exploiter ! 700 euros à sa sortie, ouille… Mais je finis par la négocier à 170€ à un particulier. Aussitôt reçue, aussitôt branchée. Et très vite, il y a un truc qui cloche. Et ce n’est pas la barre de son.

Les emmerdes, ça vole toujours en escadrille
============================================

eARC, pas le bon son en TNT, perte de son aléatoire
---------------------------------------------------

![Samsung en eARC](/assets/img/2025-11-08/02-samsung-earc-auto.jpg)

*eARC est activé, le format de sortie sur Auto*

Déjà, puisque la barre de son et la télé sont compatibles eARC, activons tout ça. Ça tombe bien, tout est en Auto. La télé me confirme que tout va bien via un “HDMI eARC” dans la sélection de la sortie son, et lors du réglage du volume. Mais le son n’est pas comme avant avec les chaînes de la TNT. Faut-il s’habituer ou régler la barre de son ? Oui, bien sûr, mais le problème n’est pas là. L’affichage de la barre m’indique LPCM. Ce n’est pas normal. Ça devrait être Dolby. Je passe sur le canal 52, qui sort France2 TV en UHD et DAC4 ou Dolby Atmos: pareil. Le son est plat. Disclaimer: quand le eARC est activé, je n’ai JAMAIS pu avoir un son correct avec les chaînes de la TNT. Même aujourd’hui. J’ai tout essayé. J’ai écumé les forums Samsung officiels, supports, etc. Rien.

![Sony en PCM](/assets/img/2025-11-08/03-sony-pcm.jpg)

*Mais le son sort en PCM...*

Puis, aléatoirement, en allumant la télé, il n’y a pas de son du tout. Je comprends rapidement que lorsque j’allume la télé, ça allume la barre de son, mais le temps que ce petit monde synchronise, la télé lâche l’affaire. Eteindre et allumer la télé ou la barre de son suffit, mais ce n’est pas normal. De même, je perds parfois le son en changeant de chaine. Retour sur les forums avec deux conseils dont un qui est clairement un constat d’échec de Samsung: s’ils conseillent de laisser la barre allumée en permanence (merci l’écologie, et en plus ça n’a pas réglé le problème), leur seul autre conseil est de basculer la sortie audio numérique de Auto à PCM. Alors, effectivement, ça fonctionne. Sauf qu’il y a un effet de bord. Si en TNT de toute façon je n’avais pas autre chose, via les applications Tizen (Netflix par exemple), je perds le Dolby. Et oui, on a forcé le transcodage en PCM ! A moi le son tout plat ! 

ARC, eARC, Passthough, oui mais des fois seulement
--------------------------------------------------

Allons maintenant voir ce qui se passe avec un boîtier de streaming, à savoir pour moi une clé Fire TV Stick 4K Max puis une Fire TV Cube. Magie : l’option Transfert (passthrough) apparaît dans le choix de sortie audio numérique du téléviseur ! Oui mais comme on est en PCM, et pas en auto, il faut y retourner à chaque fois pour changer le paramètre, puis penser à remettre PCM quand on revient sur la TNT. Quelle sorcellerie ! Je continue, je lance Kodi, vérifie les paramètres Passthrough (j'écrirai un article sur la configuration de Kodi), et magie ! DTS s’affiche sur la barre, voire même DTS:X avec le contenu de test (on trouve des Blurays de test DTS et Dolby un peu partout0, qu'on peut extraire et mettre sur un partage ou un support USB). Pour le Dolby Atmos, il faut une piste UltraHD et activer le surround sur la barre de son, et hop, elle affiche Dolby Atmos ! Mais alors, tout fonctionne ? La barre de son, oui, est impeccable ! La télé Samsung non. Outre le bug du silence (y compris par les haut-parleurs) en sortie de veille, pourquoi le son n’est-il pas correct en TNT ?

![Samsung en ARC](/assets/img/2025-11-08/04-samsung-arc-auto.jpg)

*eARC est désactivé, on passe en ARC, format de sortie sur Auto*

Je suis borné, alors je continue. Mon postulat est simple: si ça marche en ARC, ça devrait marcher en eARC non ? Mais si ça ne fonctionne pas en eARC, est-ce que ça fonctionne en ARC ? Je vérifie. Je désactive l’eARC sur le téléviseur, ce qui commute en ARC. Je repasse le format de sortie audio numérique en Auto. Et… Miracle ! La TNT sort en Dolby ! Victoire !!! (non). 

![Sony en Dolby](/assets/img/2025-11-08/05-sony-dolby.jpg)

*Le son passe en Dolby !*

Je retourne sur ma Fire TV, je lance les pistes Dolby, c’est bon (y compris Atmos). Je lance les pistes DTS: rien, pas de son. Mais quelle est donc cette sorcellerie ? Deux choses. Dans Kodi le dispositif Audio a été modifé (RAW à la place de IEC), et dans les réglages de la TV, le mode Transfert est grisé ! Mais c’est une blague ! Et là je lis, sur le côté, que le mode transfert ne fonctionne qu’en eARC, pas en ARC… Mais, Samsung, mais vous êtes fous ? Bon, si je remets IEC dans Kodi, et que je retourne jouer ma piste DTS, ça fonctionne. MAIS, le DTS:X (HD/MA), je n'ai pas de son: il faut être en eARC... On ne peut pas avoir le beurre, et l'argent du beurre. Je remarque que selon le type de flux audio, sur la télé Transfert est sélectionné ou non, mais ne peut pas être forcé.

![Samsung pas de passthrough](/assets/img/2025-11-08/06-arc-transfert.jpg)

*Le Transfert HDMI ne fonctionne pas en ARC*

Je viens donc de comprendre à ce moment, l’origine de mon problème de 2022. je continue, et dans les réglages de la FireTV, il y a un réglage du mode Surround: meilleure correspondance, Dolby Digital, Dolby Digital Plus. Si je passe sur Dolby Digital Plus, le téléviseur dégrise le mode Transfert, mais si le boîtier envoie autre chose que du Dolby, il le grise à nouveau ! Autrement dit, Samsung empêche sciemment de choisir le mode Transfert sur des critères aberrants. 

J’en suis là. Je ne vois plus trop quoi faire côté télé ou barre de son.

Entre deux maux, il faut choisir le moindre
===========================================

**En général**:
* Le mode Transfert (passthrough) ne fonctionne pas ni en TNT, ni avec les applications internes Tizen. Uniquement (quand c’est possible) en HDMI.

**En mode eARC**:
* Pas de son Dolby en TNT
* Coupures de son aléatoires en sortie de veille de la télé si sortie numérique en Auto
* Le passage en sortie PCM règle le problème.
* Le mode Transfert (passthrough) est disponible et fonctionne ()
* Il faut sans arrêt aller dans les menus pour passer de PCM à Auto ou Transfert selon qu’on utilise la TNT, les applications internes ou un boîtier de streaming HDMI.

**En mode ARC**:
* Son Dolby en TNT
* Pas de coupures son en sortie de veille même en Auto
* Le mode Transfert (passthrough) ne peut être forcé.
* Le mode transfert n’est activable uniquement si le boîtier de streaming sort du Dolby Digital Plus (!)
* Dans ce cas, Dolby fonctionne, DTS aussi, mais pas le DTS-HD (DTS:X)

**Ce qu’il faudrait, et que Samsung ne permet pas**:
* Son Dolby en TNT en eARC
* Pas de coupures son en eARC si la sortie numérique est Auto.
* Autoriser le mode Transfert (passthrough) en ARC **TOUT LE TEMPS**.
* Autoriser une gestion personnalisée des réglages internes et de chaque port HDMI (tel réglage en TNT, tel réglage sur HDMI1, …)

La conclusion est qu’il est impossible d’avoir quelque chose de cohérent avec ce téléviseur Samsung. Pour regarder la TNT, je dois passer en ARC. Pour passer sur mon boitier de streaming (Fire TV), si je veux du DTS:X ou Atmos lossless, je dois passer en eARC.

Est-ce seulement Samsung ?
==========================

Encore une fois, je suis borné. Est-ce lié à ce modèle, à la marque ou est-ce systémique ? Je suis tombé sur des centaines de sujets et de messages de plaintes concernant les téléviseurs Samsung, de tous modèles, y compris de très haut de gamme, des gens essayant désespérément de faire fonctionner leurs barres de son en eARC. Y compris les barres de son Samsung (les Q-Symphony par exemple). La gestion du son de Samsung semble très problématique. Les réponses de Samsung, comme celles de forcer le PCM, ne sont pas à la hauteur: forcer le PCM, débrancher ceci, rebrancher celà, réinitialiser, changer de câble HDMI (j’en ai essayé trois, au cas où), etc. On trouve toujours, à force, un moyen de tout faire fonctionner. Mais avec tant d’efforts ! Et pas tout en même temps.
Est-ce pareil avec les autres marques de téléviseurs ? Sur certains forums, on insiste sur le fait que les marques sont parfois incompatibles avec elles-mêmes (comme par exemple, Samsung avec ses propres barres de son). Mais, dans mes recherches, Samsung semble sortir clairement du lot. J’ai aussi trouvé plusieurs griefs concernant LG, mais je n’ai pas creusé. Quant à Sony, j’ai trouvé des gens qui râlent, mais souvent la réponse est donnée plus loin, et finalement ça fonctionne après quelques réglages. Ce fut mon cas lorsque je comprenais pas pourquoi je n’obtenais pas de Dolby Atmos: on m’a donné une réponse simple et ça a fonctionné.

Que faire ?
===========

Alors, que faire ? Il faudrait que j’arrive à convaincre Madame de changer de TV ? Mais dans le fond Madame a un peu raison en disant qu’elle fonctionnait très bien avant de changer de barre de son ! Eh oui, ma petite barre Panasonic ARC, recevait bien du Dolby en TNT avec la télé. Et pourtant, le dysfonctionnement était déjà là, caché dans les réglages de Transfert (throughput) impossibles avec le boîtier de streaming. Changer de téléviseur, et de marque, c’est prendre le risque d’avoir le même problème, ou d’autres. Pas sûr qu’un vendeur accepte de me voir débarquer avec ma barre de son et ma clé Fire TV chez Darty ou Boulanger :) Et donc, acheter à distance et renvoyer à chaque fois une télé ? 

Autre possibilité, utiliser un boitier extracteur audio pour barre de son, par exemple le [FeinTech AX340](https://www.amazon.fr/FeinTech-AX340-Switch-Extracteur-audio/dp/B0FBRZ29DF), qui va intercepter le signal de sortie de ce qui est connecté dessus pour le renvoyer directement vers la barre de son: on évite le téléviseur. Le coût est élevé, et ça rajoute un équipement.

En attendant, je prends l'habitude de rester en ARC en regardant la télé, mais de passer en eARC pour le boitier de streaming. Et surtout, il ne faut pas oublier de revenir en ARC, sinon, pas de TNT Dolby, perte de son irrégulière en changeant de chaine, etc. !

Dans un prochain article, j'expliquerai comment, avec une Fire TV Stick ou un Fire TV Cube, jouer correctement tous les formats audio en mode transfert (passthrough).

# Quelques sources

* [Wikipedia HDMI](https://fr.wikipedia.org/wiki/High-Definition_Multimedia_Interface)
* [La communauté râle aussi](https://en.community.sonos.com/home-theater-229129/no-sound-from-arc-when-using-youtube-on-samsung-tv-6885315)
* [Configuration eARC par Samsung](https://www.samsung.com/fr/support/tv-audio-video/what-is-earc-and-how-to-set-on-samsung-smart-tv/)
* [Que faire si pas de son selon Samsung](https://www.samsung.com/latin_en/support/tv-audio-video/no-sound-from-your-samsung-soundbar/)
* [Ca râle sur Reddit aussi](https://www.reddit.com/r/samsung/comments/166nbp0/samsung_smarttv_suddenly_no_sound_via_arc/)

Et tellement d’autres…

