---
layout: default
title:  "Écrire un livre à deux : Haute Disponibilité sous Linux, des prémices à la sortie"
date:   2021-09-06 01:00 +0100
description: "En juillet 2020, LinuxFR m’avait fait un grand honneur en m’[interviewant](https://linuxfr.org/redirect/109088) dans le contexte de la sortie de la sixième édition de mon livre sur l’administration Linux. Une question concernait la coécriture, (l’écriture à plusieurs auteurs) et j’avais indiqué que j’avais proposé ce projet à un ami, sur un sujet devenu compliqué. Un an après, le résultat de cette collaboration a été publié !"
categories: 
- livre

---

Publié le {{ page.date | date: "%d/%m/%Y à %H:%M" }}

Publié aussi sur LinuxFR.

- [Contexte](#contexte)
- [Environnement de travail](#environnement-de-travail)
- [Le fil rouge](#le-fil-rouge)
- [Les technologies](#les-technologies)
- [La répartition du travail](#la-répartition-du-travail)
- [L’écriture et l’accident OVH](#lécriture-et-laccident-ovh)
- [La relecture et la sortie](#la-relecture-et-la-sortie)
- [Et après ?](#et-après-)


En juillet 2020, LinuxFR m’avait fait un grand honneur en m’[interviewant](https://linuxfr.org/redirect/109088) dans le contexte de la sortie de la sixième édition de mon livre sur l’administration Linux. Une question concernait la coécriture, (l’écriture à plusieurs auteurs) et j’avais indiqué que j’avais proposé ce projet à un ami, sur un sujet devenu compliqué. Un an après, le résultat de cette collaboration a été publié !



Charles Sabourdin et moi avons donc la joie de vous annoncer le résultat de plus de six mois de travail : 
[Haute disponibilité sous Linux : De l’infrastructure à l’orchestration de services](https://www.editions-eni.fr/livre/haute-disponibilite-sous-linux-de-l-infrastructure-a-l-orchestration-de-services-heartbeat-docker-ansible-kubernetes-9782409030796). Il est disponible via les réseaux de distribution ordinaires. Il nous a semblé pertinent de vous montrer tout le cheminement nous ayant mené jusqu’à cette sortie.

----

- [Journal à l’origine de la dépêche](https://linuxfr.org/users/usawa/journaux/ecrire-un-livre-a-deux-haute-disponibilite-sous-linux-des-premices-a-la-sortie)
- [Livre sur le site des éditions ENI](https://www.editions-eni.fr/livre/haute-disponibilite-sous-linux-de-l-infrastructure-a-l-orchestration-de-services-heartbeat-docker-ansible-kubernetes-9782409030796)
- [L'interview de Sébastien Rohaut](https://linuxfr.org/news/interview-de-sebastien-rohaut-auteur-de-livres-notamment-sur-linux)
- [Le dépôt git des exemples](https://github.com/halnx-todo)

----

Contexte 
========
Si je suis un Linuxien passionné depuis le milieu des années 1990, l’ingénierie système Linux n’est devenue mon cœur de métier qu’en 2006. Je me suis très vite intéressé à la Haute Disponibilité : répartition de charge, clusters, stockage, réseau. Entre 2010 et 2012, j’ai sorti deux éditions d’un livre dédié à ce sujet, qui dérivait la mise en place et l’utilisation de la haute disponibilité sous Linux. Très spécifique, il a eu un succès que je qualifierais d’estime : peu vendu, mais apprécié des spécialistes. Très axé technique, il ne présentait pas de fil rouge ou de cas concrets, juste les grands principes et leur implémentation.



En 2013, j’ai changé d’employeur pour travailler sur un projet de grand ampleur, la construction d’une plateforme directement exposée sur Internet, délivrant de nombreux sites web et backends d’applications mobiles pour une quarantaine d’entités internationales du groupe. Les six années que j’ai passées sur cette plateforme, de la définition de son architecture jusqu’à sa fermeture en passant par son exploitation, ont été les plus denses que j’ai connues, jusqu’à ce jour. Elles m’ont permis d’aborder des notions supplémentaires liées à l’exposition sur Internet, la répartition sur plusieurs datacentres, la mutualisation des équipements, le cloud, la sécurité, mais aussi une nouvelle forme de travail associée à de nouveaux outils via la culture DevOps, notamment via une collaboration importante avec les équipes projets, et l’utilisation d’outils d’automation.



En 2016, Charles rejoint mon équipe au moment où nous effectuons notre passage vers les orchestrateurs, notamment avec Kubernetes mais surtout OpenShift. Pour être parfaitement honnête, un troisième larron nous a rejoint (Salut Pascal ! Le prochain, on s’y colle ensemble ?) De 2016 à 2019, l’intégralité des projets ont été portés sur une plateforme OpenShift. 



Si je suis Ops, Charles est à l’origine plutôt orienté projet, urbanisme et développement. Charles a très vite appréhendé la logique de Kubernetes, dont il est devenu un virtuose, jusqu’à se faire certifier et évangéliser les foules. Quant à moi, j’avais posé toutes les fondations de la plateforme et une grosse partie de l’automation, basée sur Ansible et docker. L’équipe était plus nombreuse, mais nous en étions le cœur battant. À la fin du projet, outre un sentiment de vide à combler, nous avons tous voulu capitaliser notre expérience. D’où cette idée d’écriture commune d’un livre.



Environnement de travail
========================
Désolé pour les puristes, le livre a été écrit sous Office 365, qui s’est très bien prêté à l’exercice, malgré parfois quelques soucis liés à la feuille de style imposée par l’éditeur. Pour l’environnement de test, nous avons souhaité qu’il soit accessible au plus grand nombre, et nous avons utilisé des machines virtuelles sous Virtualbox. 13, au total. Et nous avons poussé notre matériel personnel dans ses derniers retranchements. Ma machine de plus de 7 ans a souffert, avec son vieux Intel Core i7 et ses 32 Go de RAM. De même, le porte-monnaie a souffert lors des tests d’OpenShift sous AWS, surtout quand on oublie de libérer le stockage à la fin… Quant à l’OS, c’est Linux, plus spécialement Ubuntu 20.04 LTS. Sauf pour la partie OpenShift.



Le fil rouge
============
Là ou les précédents livres n’avaient pas de fil rouge spécifique, nous avons pris un nouveau point de vue : nous avons une application, un gestionnaire de type todo list auquel on peut associer des fichiers. L’application est écrite en Java, démarre comme un service, et est accessible via un navigateur web, sur un réseau d’entreprise ou depuis Internet. Elle a besoin d’un peu de stockage pour les fichiers téléchargés, et d’une base de données (MariaDB) pour stocker les informations. Comment, d’une installation standalone sur une petite machine, peut-on passer à une solution totalement redondante et virtuellement incassable ?



C’est le fil rouge du livre, où nous abordons simplement, étape par étape, comment construire l’application, comment la modifier selon les évolutions nécessaires, comment construire une plateforme physique assurant sa propre disponibilité, comment bien configurer le réseau et ses services, son stockage local comme partagé, comment créer des clusters, comment automatiser, comment mettre en place des répartiteurs de charge, des reverse proxies, comment gérer les affinités de session… Puis nous abordons l’orchestration, les clusters Kubernetes, jusqu’à disposer d’une plateforme complète en haute disponibilité.



Les technologies
================
Tout un tas de choses sont abordées : Les architectures redondantes en haute disponibilité, le code Java, le RAID, les agrégats réseau, les DNS, [Ansible](https://en.m.wikipedia.org/wiki/Ansible_(software)), les containers ([docker](https://fr.m.wikipedia.org/wiki/Docker_(logiciel))), les reverse proxies ([NGinx](https://fr.wikipedia.org/wiki/NGINX)), les répartiteurs de charge ([HAproxy](https://en.m.wikipedia.org/wiki/HAProxy)), [Kubernetes](https://fr.wikipedia.org/wiki/Kubernetes) avec [minikube](https://minikube.sigs.k8s.io/docs/start/), puis avec un vrai cluster, la construction d’un cluster de stockage associant NFS et [XFS](https://fr.wikipedia.org/wiki/XFS), les protocoles [VRRP](https://fr.wikipedia.org/wiki/Virtual_Router_Redundancy_Protocol#:~:text=VRRP%20peut%20%C3%AAtre%20utilis%C3%A9%20sur,syst%C3%A8mes%20d'exploitation%20comme%20Linux.) et [OSPF](https://fr.wikipedia.org/wiki/Open_Shortest_Path_First), etc.

Aussi, et c’est souvent une critique faite sur ce type de solution, comment on passe d’un seul serveur à une dizaine… Mais comme tout est automatisé et mutualisé, ce n’est plus une seule application qu’on peut déployer, mais des dizaines ! Et du coup, cette critique n’a plus de sens. Sur dix serveurs bien dimensionnés et bien répartis, on fait tourner des centaines de sites sans problème.



La répartition du travail
=========================
La répartition du travail s’est faite naturellement : Charles a écrit l’application, il était plus sur  les chapitres la concernant, et une bonne partie de la présentation et l’implémentation de Kubernetes et du dernier chapitre. De mon côté, je me suis occupé des parties liées à l’infrastructure : OS, système, réseau, stockage, mais aussi une partie du clustering Kubernetes. C’était une symbiose : nos écrits et livrables s’articulent naturellement, le travail de l’un est réutilisé par l’autre. Charles comme moi avons écrit des playbooks ansible, des yaml de configuration Kubernetes.



L’écriture et l’accident OVH
============================
La mise en place de l’environnement de travail, les premiers tests d’infrastructure, l’écriture de l’application de base et des premiers playbooks ansible, démarrent en octobre 2020. L’écriture du livre, qui devait originellement contenir huit chapitres et durer deux-trois mois, démarre en décembre 2020. Soir et week-end, c’était le deal mais Covid oblige, nous avons travaillé à distance, à l’aide de nombreuses sessions de visioconférence, séances de tests et de corrections. Nous avons ajouté un neuvième chapitre, tant la description de l’installation et des tests d’un cluster Kubernetes était dense.

En mars 2021, un incendie ravage en partie un datacenter d’OVH. Dès fin février nous pensions ajouter un dixième et dernier chapitre pour aborder toute la partie « casse-pieds » pour les développeurs et les Ops. On aime bien écrire du code et monter des plateformes, mais après, il faut penser production et exploitation. Comment protéger, comment surveiller ? Que se passe-t-il si on perd tout ? L’incendie nous a fourni de la matière pour l’écriture du dernier chapitre.
Signalons aussi que rien n’est incassable. D’où le mot « virtuellement ». Un OS, des services peuvent devenir instables. Le plus grand facteur d’instabilité reste cependant l’humain, qui, au lieu de redémarrer séquentiellement les répartiteurs, les éteindra tous d’un coup, avant de se rappeler que c’est via eux qu’il était connecté…

La relecture et la sortie
=========================
En avril, nous avons pu souffler après plusieurs mois intenses. La relecture est souvent une phase compliquée. Elle a démarré en mai. Non seulement le délai pour tout rendre n’est que d’une semaine, mais il faut accepter les critiques du correcteur qui a l’œil du lecteur et le recul que nous n’avions pas lors de l’écriture. Avoir parfois une dizaine de remarques sur une page, et admettre qu’elles sont pertinentes… Cette phase terminée, c’est la création de l’index, plus rapide, mais si nécessaire.

La relecture, c’est aussi rentrer dans le concret. Les épreuves fournies par l’éditeur sont ce que sera le résultat final, et c’est gratifiant, surtout pour une première écriture. 
Enfin, deux semaines après la remise des corrections, avec parfois quelques retouches, c’est la sortie ! Le livre apparaît partout !

La sortie, c’est aussi l’occasion de partager, comme ici, notre expérience. Mais c’est aussi celui de partager notre code, sous licence libre, disponible sur github (lien plus bas).

Et après ?
==========
Nous en sommes là. Avec plein de questions : qui va lire ce livre ? Les gens vont-ils l’apprécier ? Les critiques seront-elles positives ? Comment en parler sans faire trop « pub » ? Allons-nous devenir riches (surtout que cette fois, nous avons un contrat lié aux ventes) ? 

Chers camarades Linuxiens et Linuxiennes, les auteurs espèrent que vous apprécierez autant le contenu du livre que le plaisir que nous avons eu à le rédiger. Et surtout, amusez-vous.
