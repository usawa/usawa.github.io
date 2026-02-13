---
layout: default
title:  "Plomberie : un bon coup de pression pour une bien mauvaise vanne"
date:   2026-01-22 18:00 +0100
description: "Décidemment, mes articles peuvent être très éclectiques. Aujourd'hui je vous conte une sale histoire qui m'ont mis, ma plomberie et moi, sous pression."
categories: 
- plomberie

---

Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

- [Ne faites pas l'erreur de faire confiance](#ne-faites-pas-lerreur-de-faire-confiance)
- [Premier réducteur, premiers problèmes de pression](#premier-réducteur-premiers-problèmes-de-pression)
- [Second réducteur, même problème](#second-réducteur-même-problème)
- [Dilatation, expansion, tâtonnements](#dilatation-expansion-tâtonnements)
- [Incompétents ou malhonnêtes ?](#incompétents-ou-malhonnêtes-)
- [Le génie des linuxiens, et l'IA](#le-génie-des-linuxiens-et-lia)
- [Et la pression redescend](#et-la-pression-redescend)
- [Bilan](#bilan)

> **NOTE**
> Pour beaucoup, les raisons, et la solution à mon problème vont sembler évidentes. Mais je ne suis pas plombier, et j'ai perdu du temps avant de comprendre pourquoi ça ne marchait plus, j'ai été conseillé par des pros incompétents, avant de comprendre et réparer par moi-même.

# Ne faites pas l'erreur de faire confiance

Le 4 décembre 2025, un employé de l'entreprise **Godin** missionné par **Franciliane** est venu changer mon compteur. C'est la troisième fois en 20 ans (donc 4ème compteur), entre les changements de fournisseur et les télé-relevés... Gros gâchis, ils remplissaient tous correctement leur rôle. Mais ici, j'ai fait une erreur : le compteur est dans la cave, et je n'ai pas surveillé. Après quelques minutes, l'employé est parti, merci et au revoir. Je ne suis même pas allé voir pendant trois semaines.

Le 30 décembre, un quelconque abruti énervé ayant cassé l'arrêt (loquet) du volet donnant sur la rue, après avoir acheté le nécessaire, je descends à la cave pour préparer la réparation. Surprise ! Je vois le nouveau compteur qui pend littéralement, suspendu et non soutenu sur la conduite d'arrivée d'eau ! Le compteur n'est maintenu que par mon installation cuivre, en tension. Pire ! La bassine, toujours en place sous la vanne de purge de mon installation, est remplie d'eau ! Ca fuit ! L'ensemble a forcé sur le réducteur de pression (vieux de plus de 25 ans), et après avoir pensé que la fuite était sur les raccords 15/21, force est de constater que c'est le réducteur qui est cassé. Pensez bien que j'ai envoyé une gueulante à la compagnie ayant changé le compteur. J'ai réussi à le repositionner sur le précédent support.

# Premier réducteur, premiers problèmes de pression

Je suis quelqu'un qui stresse beaucoup, surtout lorsqu'il s'agit de travailler sur des choses que je maitrise mal, comme couper le tuyau d'arrivée d'eau de toute ma maison (si je coupe trop, je fais comment ?). Et impossible de trouver un plombier rapidement (nouvel an, congés, etc.). Donc, je vais chez **Cedeo** qui me vend un réducteur de pression, **Altech 3696046**, et quelques conseils d'un plombier présent. Coupe cuivre, raccords clipsables (très pratique), stress car il faut des adaptateurs 1/2 vers 3/4, de la filasse, etc. Mais j'y arrive.

Tout semble bien, mais la pression est super basse. Alors je règle pour en mettre plus, mais ce n'est pas parfait et ça fluctue. Surtout, on se rend compte après deux jours qu'on a d'énormes coups de bélier quand on tire la chasse des WC. Ou qu'on a une forte pression qui descend très vite quand on ouvre un robinet. Notamment après avoir tiré de l'eau chaude : ma chaudière produit de l'eau chaude en instantané, je n'ai pas de ballon. Et jusque là, en 20 ans, dont un changement de chaudière fin 2013, on a jamais rien constaté (pas de surpression, pas de coup de bélier). Retour à la cave : le nouveau réducteur fuit ! Purée... Après recherches, le réducteur est sous-dimensionné pour l'installation (c'est plutôt en amont d'un chauffe-eau), et la marque n'est pas très réputée. J'en achète un nouveau, de bonne facture : **Itron Isobar+**. 

Le temps qu'il arrive, je donne deux consignes : 

* Tirer de l'eau froide 2 secondes avant de tirer la chasse
* Laisser couler quelques secondes l'eau froide après avoir tiré de l'eau chaude.

# Second réducteur, même problème

Le nouveau réducteur arrive, il est bien plus gros, et il faut de nouveau couper. Pas le droit de se rater, vous connaissez la musique, le stress, etc. Mon fils m'assiste: je  purge l'installation, je coupe, raccord, filasse, etc. J'y adjoins un manomètre, puis je règle à 3 bars. 

![Le bon réducteur](/assets/img/2026-01-22/2026-01-22-reducteur.jpg)

*Le réducteur Isobar+. On voit déjà un souci sur le manomètre* 

On teste. Ca semble ok au début mais je vois la pression monter en tirant de l'eau chaude. Pression qui descend vite quand on tire de l'eau froide... Mais qui monte à plus de 8 bars ! Aiguille du manomètre bloquée en butée ! Puis, je remarque que si j'ouvre le robinet de purge, il n'y a rien qui sort. Alors je comprends que les réducteurs font aussi office de clapet anti-retour :

* Si la pression de l'installation est supérieure à celle réglée sur le réducteur, l'eau reste bloquée.
* Si la pression est inférieure ou égale à celle réglée : l'eau peut passer dans les deux sens, on peut purger en ouvrant un mitigeur, par exemple.

![8 bars !](/assets/img/2026-01-22/2026-01-22-8bars.jpg)

*8 bars ! Il y a comme un problème.* 

# Dilatation, expansion, tâtonnements 

Ainsi, je comprends que c'est la dilatation de l'eau chaude qui fait monter la pression dans toute mon installation sanitaire. Ce que je n'avais pas avant. Alors que faire ? Je vais de forums en forums, j'ai des réponses contradictoires : installer un groupe de sécurité, mettre un clapet anti-retour sur l'arrivée d'eau froide de la chaudière, oui mais non, inutile sur une chaudière sans ballon, etc.  Ca peut aussi venir des mitigeurs qui occasionnent un retour eau chaude vers froide. C'est possible ! Alors je change les mitigeurs : pareil ! Je suis perdu, alors comme j'ai peur de faire une bêtise, et que je n'ai plus beaucoup de marge pour couper mon tuyau, j'appelle des plombiers de ma ville, après avoir vérifié leur réputation. Un seul me répond, il passe deux jours plus tard.

> **Note** : vends trois mitigeurs Grohe encore sous garantie 3 ans, parfait état.

# Incompétents ou malhonnêtes ?

Ce fut ubuesque. Malgré une démonstration du problème, il ne sait pas, ne comprend pas, me propose de remplacer le réducteur par un autre de marque Watts, pour ... **690 euros** ! Et oui, on peut mettre aussi un clapet anti-retour, etc. Devant mon air dubitatif, notamment sur le tarif (après vérification, le modèle coûte moins de 100 euros, et on parle de 20 minutes de pause), je refuse. Je lui règle son déplacement, mais je ne suis pas plus avancé : un plombier qui ne sait pas... Pour sa réputation on repassera.

Je contacte alors la société **IZI Confort** effectuant l'entretien de ma chaudière. Là, c'est mieux, car il m'explique que les normes ont changé depuis 30 ans, que les réducteurs agissent comme des clapets anti-retour en sortie de compteur, que oui l'expansion de l'eau chaude provoque une montée de la pression, oui, un clapet juste avant la chaudière va empêcher mon installation de monter en pression, mais que normalement ça et les groupes de sécurité sont uniquement pour les chaudières avec ballon. En gros, ce qui se passait avant, c'est que l'ancien réducteur laissait refluer la pression vers le compteur, qui, d'une façon ou d'une autre, empêchait la montée excessive en pression.

Je commande un clapet anti-retour. Mais j'ai des doutes.

# Le génie des linuxiens, et l'IA

Je suis Linuxien, et je fréquente la **tribune de LinuxFR**. Dans ce microcosme d'une vingtaine de personnes, on trouve des gens comme tout le monde. A tout hasard j'expose mon souci. **Zephred** me répond (je ne connais pas Zephred dans la vie, mais je lui dois une bière). Et Zephred me dit qu'il a eu quelque chose d'équivalent, que je ne dois surtout pas mettre ce clapet car je vais déplacer le souci vers la chaudière et l'installation d'eau chaude, et que ça va faire des dégâts, que je devrais mettre un vase d'expansion et un groupe de sécurité. 

Je vais alors interroger une IA (ChatGPT) en lui exposant le souci et ce que je pensais faire (le clapet). Réponse immédiate et violente : 

« **Surtout pas !** »

Et l'IA de m'expliquer tout, exactement ce que Zephred m'a dit. Après un dialogue où elle me demande quelle est la taille de mon installation, la puissance de la chaudière, le diamètre de mes tuyaux, etc., la réponse tombe, calculs à l'appui :

* Il faut installer un vase d'expansion de 2 litres sur l'arrivée d'eau froide au plus près de la chaudière
* Il faut gonfler le vase entre 2.8 et 3 bars
* la pression devrait alors se maintenir entre 3 et 3.6 bars même en tirant de l'eau chaude, l'expansion se faisant dans le vase.
* dans ce cas le groupe de sécurité (7 bars) n'est pas nécessaire.

# Et la pression redescend

J'achète un Vase d'expansion **Elbi Sany-2** et tout ce qui va bien pour faire ça moi-même (et je prévois en plus si je rate : raccords, flexibles, té, tuyau cuivre, ...). Dimanche 18 janvier, encore stressé, je stoppe la chaudière, je purge l'installation, je coupe (je découvre les écrous à collet battu avec joints intégrés- *Gripp S2* : pas mal mais chiant à serrer),  je pose le tout, et je teste. Je passe sur les resserrages nécessaires, et à la fin... Après presque trois semaines de galère : 

**ça marche !**

![Le vase d'expansion](/assets/img/2026-01-22/2026-01-22-vase.jpg)

*Le vase d'expansion installé  (moche, pas fini, mais on s'en fout c'est invisible)* 

Eau chaude à fond pendant plusieurs minutes, je stoppe, je fonce au manomètre : 3.6 bars ! Je relance l'eau chaude, ça ne dépasse pas 3.6 bars ! Et en refroidissant, ça se stabilise à 3 bars. Le vase se remplit d'eau et joue son rôle. Le seul truc qui pourrait encore me faire tiquer est que depuis plus de 20 ans, l'expansion de l'eau chaude sanitaire se faisait en retour dans l'installation d'eau froide, et normalement, on évite de boire de l'eau qui a été chauffée. Sauf que le sens de circulation de l'eau et la vase rendent ce risque plus que faible.

![3 bars](/assets/img/2026-01-22/2026-01-22-3bars.jpg)

*Enfin une pression normale* 

# Bilan

Que penser de tout ça ?

* Le plombier qui est venu ne connait pas son métier, et c'est grave. Etait-il incompétent, malhonnête, ou juste que cette intervention ne l'intéressait pas ? Parce 690 euros pour un truc qui n'aurait rien réglé, voire pire, c'est un coup à arnaquer les petites vieilles et les gens crédules.
* Le chauffagiste un peu plus, mais n'avait pas la bonne solution
* Si j'avais suivi les premiers conseils, j'aurais aggravé la situation, et ça aurait pu être dramatique.
* Zephred est formidable.
* L'IA a été performante, et on devrait y penser dès le début.
* J'ai acquis de nouvelles compétences en plomberie, production d'eau chaude et en compréhension d'une installation.

J'ai de nouveaux réflexes à acquérir hors de mon métier (notamment l'IA), et surtout, il faut que j'apprenne à arrêter de stresser : ça m'a pourri pendant trois semaines, cette histoire ! Et puis j'ai découvert une micro-fuite sur le raccord inférieur du robinet d'arrêt, un truc qui n'avait pas bougé depuis 2013, il faut que je redémonte (c'est un écrou Gripp S2, donc le tuyau rentre dans le robinet).

Quant au clapet anti-retour, je ne l'ai jamais monté.