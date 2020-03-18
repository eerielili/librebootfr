---
title: Listes de changements par version stable.
x-toc-enable: true
...

Les versions mises à jour de libreboot peuvent être trouvée sur [libreboot.org](/).
Les annoncements de publication de nouvelles versions de Libreboot peuvent être trouvée dans la [section des actualités](../news/) sur le site web.

Libreboot 20160907 {#release20160907}
==================

Date de publication : 7 Septembre 2016

Pour les cartes mères existantes, il n'y pas de nouveaux changements spécifiques.

Cette version ajoute une nouvelle carte mère à libreboot:

-   Carte mère de bureau Intel D945GCLF (merci à Arthur Heymans)

Autres correction de bogue :

-   Nombreuses améliorations de la documentation
-   Réajout de "unset superusers" au grub.cfg, qui était nécessaire pour quelques utilisateurs en fonction de la distribution qu'ils utilisaient.


Libreboot 20160902 {#release20160922}
==================

Date de publication: 2 Septembre 2016.

Cela corrige des problèmes dans la version précédente 20160818.
Regardez le journal des changements pour des détails.


Libreboot 20160818 {#release20160818}
==================

Date de publication: 18 Août 2016.

Les instructions d'installation peuvent être trouvée dans `docs/install`.
Les instructions de constructions (pour le code source) peuvent être trouvé dans `docs/git/\#build`.


Machines supportées dans cette version:
-----------------------------------

-   **ASUS Chromebook C201**
    -   Regardez les notes dans ***docs/hardware/c201.html***
-   **Gigabyte GA-G41M-ES2L desktop motherboard**
    -   Regardez les notes dans ***docs/hardware/ga-g41m-es2l.html***
-   **Intel D510MO desktop motherboard**
    -   Regardez les notes dans ***docs/hardware/d510mo.html***
-   **Intel D945GCLF desktop motherboard**
    -   Regardez les notes dans ***docs/hardware/d945gclf.html***
-   **Apple iMac 5,2**
    -   Regardez les notes dans ***docs/hardware/imac52.html***
-   **ASUS KFSN4-DRE server board**
    -   PCB revision 1.05G is the best version (can use 6-core CPUs)
    -   Regardez les notes dans ***docs/hardware/kfsn4-dre.html***
-   **ASUS KGPE-D16 server board**
    -   Regardez les notes dans ***docs/hardware/kgpe-d16.html***
-   **ASUS KCMA-D8 desktop/workstation board**
    -   Regardez les notes dans ***docs/hardware/kcma-d8.html***
-   **ThinkPad X60/X60s**
    -   Vous pouvez aussi enlever la carte mère d'un X61/X61s et la remplacer avec une carte mère X60/X60s. Une carte d'une X60 Tablet rentrera aussi dans l'intérieur d'un X60/X60s.

-   **ThinkPad X60 Tablet** (résolution 1024x768 et 1400x1050) avec support pour scanneur


    -    ***docs/hardware/\#supported\_x6Ot\_list*** pour la lste d'écrans LCD supportés.
    -   C'est inconnu s'il se peut qu'une X61 Tablet ait sa carte mère remplacée par une carté mère d'une X60 Tablet.


-   **ThinkPad T60** (Intel GPU) (il y a des problèmes; regardez en dessous):

    -   Regardez les notes ci-dessous pour les exceptions et 
        ***docs/hardware/\#supported\_t60\_list*** pour une liste connue d'écrans LCD fonctionnels.

    -   C'est inconnu s'il se peut qu'un T61 ait sa carte mère remplacée par une carté mère d'un T60.
    -   Regardez **docs/future/\#t60\_cpu\_microcode***.
    -   Le T60p (et ordinateurs portables avec carte graphiques ATI) ne sera surêment jamais supporté : ***docs/hardware/\#t60\_ati\_intel***.


-   **ThinkPad X200**
    -   **ME/AMT**: libreboot enlève ceci, permanemment.
    ***docs/hardware/gm45\_remove\_me.html***

-   **ThinkPad R400**
    -   Regardez **docs/hardware/r400.html**
    -   **ME/AMT**: libreboot enlève ceci, permanemment.
    ***docs/hardware/gm45\_remove\_me.html***

-   **ThinkPad T400**
    -    Regardez ***docs/hardware/t400.html***
    -   **ME/AMT**: libreboot enlève ceci, permanemment.
    ***docs/hardware/gm45\_remove\_me.html***

-   **ThinkPad T500**:
    -   Regardez ***docs/hardware/t500.html***
    -   **ME/AMT**: libreboot enlève ceci, permanemment.
        ***docs/hardware/gm45\_remove\_me.html***

-   **Apple MacBook1,1** (MA255LL/A, MA254LL/A, MA472LL/A)
    -   Regardez ***docs/hardware/\#macbook11***.
-   **Apple MacBook2,1** (MA699LL/A, MA701LL/A, MB061LL/A, MA700LL/A,
    MB063LL/A, MB062LL/A)
    -   Regardez ***docs/hardware/\#macbook21***.


Changements pour cette version, relatif à r20150518 (les changements les plus récents en dernier, les changements récents en premier)
---------------------------------------------------------------------------------------------

Libreboot 20160818

-    NOUVELLES CARTES MÈRES AJOUTÉES:
       -   ASUS Chromebook C201 (ordi portable ARM) (grâce à Paul Kocialkowski)
       -   Gigabyte GA-G41M-ES2L carte mère (bureau) (grâce à Damien Zammit)
       -   Intel D510MO carte mère (bureau) (grâce à Damien Zammit)
       -   ASUS KCMA-D8 carte mère (bureau) (grâce à Timothy Pearson)
       -   ASUS KFSN4-DRE carte mère (serveur) (grâce à Timothy Pearson)
       -   ASUS KGPE-D16 carte mère (serveur) (grâce à Timothy Pearson)


Pour les cartes précedemment supportées, de nombreuses corrections venant d'amont ont été fusionnées.

Autre changements (par rapport à la version libreboot 20150518):
(ceci est un résumé. Pour une liste plus détaillée des changements, veuillez vous référer au git log).

256Mo VRAM alloué sur la GM45 (X200, T400, T500, R400) au lieu de 32Mo.
C'est une amélioration par rapport au BIOS Lenovo et Libreboot 20150518, permettant le décodage vidéo à 1080p d'être plus fluide. (merci Arthur Heymans). Pour clarifier, la performance vidéo GM45 dans libreboot 20160818 est meilleure que le BIOS originel et la précédente version.

64Mo VRAM sur i945 (X60, T60, MacBook2,1) maintenant supporté dans coreboot-libre, et utilisé par défaut (dans les précédentes versions, il y avait 8Mo alloué). Merci à Arthur Heymans.

Une meilleure durée de vie de la batterie sur la GM45 (X200, T400, T500, R400) dû au niveau plus haut de cstates maitenant supportés.(merci Arthur Heymans). États de puissances C4 maintenant supportés.

Le mode texte de la GM45 (X200, T400, T500, R400) marche maitenant, permettant d'utiliser MemText86+ comfortablement. (grâce à Nick High de coreboot)

Les affichages LVDS à double canaux sur la GM45 (T400, T500) sont maintenant automatiquement détecté coreboot-libre. (merci Vladimir Serbinenko de coreboot)

Résolution partielle dans coreboot-libre par rapport à l'affichage du GRUB sur la GM45, résolution plus haute pour les écrans LCD pour les affichages LVDS à double canaux (T400, T500). (merci Arthur Heymans)

Améliorations massive de la configuration du GRUB, rendant plus facile le démarrage automatique
de nombreux systèmes d'exploitations chiffré, et généralement un menu plus utile pour démarrer (les remerciements grâce à Klemens Nanni du projet autoboot).
Libreboot utilise maintenant automatiquement le grub.cfg fourni par la distribution
GNU+Linux installée, si présente, passant sur cette configuration. Celà est fait à travers de multiples partitions où libreboot cherche activement pour un fichier de configuration
(notamment sur les volumes chiffrés et LVM). Celà devrait rendre Libreboot plus facile
à utiliser pour les utilisateurs non techniques, sans avoir à modifier la
configuration GRUB utilisée dans libreboot.

Les archives des utilitaires sont maitenant en mode source seulement . Vous aurez besoin de compiler les paquets
dedans (scripts de builds sont compris, et un script pour installer les dépendances du build). (les archives de fichiers binaires sont de nouveau prévues dans une nouvelle version où le nouveau système de build sera fusionné).

SeaGRUB est maintenant le payload par défaut des cartes x86. (SeaBIOS configuré pour immédiatement charger un payload GRUB depuis CBFS sans fournir une interface dans SeaBIOS. De cette façon, GRUB est encore utilisé mais maintenant les services du BIOS sont disponibles, vous obtenez donc le meilleur des deux mondes). Les remerciements reviennent à Timothy Pearson
de coreboot pour cette idée.

crossgcc est maintenant téléchargé et construit en tant que module séparé de coreboot-libre,
avec une révision universelle utilisée pour build toutes les cartes.

Les cartes mères spécifiques ont leur propre patchs et modification de coreboot, indépendantes des autres autres. Celà rend la maintenance plus facile.

Mise à jour de tous les utilitaires et modules (coreboot, GRUB, etc) vers des versions plus récentes,
avec de nombreuses correction de bogues et améliorations en amont.

Le problème de l'octect "siècle" de l'Horloge Temps Réél (RTC/HTR) est mainteant corrigé sur la GM45 dans coreboot-libre, donc la date devrait maitenant s'afficher
correctement lors de l'utilisation du dernier kernel linux, au lieu de voir 1970-01-01 au démarrage  (grâce à Alexander Couzens de coreboot)

Le système de build utilise maintenant plusieurs coeurs d'un CPU, accélérant le build pour certaine personnes.
La spécification manuelle du nombre de coeurs a utiliser est possible pour ceux qui utilise le système de build dans un environnement chrooté (les remerciements vont à
Timothy Pearson de coreboot).

Dans le système de build (répo git), https:// est maitenant utilisée lors du clonage de coreboot.
http:// est utilisé en tant qu'alternative pour GRUB si git:// échoue

Nouveau chargeur d'amorçage, depthcharge (libre et maintenu par Google)
prêt à l'utilisation sur l'ASUS Chromebook C201. (les remerciements vont à Paul Kocialkowski)


Nombreuses corrections à l'utilitaire ich9gen (p.e maintenant la densité du composant flash est correctement mise dans la description, les descriptions gbe-less sont maintenant supportées.)


Version 20150518 {#release20150518}
================

Date de publication : 18 May 2015.

Instructions d'installation peuvent être trouvé à ***docs/install/***
Instructions de constructions (pour le code source) peuvent être trouvée dans ***docs/git/\#build***.


Machines supportés dans cette version:
-----------------------------------

-   **ThinkPad X60/X60s**
    -   Vous pouvez aussi enlever la carte mère d'un X61/X61s et la remplacer avec une carte mère d'un X60/X60s. Une carte mère d'une X60 Tablet rentrera aussi à l'intérieur d'une X60/X60s
-   **ThinkPad X60 Tablet** (1024x768 et 1400x1050) avec support pour scanneur
   -   Regardez ***docs/hardware/\#supported\_x60t\_list*** pour une liste des écrans LCD supportés
   -   C'est inconnu s'il se peut qu'un X61 Tablet peut avoir sa carte mère remplacée par une carte mère d'une X60 Tablet.
-   **ThinkPad T60** (Carte graphique Intel) (il y a des problèmes; regardez ci-dessous):
    -   Regardez les notes ci-dessous pour des exceptions, et ***docs/hardware/\#supported\_t60\_list*** pour les écrans LCDs connus pour marcher.
    -   C'est inconnu s'il se peut qu'un T61 peut avoir sa carte mère remplacée par une carte mère d'un T60.
    -   Regardez ***docs/future/\#t60\_ati\_intel***
-   **ThinkPad X200**
    -   X200S et X200 Tablet sont aussi supportés, conditionnellement; regardez
        ***docs/hardware/x200.html\#x200s***
    -   **ME/AMT**: libreboot enlève ceci, définitevement.
        ***docs/hardware/gm45\_remove\_me.html***
-   **ThinkPad R400**
    -   Regardez ***docs/hardware/r400.html***
    -   **ME/AMT**: libreboot enlève ceci, définitevement.
        ***docs/hardware/gm45\_remove\_me.html***
-   **ThinkPad T400**
    -   Regardez ***docs/hardware/t400.html***
    -   **ME/AMT**: libreboot enlève ceci, définitevement.
        ***docs/hardware/gm45\_remove\_me.html***
-   **ThinkPad T500**
    -   Regardez ***docs/hardware/t500.html***
    -   **ME/AMT**: libreboot enlève ceci, définitevement.
        ***docs/hardware/gm45\_remove\_me.html***
-   **Apple MacBook1,1** (MA255LL/A, MA254LL/A, MA472LL/A)
    -   Regardez ***docs/hardware/\#macbook11***.
-   **Apple MacBook2,1** (MA699LL/A, MA701LL/A, MB061LL/A, MA700LL/A,
    MB063LL/A, MB062LL/A)
    -   Regardez ***docs/hardware/\#macbook21***.

Changements pour cette version, relatif à r20150518  (les changements les plus récents en dernier, les changements récents en premier)
---------------------------------------------------------------------------------------------

-   Ajoute une donnée de liste blanche dans board\_enable.c dans la flashrom, pour le ThinkPad R400, T400 et T500
-   Flashrom mis à jour (à la révision SVN 1889)
     -  Patch de la liste blanche pour le X200 enlevé (fusionné depuis l'amont)
     -  Liste blanche X200 modifiée pour inclure le X200S et le X200 Tablet
-   libreboot\_util: n'inclut pas les fichiers d'agencement du cmos (ne sont plus nécessaires)
-   **coreboot_libre: Patchs rétroactifs pour le support du scanneur pour les X200 Tablet**
-   build/release/archives: créé un fichier manifeste de somme SHA512 des archives de version.
-   build/release/archives: séparation de crossgcc dans une nouvelle archive
-   désactivé la génération d'images ROM txtmode pour maintenant ( elles seront de retour de nouveau encore dans une prochaine version)
-   coreboot-libre: supprimer le code inutilisé (réduit la taille de l'archive src)
-   Guides de flashage: rendus plus amicaux pour les personnes daltonienne.
-   docs/gnulinux/encrypted\_\*.html: suppression de la mention de la longueur du mot de passe - c'était arbitraire et sans intérêt.
-   docs/maintain/: Finir le guide
-   scripts/download/coreboot: utilise les diffs incluses dans libreboot, pas quelques bon bouts du gerrit extérieur - review.coreboot.org (gerrit) étant hors service ne tue plus libreboot (des mirroirs de sauvegardes du répertoire maître existent).
-   docs/install/bbb\_setup.html: ajout d'infos à propos de wp/hold et pinouts
-   docs/: améliore la description de libreboot
-   docs/hardware/gm45\_remove\_me.html: notes à propos de l'utilitaire demefactory.
-   docs/install/bbb\_setup.html: débug EHCI : recommander linux-libre.
-   docs/install/bbb\_setup.html: Guide de mise en place des fichiers journaux de débug de l'EHCI.
-   docs/hardware/t500.html: ajout du rapport de compatibilité des écrans (ÀFAIRE: corriger les écrans incompatibles)
-   Met à jour coreboot(encore) + fusionne les patchs de carte graphiques hybride GM45 - celà signifie que la mise en place d'un T400/T500 avec GPU hybride ATI+Intel marchera (ATI désactivé, Intel activé définitivement). L'option power\_on\_after\_fail de la nvram ajoutée à toutes les cartes mères GM45, par défaut sur Non, comme ça la brancher sur secteur ne démarre pas le système contre la volonté de l'utilisateur. Net20DC est maintenant le boîtier de déboguage par défaut sur toutes les cartes mères (compatibles avec BBB).
-   demefactory (nouvel utilitaire): créée la factory.rom sans la ME
-   ich9deblob: re-façonnage des fonctions de descriptor.c
-   docs/hardware/t500.html: ajout des journaux matériels
-   docs/gnulinux/encrypted\_\*.html: pas de mot de passe pour la saisie par défaut
-   docs/git/: Ajout de plus détails à propos de BUC.TS
-   grub.cfg: Scanne aussi pour grub2/grub.cfg, pas juste grub/grub.cfg
-   docs/maintain/ (nouvelle section. Travail en cours !): Faire la maintenance de libreboot
-   docs/gnulinux/grub\_boot\_installer.html: Correction d'une instruction hasardeuse
-   docs/tasks.html: meilleure catégorisation entre intel/amd/arm
-   docs/install/bbb\_setup.html: notes à propos de la stabilité de l'utilitaire de flashage SPI.
-   docs/install/bbb\_setup.html: plus de noms pour les cables de 0.1 pouce.
-   docs/install/\*\_external.html: ajout d'un avis de non responsabilité à propos de la pâte thermique.
-   docs/install/bbb\_setup.html: Correction des liens cassés
-   docs/install/bbb\_setup.html: notes préliminaires à propos du déboguage EHCI.
-   docs/hardware/gm45\_remove\_me.html: lien vers les sites webs parlant de la ME.
-   docs/install/{t400,t500,r400}\_external.html: notes à propos de la compatibilité des processeurs (CPU).
-   Supprime le script ich9macchange. Il est inutile et embrouille les gens.
-   docs/hardware/gm45\_remove\_me.html: prioritisation du chemin d'éxecutable d'ich9gen.
-   docs/hardware/gm45\_remove\_me.html: prioritisation du changement d'adresse MAC. 
-   docs/hardware/gm45\_remove\_me.html: notes moins embrouillantes à propos d'ich9gen
-   build/dependencies/parabola: Ajout de dépendances pour l'architecture x86_64.
-   scripts/dependencies/paraboladependencies: dépendances de constructions (pour 32-bit Parabola).
-   **Nouvelle carte mère**: ThinkPad T500
-   Ajout des diffs pour les différences de descripteurs/gbe entre le T500 et X200
-   coreboot-libre: fournit une meilleure catégorisation des blobs.
-   docs/hardware/gm45\_remove\_me.html: ajout de notes à propos de la protection d'écriture flash.
-   **Nouvelle carte mère**: ThinkPad T400
-   GRUB: ajout d'un support partial de vesamenu.c32 (corrige la fin du menu ISOLINUX)
-   Mise à jour du GRUB (à la révision fa07d919d1ff868b18d8a42276d094b63a58e299).
-   Mise à jour de coreboot (à la révsion 83b05eb0a85d7b7ac0837cece67afabbdb46ea65)
    -   Le microcode du CPU Intel (la majorité de) n'est plus supprimé désormais, parce que c'était supprimé en amont (bougé dans un répertoire tierce partie).
    -   Le patch du cstate pour Macbook2,1 n'est plus choisi sur le volet (fusionné de l'amont).
    -   Le patch pour désactiver l'utilisation de l'horodatage dans coreboot n'est plus inclus (fusionné de l'amont)
-   coreboot-libre: ne pas lister le micrologiciel kbd vortex86ex en tant que microcode 
-   coreboot-libre: ajout de la license GPLv3 aux scripts findblobs.
-   coreboot-libreboot: ne supprime pas raminit\_tables (nahelem/sandybridge) (ils ne sont pas des blobs)
-   coreboot-libre: ne supprime pas les fichiers .spd.hex (ils ne sont pas des blobs).
-   build/release/archives: ne met pas rmodtool dans libreboot\_util
-   docs/install/x200\_external.html: recommande l'installation de GNU+Linux à la fin.
-   docs/install/x200\_external.html: ajoute plus de photos, améliore les instructions
-   build/clean/grub: utilise distclean au lieu de clean
-   grub-assemble: Ajoute les modules *bsd* et *part\_bsd*
-   build/roms/withgrub: n'éxecute seulement ich9gen si les images de gm45/gs45 existe
-   docs/git/: ajout de notes à propos de la compilation pour certaines cartes mères spécifiques.
-   build/roms/withgrub: permet la compilation pour une portée customisée de cartes mères.
-   grub-assemble: désactive la sortie verbeuse.
-   Ajoute de la documentation sur comment déverouiller le système de fichiers chiffré avec clé dans initramfs dans Parabola Linux
-   docs/gnulinux/grub\_cbfs.html: amélioration de la structure (plus facile à utiliser)
-   grub.cfg: Désactive le beep au démarrage.
-   docs/install/bbb\_setup.html: rend le guide plus facile à utiliser
-   docs/gnulinux/grub\_cbfs: supprime les instructions redondantes
-   docs/install/x200\_external.html: met des punaises dans les images.
-   docs/install/bbb\_setup.html: remplace la photo du bloc d'alimentation 3.3V avec un bloc d'alimentation ATX
-   docs/hardware/x200.html: ajout de décharges du X200 4Mo avec BIOS Lenovo v3.22
-   docs/hardware/x200.html: ajout de décharges du X200 4Mo avec BIOS Lenovo v3.18
-   grub.cfg: ajout d'une entrée de menu syslinux\_configfile pour ahci0
-   grub.cfg: ajout de plus de chemins pour syslinux\_configfile
-   docs/future.html: T60:  ajout d'une décharge EDID du LG-Philips LP150E05-AK21
-   docs/install/bbb\_setup.html: clarifie encore plus quel clip est nécessaire
-   scripts bash: rend la sortie du script plus amicale pour l'utilisateur en général.
-   scripts bash: active seulement la sortie verbeuse si DEBUG= est utilisé.
-   build: Supporte de nombreuses options extras - maintenant possible de compiler de multiples images pour les cartes mères
de votre choix (configs), mais sans construire l'entière collection.
-   A supprimé la clé de signement de l'archive - au lieu de cela l'empreinte et l'ID sont donnés, comme ça l'utilisateur peut le télécharger depuis un serveur de clé.
-   scripts/helpers/build/release: déplace les documentations pour séparer l'archive - réduit la taille des autres archives considérablement.
-   déplace le fichier DEBLOB dans resources/utilities/coreboot-libre/deblob
-   scripts/helpers/build/release: supprime DEBLOB de libreboot\_src/ - pas nécessaire dans libreboot\_src (archive de version) parce qu'il contient une révision de coreboot qui a déjà été déblobbé.
-   flash (script): Utilise *build* au lieu de *DEBLOB* pour savoir si on est dans src
-   docs/install/r400\_external.html: Montre les images, ne pas mettre de liens.
-   docs/install/x200\_external.html: Montre les images, ne pas mettre de liens.
-   docs/install/bbb\_setup.html: Montre les images, au lieu de mettre des liens
-   Documentation: optimise toutes les images (réduit la taille des fichiers)
-   Enlève les liens de téléchargements sur la page de publication de version ( et de la page d'archive) - les archives de versions sont hébergés différement à la suite de cette version,ce qui signifie que les vieilles méthodes ne sont plus viables.
-   A déplacé ich9macchange dans resources/scripts/misc/ich9macchange
-   ich9macchange: on assume qu'il est éxecuté depuis \_util (agit seulement sur une seule image ROM, définie par un chemin fournit par l'utilisateur).
-   Déplace grub-background dans resources/scripts/misc/grub-background
-   grub-background: on assume qu'il est éxecuté depuis libreboot\_util
-   grub-background: change seulement une image ROM, spécifié par chemin
-   build (archives de version): ajoute le fichier commitid dans release/
-   build-release: déplace les archives de versions dans release/
-   Fusionne tout les scripts de compilation/construction dans un seul script générique, avec les helpers (scripts de fonctions) dans resources/scripts/helpers/build/
-   Remplace *getall* avec *download*, qui prend en entrée un argument spécifiant quel programme l'utilisateur veut télécharger
-   A déplacé les scripts de téléchargements dans resources/scripts/helpers/download
-   build-release: enlève les entrées powertop
-   Documentation: améliorations générales des instructions de flashage.
-   Fusionne tous les scripts de flashage dans un seul script.-   Mis à jour GRUB
-   bucts: le rendre compilable sans git
-   A déplacé dejavu-fonts-tff-2.34/AUTHORS dans resources/grub/font/
-   A supprimé GRUB Invaders de libreboot
-   A supprimé SeaBIOS de libreboot
-   build-release: optimise l'utilisation de tar (taille des fichiers réduites)
-   grub.cfg: ajoute un autre chemin pour la configuration SYSLINUX (/syslinux/syslinux.cfg)
-   build-release: enlève le répertoire bin/ de libreboot\_util
-   cleandeps: enlève le répertoire bin/
-   buildrom-withgrub: créer le répertoire bin si il n'existe pas
-   coreboot-libre: n'utilise pas git pour l'horodatage de version
-   i945-pwm: ajout d'une commande de nettoyage (clean) dans le Makefile
-   i945-pwn: ajout de -lz à la Makefile.
-   docs/install/x200\_external: Mentionne le mode non-descripteur de la GPIO33
-   docs/hardware/: enlève les liens redondants
-   ich9macchange: ajout de R400
-   build-release: sépare les images de ROM dans des archives individuelle
-   build-release: renomme libreboot\_bin en libreboot\_util
-   **Nouvelle carte mère**: le support du ThinkPad R400 a été ajouté à libreboot.
-   bbb\_setup.html: dis à l'utilisateur d'utiliser la flashrom propre à libreboot.


Version 20150124, 20150126 et 20150208 {#release20150124}
=======================================

Date de publication: 24 Janvier 2015.

Machines supportées dans cette version:
-----------------------------------

-   **Lenovo ThinkPad X60/X60s**
    -   Vous pouvez aussi enlever la carte mère d'un X61/X61s et le remplacer avec la carte mère d'un X60/X60s.
        La carte mère d'un X60 Tablet rentrera aussi à l'intérieur d'un X60/X60s.
-   **Lenovo ThinkPad X60 Tablet** (résolution 1024x768 et 1400x1050) avec support pour scanneur
    -   Voir **hardware/\#supported\_x60t\_list** pour une liste d'écrans LCD supportés
    -   C'est inconnu si une X61 Tablet peut avoir sa carte mère remplacé par celle d'une X60 Tablet.
-   **Lenovo ThinkPad T60** (carte graphique Intel) (il y a des problèmes install/x200\_external.html; regardez ci-dessous):
    -   Regardez les notes ci-dessous pour des exceptions, et **hardware/\#supported\_t60\_list** pour une liste d'écrans LCD connus pour marcher.
    -   Il est inconnu si un T61 peut avoir sa carte mère remplaçée avec celle d'un T60.
    -   Voir **future/\#t60\_cpu\_microcode**.
    -   Le T60p (et ordinateurs T60 avec carte graphique ATI) ne seront sûrement jamais supportés: **hardware/\#t60\_ati\_intel**
-   **Lenovo ThinkPad X200**
    -   X200S et X200 sont aussi supportés, conditionnellement; voir
        **hardware/x200.html\#x200s**
    -   **ME/AMT**: libreboot enlève ceci, définitivement.
        **hardware/gm45\_remove\_me.html**
-   **Apple MacBook1,1** (MA255LL/A, MA254LL/A, MA472LL/A)
    -   Voir **hardware/\#macbook11**.
-   **Apple Macbook2,1** (MA699LL/A, MA701LL/A, MB061LL/A, MA700LL/A, MB063LL/A, MB062LL/A)
    -   Voir **hardware/\#macbook21**.

Modifications pour r20150208 (relatif à 20150126)
-----------------------------------------------

C'est une version de maintenance (de polissage) basé sur r20150126. Les utilisateurs qui ont installé la r20150126 n'ont pas vraiment besoin de mettre à jour à cette version.

-   buildrom-withgrub : utilise le fond d'écran gnulove.jpg sur les ordinateurs portables avec un ratio écran 16:10 (MacBook2,1 et X200)
-   build-release: inclut le script grub-background dans libreboot\_bin
-   grub-background (nouveau): laisse l'utilisateur choisit le fond d'écran du GRUB.
-   grub-assemble: ajout d'un lien à l'utilitaire original.
-   buildrom-withgrub: Met background.jpg dans CBFS, non dans le disque mémoire GRUB.
-   grub-assemble: fusionne les scripts en un seul script gen.sh
-   Documentation: implémente un thème, améliore drastiquement la lisibilité
-   docs/hardware/: met à jour une liste des écrans LCD compatibles avec le T60.
-   docs/: plus de clarification sur le but établi de libreboot.
-   build-release: inclut le fichier commitid dans les archives de version.
-   docs/: plus souligner la nécessité de GNU+Linux
-   lenovobios\_firstflash: corrige les erreurs BASH
-   lenovobios\_secondflash: corrige les erreurs BASH
-   docs/install/x200\_external.html: dit à l'utilisateur de changer son adresse MAC.
-   docs/git/: ajoute à la liste des hôtes compatibles avec l'architecture x86_64.
-   docs/install/: enlève les vieilles informations (obsolètes).
-   docs/git/: indiquer que les dépendances de compilation sont pour src (et non nécessaire pour libreboot\_bin) 
-   build: 
-   X60, X60S et X60 Tablet partagent maintenant les même images de ROM.
-   Ajoute le support de QEMU (q35/ich9) à libreboot
-   Ajout le support de QEMU (i440x/piix4) à libreboot
-   docs/: réécrit la description de ce qu'est libreboot.
-   docs/release.html: ajoute des notes sur comment utiliser GPG.
-   build-release: supprime le fichier commitid des archives de versions
-   build-release: créé un fichier nommé commitid après build-release


Modifications pour r20150126 (relatif à r20150124)
-----------------------------------------------
C'est une version de correction de bogue basé sur r20150124. Elle contient quelques petits changements: 
-   grub.cfg: code en dur la liste des partitions à chercher (accélère le démarrage considérablement. L'expression régulière du GRUB n'est pas très bien optimisée)
-   Docs (x200.html hcl): enlève les informations incorrectes
 -   Documentation (bbb\_setup.md): Corrige les erreurs d'orthographes
-   build-release: supprime les fichiers ich9fdgbe\_{4m,8m}.bin du dossier ich9gen
    -   Ils ont été inclus accidentellement dans la version r20150124.
        Ils sont générés depuis ich9gen donc c'est ok, mais ils n'ont pas besoin d'être dans l'archive.
-   Documentation (grub\_cbfs.md): boucle dans libreboot\_grub.cfg (ajoute des notes à propos de cela si l'utilisateur l'a copié depuis grub.cfg dans CBFS).


Changements pour cette version (les derniers changements en premier, les plus récents en dernier)
----------------------------------------------------------------------

-   grub.cfg: a ajouté (ahci1) à la liste des appareils pour l'analyseur syntaxique (CD/DVD) d'ISOLINUX (ceci est nécessaire pour le dock du X200).
-   grub.cfg: l'analyse syntaxique d'ISOLINUX est maintenant faite sur toutes les partitions USB.
-   grub.cfg: change automatiquement sur /boot/grub/libreboot\_grub.cfg sur une partition, si le fichier existe.
-   libreboot\_bin: a ajouté les éxecutables statiques sur architecture ARM pour flashrom, cbfstool, ich9gen et ich9deblob (testé sur beaglebone black).
-   Flashrom: a enlevé les définitions redondantes de la puce de flashage (flashchip) (pour les propriétaires d'X200).
-   Flashrom: a ajouté une liste blanche pour le ThinkPad X200.
-   X200: a corrigé un rétroéclairage inégal (à bas niveaux)
-   ich9macchange (nouveau script, utilise ich9gen): pour changer l'adresse MAC par défaut pour les images ROM du X200.
-   ich9gen: a ajouté la capacité de changer l'adresse MAC par défaut (et met à jour la somme de contrôle)
-   ich9deblob: a ajouté un nouvel utilitaire ich9gen: cela peut générer une image descriptor+gbe sans la présence d'un dump factory.rom.
-   A modifié ich9deblob afin d'utiliser une structure pour Gbe, documentant le tout.
-   A mis à jour massivement l'utilitaire ich9deblob: a complétement tout retravaillé.
-   A activé les cstates 1 et 2 sur macbook21. Cela réduit la chaleur au repos / consommation électrique.
-   buildrom-withgrub: a désactivé la création de \*txtmode\*.rom pour le X200 (seulement le tampon d'affichage graphique marche)
-   A mis à jour SeaBIOS (encore)
-   docs/install/\#flashrom\_x200: améliore les instructions
-   A mis à jour flashrom (encore) - a mis à jour les patchs
-   A mis à jour GRUB (encore)
-   A mis à jour coreboot (encore)
-   build-release: pas tout les fichiers étaient copié dans libreboot\_src. corrige cela.
-   build-release: inclus cbmem (compilé statiquement) dans libreboot\_bin
-   Documentation (X200): a ajouté les instructions pour le flashage basée logiciellement.
-   Documentation: a enlevé toutes les références du connecteur pirate (a été remplacé avec des tutoriels de flashages BBB).
-   **Nouvelle carte mère:** a ajouté le support du ThinkPad X200S et X200 dans libreboot
-   build: trouve automatiquement les noms de cartes mères (configs) afin de compilé pour celles-ci
-   **Nouvelle carte mère:** a ajouté le support du ThinkPad X200 dans libreboot
-   configuration de coreboot-libre (toutes cartes mères): active la sortie du journal de clé USB (pour BeagleBone Black)
-   cleandeps: en fait, nettoyer les grubinvaders
-   .gitignore: ajoute le répertoire powertop
-   cleandeps: nettoie l'utilitaire i945-pwn
-   scripts (tous): corrige les fautes de frappes
-   Documentation: nettoyage général.
-   builddeps-flashrom: réduit les commandes de compilations dans une seule boucle pour/for.
-   scripts (tous): remplace les rm -Rf non nécessaires avec rm -f
-   docs/release.html: ajoute lenovo g505s à la liste des candidats
-   .gitignore: ajoute libreboot\_bin.tar.xz et libreboot\_src.tar.xz
-   libreboot\_bin.tar.xz: inclut les utilitaires en tant que binaires liés statiquement
    -   Cela veut dire que l'utilisateur n'a plus à installer les dépendances de compilation ou à compiler depuis la source.
-   deps-parabola (enlevé) enlève le script des dépendances de Parabola. Rajoutera plus tard (proprement testé)
-   grub.cfg: ajoute plus de chemins à vérifier à l'analyseur syntaxique isolinux (plus d'images ISOs devrait marcher maintenant)
-   Met à jour SeaBIOS
-   x60flashfrom5 (nouveau), pour les utilisateurs de X60 mettant à jour depuis la 5ième/récente version.
-   Met à jour flashrom
-   Met à jour GRUB
-   A mis à jour coreboot-libre
    -   i945: a établi permanemment tft\_brightness à 0xff (corrige un bogue sur l'X60 où monter la luminosité au max ferait retourner en basse luminosité).
-   getcb: révocations des contrôles de rétroéclairage aux origines pour le X60/T60.
    -   Les patchs de luminosité ACPI ont été abandonnés et rendus obsolètes.
-   grub.cfg: charge seulement initrd.img si cela existe. Ajoute rw à la ligne linux (pour ProteanOS).
-   build: Génère seulement les configurations GRUB une seule fois (réutilise sur toutes les images)
-   Compile seulement 2 charges utiles (payload) GRUB éxecutables, réutilise sur toutes les cartes mères.
-   resources/utilities/grub-assemble/gen.txtmode.sh: Utilise GNU BASH\
    resources/utilities/grub-assemnle/gen.vesafb.sh: Utilise GNU BASH
-   scripts (traitement des erreurs): remplace exit avec exit 1 (rend le déboguage plus facile)
-   Bouge la majorité des fichiers dans CBFS dans le disque mémoire GRUB, sauf grub.cfg et grubtest.cfg
-   docs/release.html Ajoute le processeur DM&P vortex86ex à la liste des candidats.
-   docs/release.html Ajoute le ThinkPad X201 à la liste des candidats.
-   Nouveaux liens ajouté à docs/security/x60\_security et docs/security/t60\_security
-   lenovobios\_secondflash: avertit si BUCTS n'est pas présent (ce n'est pas un drame. Pouvez juste enlever la batterie/pièce nvram).
-   lenovobios\_firstflash: échoue si BUCTS échoue. (précaution d'anti-bousillage)
-   A enlevé des avertissements déplaisants des scripts de flashage, a amélioré la documentation en contrepartie.
-   scripts (tous): ajoute une vérification d'erreur propre (échoue rapidement, échoue tôt. Ne continue plus c'il y a des erreurs)
-   buildrom-withgrub: renomme les images en boardname\_layout\_romtype.rom
-   buildrom-withgrub: ne bouge pas cbfstool, exécute directement
-   resources/utilities/grub-assemble: ajoute la disposition de clavier française Dvorak (BEPO).
-   Documentation: ajoute docs/hardware/x60\_keyboard.html (montre comment remplacer le clavier sur X60/X60T)
-   Documentation: nettoyage majeur (meilleure structure, plus facile pour trouver des choses)
-   docs/release.html: enlève l'Acer CB5 de la liste des futurs candidats.
   -   Trop de problèmes. Les Chromebooks sont limités (RAM/stockage/wifi soudé) et ont trop de problèmes ergonomiques pour le projet libreboot.
-   docs/gnulinux/grub\_cbfs.html nettoyage majeur. Améliorations ergonomiques.
-   flash (script flashrom): enlève boardmismatch=force
    -   C'était mis là avant pour les utilisateurs mettant à jour de libreboot r5 à libreboot r6, mais permet aussi à l'utilisateur de flasher la mauvaise image. Par example, l'utilisateur pouvait flasher l'image d'un T60 sur un X60, et donc bousiller le système. Il est presque certain que la majorité des pergens ont mis à jour maintenant, donc nous enlevons cette option potentiellement dangereuse.
-   Documentation: met à jour la liste de compatibilité pour les écrans LCD X60T.
-   docs/release.html: ajoute une note à propos de la carte mère du X60 Tablet dans les X60/X60s.
-   docs/howtos/grub\_boot\_installer.html: petites corrections
-   docs/howtos/grub\_boot\_installer.html: lisibilité améliorée, a corrigé les erreurs html.
-   Documentation (en lien avec le macbook21): nettoyage général. 


Version 20141015 {#release20141015}
================

Machines supportées dans cette version:
-----------------------------------

-   **Lenovo ThinkPad X60/X60s**
    -   Vous pouvez aussi enlever la carte mère d'un X61/X61s et le remplacer avec la carte mère d'un X60/X60s. La carte mère d'une X60 Tablet pourra aussi rentrer à l'intérieur d'un X60/X60s.
-   **Lenovo ThinkPad X60 Tablet** (résolutions 1024x768 et 1400x1050) avec support pour scanneur.
    -   Regardez **hardware/\#supported\_x60t\_list** pour une liste d'écran LCDs supportés.
    -   C'est inconnu s'il se peut qu'une X61 Tablet ait sa carte mère remplacée par une carté mère d'une X60 Tablet.
-   **Lenovo ThinkPad T60** (carte graphique Intel) (il y a des problèmes, regardez ci-dessous):
    -   Regardez les notes pour les exceptions, et **hardware/\#supported\_t60\_list** pour des écrans LCD connus pour marcher.
    -   C'est inconnu s'il se peut qu'une T61  ait sa carte mère remplacée par une carté mère d'une T60.
    -   Voir **future/\#t60\_cpu\_microcode**.
    -   Le T60p (et variante du T60 avec carte graphique ATI) ne sera sûrement jamais supporté: 
        **hardware/\#t60\_ati\_intel**
-   **Apple MacBook1,1** (MA255LL/A, MA254LL/A, MA472LL/A)
    -   Voir **hardware/\#macbook11**.
-   **Apple MacBook2,1** (MA699LL/A, MA701LL/A, MB061LL/A, MA7600LL/A, MB063LL/A, MB062LL/A)
    -   Voir **hardware/\#macbook21**.




Changements pour cette version (les derniers changements en premier, les plus récents en dernier)
----------------------------------------------------------------------

-   A mis à jour coreboot (git commit 8ffc085e1affaabbe3dca8ac6a89346b71dfc02e), la dernière version au temps de l'écriture.
-   A mis à jour SeaBIOS (git commit 67d1fbef0f630e1e823f137d1bae7fa5790bcf4e), la dernière version au temps de l'écriture.
-   A mis à jour Flashrom (révision svn 1850), la dernière version au temps de l'écriture.
-   A mis à jour GRUB (git commit 9a67e1ac8e92cd0b7521c75a734fcaf2e58523ad), la dernière version au temps de l'écriture.
-   A nettoyé la documentation, enlève les fichiers non nécessaires.
-   ec/lenovo/h8 (x60/x60s/x60t/t60): active le wifi/bluetooth/wwan/pavé tactile/dispositif de pointage par défaut.
-   Documentation: a mis à jour la liste des écrans LCD du T60 (Samsung LTN150XG 15" XGA listé comme ne marchant pas).
-   builddeps-coreboot: ne compile pas libpayload (non nécessaire. Cela a été laissé par erreur, lors de l'essai de la charge utile TINT).
-   Remplace la majorité des fichiers de diff (patchs) avec des branches gerrit (choisies sur le volet).
-   Documentation: x60\_security.html et t60\_security.html: a ajouté des liens pour informer à propos du contrôleur Ethernet (Intel 82573).
-   Documentation: x60\_security.html et t60\_security.html: a ajouté des notes à propos de DMA (accés direct à la mémoire) et du dock.
-   Documentation: configuring\_parabola.html: étapes basiques de post-installation pour Parabola GNU+Linux (utile, puisque le développement de libreboot est en train d'être bougé sur Parabola GNU+Linux au temps de l'écriture).
-   builddeps-coreboot: utiliser 'make crossgcc-i386' au lieu de 'make crossgcc'. Libreboot cible seulement l'architecture x86 au temps de l'écriture.
-   Les images ROM n'incluent plus SeaBIOS. Au lieu de ça, l'utilisateur l'ajoute après. A mis à jour la documentation et les scripts.
-   docs/images/encrypted\_parabola.html: Notes à propos de linux-libre-grsec
-   Documentation: encrypted\_parabola.html: ajoute un tutoriel pour une installation chiffrée de Parabola GNU+Linux.
-   Documentation: a ajouté plus d'information à propos des jeu de puces (chipsets) wifi.


6ième version (pre-version, bêta pour la 7ième) {#release20140911}
===================================

-   Publié le 11 Juillet 2014 (pré-version) première bêta
-   Revu (pré-version, seconde bêta) le 16 Juillet 2014
-   Revu (pré-version, troisième bêta) 20 Juillet 2014
-   Revu (pré-version, quatrième bêta) 29 Juillet 2014
-   Revu (pré-version, cinquième bêta) 11 Août 2014 (corrigé le jour-même)
-   Revu (pré-version, sixième bêta) 3 Septembre 2014
-   Revu (pré-version, septième bêta) 11 Septembre 2014:


Machines encore supportées (comparé à la dernière version):
--------------------------------------------------------

-   **Lenovo ThinkPad X60/X60s**
    -   Vous pouvez aussi enlever la carte mère d'un X61/X61s et la remplacer avec celle d'un X60/X60s.


Nouveaux systèmes supportés dans cette version: 
--------------------------------------

-   **Lenovo ThinkPad X60 Tablet** (résolution 1024x768 et 1400x1050) avec support pour le scanneur
    -   Voir **hardware/\#supported\_x60t\_list** pour la liste d'écrans LCD supportés.
    -   C'est inconnu s'il se peut qu'une X61 Tablet ait sa carte mère remplacée par celle d'une X60 Tablet.
-   **Lenovo ThinkPad T60** (carte graphique Intel) (il y a des problèmes; regardez ci-dessous)
    -   Voir les notes ci-dessous pour des exceptions, et **hardware/\#supported\_t60\_list** pour des écrans LCD connus pour marcher.
    -   C'est inconnu s'il se peut qu'un T61 ait sa carte mère remplacée par celle d'un T60.
    -   Le T60p (et variantes du T60 avec carte graphique ATI) ne sera sûrement jamais supporté:  
        **hardware/\#t60\_ati\_intel**
-   **Apple MacBook1,1** (MA255LL/A, MA254LL/A, MA472LL/A)
    -   Voir **hardware/\#macbook11**.
-   **Apple MacBook2,1** (MA699LL/A, MA701LL/A, MB061LL/A, MA700LL/A, MB063LL/A, MB062LL/A)
    -   Voir **hardware/\#macbook21**

Machines n'étant plus supportés (comparé à la version précédente):
------------------------------------------------------------

-   **Tous les systèmes précédents sont encore supportés**

Modifications pour r20140911 (7ième bêta) (11 Septembre 2014)
--------------------------------------------------------

-   Les changements ci-dessous ont été fait dans un répertoire git, contrairement aux précédentes versions. Les descriptions ci-dessous sont copiés depuis 'git log'.
-   Met à jour .gitignore pour les nouvelles dépendances.
-   Utilise un submodule pour i945-pwm.
-   Ne nettoie pas les paquetages qui échouent ou qui n'ont pas besoin de nettoyage.
-   Ne nettoie pas i945-pwn, ce n'est pas nécessaire.
-   Correction de régression: Ré-ajoute CD-ROM (ata0) dans le GRUB
-   Documentation: ajoute des notes à propos de la pénalité de performance lors de l'utilisation d'ecryptfs.
-   Documentation: a corrigé des erreurs de grammaire et d'orthographe.
-   Documentation: macbook21: ajoute un nouveau système comme testé
-   Documentation: macbook21: ajoute des infos à propos de l'amélioration de la sensitivité du pavé tactile.
-   Documentation: X60 Tablet: ajoute plus d'informations à propos de la saisie des doigts.
-   Documentation: release.html: ajoute des informations à propos des commit récemment fusionnés dans coreboot.


Modifications pour r20140903 (6ième bêta) (3 Septembre 2014)
-------------------------------------------------------

-   A ajouté des scripts builddeb\* modifiés pour Parabola GNU+Linux-libre:
    builpac, builpac-flashrom, buildpac-bucts (courtoisie de Noah Vesely)
-   Documentation: a mis à jour toutes les sections concernés pour mentionner l'utilisation des scripts dans buildpac\* pour les utilisateurs de Parabola.
-   Documentation: a ajouté des informations montrant comment activer ou désactiver le bluetooth sur le X60.
-   MacBook1,1 testé! Regardez **hardware/\#macbook11**
-   Documentation: a corrigé une faute d'ortographe dans \#get\_edid\_panelname (a changé get-edit en get-edid)
-   Documentation: a ajouté images/x60\_lcd\_change/  (seulement des images pour maintenant)
-   A ajouté gcry\_serpent et gcry\_whirlpool à la liste de module GRUB
dans le script 'build' (pour les utilisateurs de luks)
-   **Libreboot est maintenant basé sur une nouvelle version de coreboot du 23 août, 2014:\
   A fusionné les commits (concerne les cartes mères qui étaient déjà supportées dans libreboot):**
    -   <http://review.coreboot.org/#/c/6697/>
    -   <http://review.coreboot.org/#/c/6698/> (déjà fusionné)
    -   <http://review.coreboot.org/#/c/6699/> (déjà fusionné)
    -   <http://review.coreboot.org/#/c/6696/> (déjà fusionné)
    -   <http://review.coreboot.org/#/c/6695/> (déjà fusionné)
    -   **<http://review.coreboot.org/#/c/5927/> (déjà fusionné)**
    -   <http://review.coreboot.org/#/c/6717/> (déjà fusionné)
    -   <http://review.coreboot.org/#/c/6718/> (déjà fusionné)
    -   <http://review.coreboot.org/#/c/6723/> (déjà fusionné)
        (patch mode texte, pourrait peut-être activé memtest. macbook21)
    -   <http://review.coreboot.org/#/c/6732/> (FUSIONNÉ) (enlève le délai inutile dans le clavier ps/2 du macbook21. déjà fusionné)
-   Ceux qui ont été aussi fusionné dans coreboot (concerne les cartes mères que libreboot supporte déjà):
    -   <http://review.coreboot.org/#/c/5320/> (fusionné)
    -   <http://review.coreboot.org/#/c/5321/> (fusionné)
    -   <http://review.coreboot.org/#/c/5323/> (fusionné)
    -   <http://review.coreboot.org/#/c/6693/> (fusionné)
    -   <http://review.coreboot.org/#/c/6694/> (fusionné)
    -   <http://review.coreboot.org/#/c/5324/> (fusionné)

-   Documentation: a enlevé la section à propos de tft\_brightness sur l'X60 (le nouveau code le rend obsolète)
-   A enlevé tous les patchs de resources/libreboot/patch/ et a ajouté un nouveau patch: 0000\_t60\_textmode.git.diff
-   A mis à jour le script getcb et DEBLOB.
-   A mis à jour les fichiers de configuration sous resources/libreboot/config pour s'accommoder à la nouvelle version de coreboot.
-   A enlevé grub\_serial\*.cfg et libreboot\_serial\*.rom, tous les fichiers de configurations/rom sont maintenant unifiés (contenant la même configuration comme les fichiers de ROM serial d'avant)
-   A enlevé grub\_serial\*.cfg et libreboot\_serial\*.rom,  tous les fichiers de configuration/rom sont maintenant unifiées (contient la même configuration comme les fichiers de rom en série d'avant)
    -   Documentation: a mis à jour \#rom pour refléter le dessus.
-   A mis à jour GRUB à la nouvelle version du 14 Août, 2014.
-   A unifié toutes les configuration grub pour tous les systèmes dans un seul grub.cfg sous resources/grub/config
-   A mis à jour flashrom à la nouvelle version du 20 Août 2014.
-   A ajouté getseabios et builddeps-seabios (builddeps et getall ont été aussi mis à jour).
    -   A ajouté des instructions à 'buildrom-withgrub' pour inclure bios.bin.elf et vgaroms/vgabios.bin de SeaBIOS à l'intérieur de la ROM.
-   A ajouté seabios (et sgavgabios) dans grub en tant qu'option de charge utile dans le menu.
-   A désactivé la sortie série dans Memtest86+ (n'est plus nécessaire) pour accélérer les tests.
    -   MemTest86+ marche proprement maintenant, il peut s'afficher sur l'écran de l'ordinateur portable (plus besoin de port série).
-   A ajouté getgrubinvaders, et les scripts builddeps-grubinvaders. A ajouté ceux-ci à getall et builddeps.
    -   A ajouté [GRUB Invaders](https://www.coreboot.org/GRUB_invaders) dans les choix de menu dans resources/grub/config/grub.cfg
-   A ajouté des régles à builddeps-coreboot pour construire libpayload avec TinyCurses (a ajouté les instructions approprié dans le script cleandeps)
-   A décommenté des lignes dans resources/grub/config/grub.cfg pour faire charger les polices d'écritures/fonds d'écrans (n'est plus utile, maintenant que grub est en mode texte).
-   A décommenté des lignes dans buildrom-withgrub qui incluaient les polices d'écritures/fonds d'écrans (n'est plus utile, maintenant que grub est en mode texte).
-   A ajouté resources/utilities/i945-pwm/ (de git://git.mtjm.eu/i945-pwd), pour déboguer la luminosité acpi sur les systèmes i945.
    -   A ajouté des instructions pour celà dans builddeps, builddeps-i945pwm, builddeb et cleandeps
-   script de 'build': a enlevé les parties qui ont généré les manifestes sha512sum (non nécessaire, depuis que les tarballs de versions sont signés avec GPG).
-   script de 'build': a enlevé les parties qui ont généré le répertoire libreboot\_meta (n'est plus nécessaire, depuis que \_meta sera hébergé dans git)
    -   A mis à jour \#build\_meta (et autres parties de la documentation) pour s'adapter à ce changement.
-   Documentation: a simplifié (a refaçonné) les notes dans \#rom
-   script de 'build': a enlevé les parties qui ont généré libreboot\_bin et les a ajoutés dans un nouveau script: 'build-release'
    -   Documentation: \#build mis à jour pour réfléter le dessus.
-   A ajouté tous les modules gcry\_\* dans grub (luks/cryptomount):
    gcry\_arcfour gcry\_camellia gcry\_crc gcry\_dsa gcry\_md4
    gcry\_rfc2268 gcry\_rmd160 gcry\_seed gcry\_sha1 gcry\_sha512
    gcry\_twofish gcry\_blowfish gcry\_cast5 gcry\_des gcry\_idea
    gcry\_md5 gcry\_rijndael gcry\_rsa gcry\_serpent gcry\_sha256
    gcry\_tiger gcry\_whirlpool~~
-   A ajouté la liste de modules GNUtoo (incluent tous les modules gcry\_\* d'au-dessus), cryptomount devrait marcher maintenant.
-   A enlevé builddeb-bucts et builddeb-flashrom, a fusionné avec builddeb (a mis à jour en accordance).
-   A enlevé builddeb-bucts et builddeb-flashrom, a fusionné avec buildpac (a mis à jour en accordance).
-   A renommé buildpac en deps-parabola (a mis à jour en accordance).
-   Documentation: a enlevé toutes les parties à propos des dépendances de build, les a remplacées avec des liens vers \#build\_dependencies,
-   Documentation: a mis l'emphase plus fortement dans la documentation sur le fait du besoin de recompiler bucts et/ou flashrom avant de la flasher une image ROM.
-   build-release: flashrom, nvramtool, cbfstool et bucts ne sont plus fournis pré-compilé dans les archives d'éxecutables, et sont maintenant sous la forme source seulement. (pour maximiser la compatibilité avec les distributions).
-   script de 'build': a remplacé les instructions d'assemblage de grub.elf, c'est maintenant pris en charge par un utilitaire ajouté sous resources/utilities/grub-assemble.
-   A bougé resources/grub/keymap dans resources/utilities/grub-assemble/keymap, et a mis à jour cet utilitaire.
-   Documentation: a enlevé les liens inutiles d'images de dispositions de claviers et de dispositions non modifiés.
-   A enlevé toutes les polices d'écritures non utilisées du répertoire dejavu-fonts-ttf-2.34/
-   script de 'buildrom-withgrub': a mis à jour pour créer 2 sets de ROMs pour chaque système: un avec le mode texte, un avec la mémoire d'affichage de coreboot.
-   Documentation: a mis à jour \#rom en accordance avec le dessus.
-   A supprimé les fichiers inutilisés README et COPYING du répertoire principal.
-   A enlevé quelques instructions d'rm -Rf .git\* des scripts get\* et les a bougés dans le script build-release.
-   A partagé la grub.cfg par défaut en 6 partiess:
    extra/{common.cfg,txtmode.cfg,vesafb.cfg} et
    menuentries/{common.cfg,txtmode.cfg,vesafb.cfg}
    -   le script buildrom-withgrub utilise ceux-ci pour générer le grub.cfg pour chaque type de configuration.
-   grub\_memdisk.cfg (utilisé à l'intérieur de grub.elf) charge maitenant seulement grub.cfg depuis cbfs. Celà n'active plus la sortie série ou met en place des préfixes (menuentries/common.cfg le fait à la place).
-  resources/grub/config/extra/common.cfg a ajouté:
    -   instructions insmod pour charger ces modules: nativedisk, ehci, ohci, uhci, usb, usbserial\_pl2303, usbserial\_ftdi, usbserial\_usbdebug
    -   a mis à jour prefix=(memdisk)/boot/grub
    -   Pour les graphiques natifs (recommandé par le wiki coreboot):\gfxpayload=keep\terminal\_output \--append gfxterm
    -   Joue un beep au démarrage:\ play 480 440 1
-   Documentation: a mis à jour gnulinux/grub\_cbfs pour le rendre plus sécurisé (et facile) à suivre.


Corrections dans r20140811 (5ième beta) (11 Août 2014)
------------------------------------------------------
-   A corrigé un erreur où la liste des révisions pour la 5ième bêta était listée comme étant du 11 Mars 2014, alors qu'en fait du 11 Août 2014
-   A corrigé un grub.cfg incorrect qui a été actuellement plaçé dans resources/grub/config/x60/grub\_usqwerty.cfg qui a cassé l'option par défaut du menu GRUB du X60.



Corrections to r20140811 (5ième beta) (11 Août 2014)
-----------------------------------------------------
-   build: a ajouté 'luks', 'lvm', 'cmosdump' et 'cmostest' à la liste des modules pour grub.elf
-   Documentation: a ajouté des images montrat le débousillage du T60 (encore besoin d'écrire un tutoriel)
-   compilation: a inclut les fichiers cmos.layout (coreboot/src/mainboard/manufacturer/model/cmos.layout) dans libreboot\_bin
-   Documentation: a ajouté **install/x60tablet\_unbrick.html**
-   Documentation: a ajouté **install/t60\_unbrick.html**
-   Documentation: a ajouté **install/t60\_lcd\_15.html**
-   Documentation: a ajouté **install/t60\_security.html**
-   Documentation: a ajouté **install/t60\_heatsink.html**
-   Documentation: a renommé RELEASE.html en release.html
-   Documentation: a enlevé la référence à pcmcia dans x60\_security.html (c'est cardbus)
-   Documentation: a ajouté  des informations préliminaires à propos de la marque au hasard (pour la détection d'intrusion physique) dans x60\_security.html et t60\_security.html
-   Documentation: a ajouté  des informations préliminaires à propos de la marque au hasard (pour la détection d'intrusion physique) dans x60\_security.html et t60\_security.html
-   Documentation: a ajouté des informations préliminaires à propos de prévenir/mitiger les attaques de démarrage à froid dans x60\_security.html et t60\_security.html
-   Documentation: a ajouté des informations dans \#macbook21 avertissant à propos de problèmes avec macbook21
-   Documentation: X60/T60: a ajouté des informations à propos de la vérification des ROMs personnalisés en utilisant dd, pour savoir si oui ou non la région  de 64Ko est dupliqué en dessous de top ou non. Conseille la prudence à propos de cela dans le tutoriel qui traite avec le flashage sur le BIOS Lenovo, citant les commandes dd nécessaires si il est confirmé que la ROM n'a pas encore été appliquée avec dd. (dans le cas que l'utilisateur a compilé leur propre ROMs depuis libreboot, en utilisant pas les scripts de build, ou si ils ont oublié d'utiliser dd, etc).
-   A partagé resources/libreboot/patch/gitdiff dans des fichiers de patch séparés (le script getcb a été mis à jour pour s'accomoder à ce changement).
-   A ré-ajouté les fichiers .git à bucts
-   A corrigé l'oubli où le fichier macbook21\_firstflash n'était pas inclut dans les archives d'éxecutables
-   Les archives de versions sont maintenant compressées utilisant .tar.xz pour une meilleure compression.


Modifications pour r20140729 (4ième beta) (29 Juillet 2014)
---------------------------------------------------

-   Documentation: a été amélioré (plus d'explications, informations contextuelles) dans docs/security/x60\_security.html (courtoisie de Denis Carikli)
-   Le MacBook2,1 a été testé (et confirmé)
-   macbook21: a ajouté le script 'macbook21\_firstflash' pour le flashage de libreboot pendant que le micrologiciel EFI d'Apple est en marche.
-   Documentation: macbook21: a ajouté des instructions pour le flashage basé logiciel de libreboot pendant que le micrologiciel EFI d'Apple est en marche.
-   A réduit la taille du fichier libreboot\_src.tar.gz:
    -   A enlevé .git et .gitignore du répertoire grub (libreboot\_src); pas nécessaire. Les enlever réduit la taille de l'archive (par beaucoup). Le développement de GRUB devrait se faire en amont.
    -   A enlévé .git et .gitignore du répertoire bucts (libreboot\_src); pas nécessaire. Les enlever réduit la taille de l'archive. Le développement de bucts devrait se faire en amont.
    -   A enlevé .svn du répertoire flashrom (libreboot\_src); pas nécessaire. L'enlever réduit la taille de l'archive. Le développement de flashrom devrait se faire en amont.
-   A ajouté des ROMs avec une dispostion Qwerty (Italienne) dans GRUB (libreboot\*itqwerty.rom)
-   A ajouté le script ressources/utilities/i945gpu/intel-regs.py pour des problèmes de débogguage en lien avec la compatibilité d'écrans LCD sur l'X60 Tablet et le T60. (courtoisie de [Michał Masłowski](http://mtjm.eu))


Modifications pour r20140720 (3ième beta) (20 Juillet 2014)
---------------------------------------------------

-   A corrigé la faute d'ortographe qui a existé dans la seconde bêta où la date de publication de cette dernière était listée comme étant en 2016, alors qu'en fait c'était 2014.
-   Documentation: a ajouté des détails (préliminaires) à propos de (rares) processeurs bogués sur le ThinkPad T60 qui ont été trouvé comme en échec (instabilité, panique de kernels, etc) sans les mises à jour du microcode.
-   Documentation: a ajouté docs/hardware/x60\_heatsink.html pour montrer comment changer le dissipateur thermique sur le ThinkPad X60
-   A ajouté des images ROMs pour la disposition clavier Azerty (Français) dans GRUB (courtoisie d'Olivier Mondoloni).
-   A réarrangé quelques scripts:
    -   ~~Re-factorisé ces scripts (les a rendus plus facile à lire/maintenir):
        build-x60, build-x60t, build-t60, build-macbook21~~
    -   ~~A réduit le nombre de configurations grub à 2 (ou 1, pour le macbook21), les scripts de compilation génère maintenant les autres configurations au temps de la compilation.~~
    -   A supprimé build-x60, build-x60t, build-t60, build-macbook21 et l'a remplacé avec un script (générique) buildrom-withgrub intelligent
    -   A mis à jour la compilation pour utiliser le script buildrom-withgrub pour la compilation des images ROM.
    -   coreboot.rom et coreboot\_serial.rom ont été renommé en coreboot\_usqwerty.rom et coreboot\_serial\_usqwerty.rom
    -   coreboot\_dvorak et coreboot\_serial\_dvorak.rom ont été renommé en coreboot\_usdvorak.rom et coreboot\_serial\_usdvrak.rom
    -   A renommé coreboot\*rom en libreboot\*rom
    -   A rendu les scripts flash, lenovobios\_firtflash et lenovobios\_secondflash s'arrêter et échouer si le fichier spécifié n'existe pas.
    -   A mis à jour toutes les parties pertinentes de la documentation pour réfléchir le dessus.
    -   A remplaçé le background.png avec background.jpg. A ajouté gnulove.jpg. (resources/grub/background)
-   A mis à jour buildrom-withgrub pour utiliser background.jpg au lieu de background.png
-   A mis à jour buildrom-withgrub pour utiliser gnulove.jpg aussi
-   A mis à jour les fichiers ressources/grub/config/macbook21/grub\*cfg pour utiliser le fond d'écran gnulove.jpg.
-   A mis à jours les fichiers resources/grub/config/{x60,t60,x60t}/grub\*cfg pour utiliser le fond d'écran background.jpg
-   Documentation: a mis à jour docs/\#grub\_custom\_keyboard pour être généralement plus utile.
-   nvramtool:
    -   A mis à jour le script builddeps-coreboot pour le compiler
    -   A mis à jour le script de compilation pour l'inclure dans libreboot\_bin
-   Documentation: a ajouté docs/security/x60\_security.html ( durcissement de la sécurité pour le X60)

Modifications pour r20140716 (Seconde beta) (16 Juillet 2014)
--------------------------------------------------

-   A supprimé tous les fichiers liés à git dans le répertoire coreboot. C'était nécessaire parce qu'avec ceux-ci il est possible d'exécuter 'git diff' qui montre les changements fait sous la forme d'un patch (sous format diff); cela inclut les blobs qui ont été supprimé pendant le déblobbage.


Modifications pour r20140711 (1ère beta) (11 Juillet 2014)
---------------------------------------------------

-   Publication initiale (nouvelle base coreboot, datée du 1 Juin 2014. Voir le script 'getcb' pour référence)
-   coreboot DÉBLOBBÉ
-   A enlevé la partie de memtest86+ 'make' où ça essayait de se connecter à un serveur scp pendant la compilation ( a décommenté la ligne 24 dans la Makefile)
-   Le X60 utilise maintenant un seul .config (pour coreboot)
-   Le X60 utilise maintenant un seul grub.cfg (pour le disque mémoire grub)
-   Le X60 utilise maitenant un seul grub.elf (en tant que charge utile)
-   A ajouté un nouveau code graphique pour X60 (remplace le vieux code 'replay') de Vladimir Serbinenko: 5320/9 de review.coreboot.org
-   Le T60 est maintenant supporté, avec des graphiques natifs (5345/4 de review.coreboot.org, pris sur le volet au dessus du checkout 5320/9)
-   A ajouté le support pour le macbook2,1 ( fait par Mono Moosbart et Vladimir Serbinenko) depuis review.coreboot.org (voir le script 'getcb' pour savoir comment cela a été fait)
   -   Documentation: a ajouté des informations amenant sur la bonne page et parlant à propos des modèles qui sont supportés
   -   A ajouté le fichier resources/libreboot/config/macbook21config
   -   macbook21: a ajouté le script 'build-macbook21' et a fait un lien vers lui dans 'build' (les ROMs incluent sous bin/macbook21/)
   -   macbook21: a enlevé les instructions dd du script build-macbook21 (le macbook21 n'a pas besoin de bucts lors du flashage du libreboot pendant que le micrologiciel EFI d'Apple est en marche ).
   -   Documentation: a ajouté les ROMs macbook21 à la liste des ROMs dans docs/\#rom
   -   Documentation: a écrit la documentation faisant lien vers la page de Mono Moosbart à propos du macbook21 et Parabola (et inclut une copie)
-   Documentation: a ajouté une copie du guide d'installation de Parabola de Mono (pour le macbook21 avec le micrologiciel EFI d'Apple) a mis un lien vers dans la page principale.
-   Documentation: a ajouté une copie de la page coreboot de Mono (pour macbook21) et a mis un lien vers dans la page principale.
-   T60: copie les options CD des fichiers grub.cfg pour les images \*serial\*.rom du T60 dans les configurations grub pour les images non-serial. (les ordinateurs portables ont un disque CD/DVD sur l'ordinateur portable principal)
-   macbook21: enlève les options dans build-macbook21 pour \*serial\.rom (il n'y pas de dock ou port série disponible pour macbook21)
-   A ajouté des patchs pour les contrôles du rétroéclairage sur le X60 et T60 avec l'aide de Denis Cariskli (voir ./resources/libreboot/patch/gitdiff et ./getcb et docs/i945\_backlight.md)
    -   Documentation: a ajouté docs/i945\_backlight.md montrant comment les contrôles  du rétroclairage ont été concus pour marcher sur le X60/T60.
-   Documentation: a ajouté des informations à propos de la récupération du nom de l'écran LCD basé sur les données EDID.
    -   Documentation: a ajouté un lien vers cela depuis la liste des ordinateurs portables T60 supportés et des écrans supportés pour les ordinateurs portables T60 (comme ça l'utilisateur peut vérifier quel écran LCD ils ont).
-   X60/T60: A fusionné les patchs pour la correction de la 3D (de Paul Menzel) lors de l'utilisation du kernel 3.12 ou plus haut (voir ./resources/libreboot/patch/git diff et ./getcb)
    -   basé sur 5927/11 et 5932/5 de review.coreboot.org
-   A amélioré le support de thinkpad\_acpi (de coreboot) : les xsensors montrent plus d'informations.
    -   pris de 4650/29 dans review.coreboot.org (fusionné dans le 'master' de coreboot le 1er Juin 2014)
-   A fusionné les changements pour le numériseur (X60 Tablet et IR (X60 et T60) basé sur 5243/17, 5242/17 et 5239/19 de review.coreboot.org
    -   (voir ./resources/libreboot/patch/gitdiff et ./getcb) 
-   Documentation: a ajouté des informations à propos de la compilation de flashrom utilisant le script 'builddeps-flashrom'.
-   A re-créé resources/libreboot/config/x60config
-   A re-créé resources/libreboot/config/t60config
-   A ajouté 'x60tconfig' dans resources/libreboot/config (parce que la X60 Tablet a des informations différentes à propos de  serial/model/version dans 'dmidecode')
    -   A ajouté le script 'build-x60t'
    -   A mis à jour le script 'build' pour utiliser 'build-x60t'
    -   Documentation: a ajouté à la section \#config la section \#config\_x60t (la configuration de libreboot et l'information de dmidecode)
    -   Documentation: a ajouté les ROMs x60t à la liste des ROMs.
-   A arrangé le script 'builddeps' (plus facile à lire)
-   A arrangé le script 'cleandeps' (plus facile à lire)
-   A annoté le script 'buildall'
-   A ajouté le script 'getcb' pour récupérer la modification depuis coreboot utilisée depuis git, et la patcher.
-   A ajouté le script 'getgrub' pour récupérer la modification de GRUB utilisée depuis git, et la patcher.
-   A ajouté le script 'getmt86' pour récupérer la version de memtest86+ utilisée, et la patcher.
-   A ajouté le script 'getbucts' pour récupérer la version de bucts utilisée.
-   A ajouté le script 'getflashrom' pour récupérer la version de flashrom utilisé, et la patcher.
-   A ajouté le script 'getall' qui exécute tout les autres scripts 'get'.
-   Ajoute des instructions dans le script 'build' pour préparer libreboot meta.tar.gz
    -   Nouvelle archive: libreboot\_meta.tar.gz - archive minimale, utilisant les scripts 'get' pour télécharger toutes les dépendances (coreboot, memtest, grub et ainsi de suite).
-   Documentation: a ajouté des informations à propos d'où le script 'build' prépare l'archive libreboot\_meta.tar.gz. 
-   Documentation: a ajouté des informations à propos de la façon d'utiliser les scripts 'get' dans libreboot\_meta.tar.gz (pour générer libreboot\_src.tar.gz)
    -   Documentation: mention que la meta ne créé pas le répertoire libreboot\_src/, mais que libreboot\_meta lui-même devient le même.
    -   Documentation: conseille de renommer libreboot\_meta en libreboot\_src après avoir exécuté 'getall'.
-   A annoté le script 'builddeb', pour dire qu'est ce que chaque lots de dépendances sont pour.
-   A divisé les sections bucts/flashrom builddeb en des scripts séparés
-   A annoté le script 'builddep', pour dire ce que chaque set de dépendances sont faites pour.
-   A séparé les sections builddeb bucts/flashrom en des scripts séparés: builddeb-flashrom, builddeb-bucts.
-   Documentation: A mis à jour des parties pertinentes en relation avec les information ci-dessus. 
-   A ajouté des instructions au script 'buil pour inclure builddeb-bucts et builddeb-flashrom dans libreboot\_bin
-   A mis à jour la révision de flashrom (r1822 2014-06-16) depuis SVN (http://flashrom.org/Download).
    -   A mis à les instructions dans docs/ pour les nouvelles commandes necéssaires (Puce Macronix sur le X60/T60)
    -   Pour X60/T60 (flahrom): a patchéles exécutables
        flashchips.c\_lenovobios\_macronix et
	flashchips.c\_lenovobios\_sst pour SST/macronix
	(inlus dans resources/flashrom/patch)
    -   A mis à jour builddeps pour construire flashrom__lenovobios\_sst et flashrom\_lenovobio\_macronix, pour les utilisateurs du X60/T60 avec le BIOS Lenovo
    -   A bougé les instructions de compilations flashrom de 'builddeps' et les a mis dans 'builddeps-flashrim', a executé celà depuis 'builddeps'.
    - A ajouté builddeps-flashrom dans libreboot\_bin.tar.gz
-   flashrom: a ajouté le flashchips.c patché dans resources/flashrom/patch (utilise automatiquement la puce macronix correcte sur libreboot, sans utilisé le paramètre -c)
    -   A enlevé les entrées 'MX25L1605' et 'MX25L1605A/MX25L1606E' dans flashchips.c pour la verion patchée de flashchips.c
    -   A ajouté des instructions pour 'builddeps-flashrom' pour automatiquement utiliser ce flashchips.c modifié dans la compilation par défaut.
-   A ajouté builddeb pour libreboot\_bin.tar.gz
-   A bougé les instructions de la compilation de 'bucts' depuis builddeps dans builddep-bucts.
    -   builddeps exécute maintenant 'builddeps-bucts' au lieu de celà.
    -   A ajouté 'builddeps-bucts' dans libreboot\_bin.tar.gz
    -   Documentation: a ajouté des informations à propos de l'utilisation de 'builddep-bucts' pour construire l'utilitaire BUC.TS
-   A ajouté les scripts 'lenovobios\_firstflash' et 'lenovobios\_secondflash'
    -   A ajouté des instructions dans le script 'build' pour inclure ces fichiers dans libreboot\_bin
    -   Documentation: ajout d'un tutoriel pour flashé pendant que le BIOS Lenovo est en marche (sur le X60/T60).

    -   Added instructions to 'build' script for including those files
        in libreboot\_bin
    -   Documentation: Add tutorial for flashing while Lenovo BIOS is
        running (on X60/T60)
-   A ajouté le script 'flash' (assurez-vous d'exécuter en premier builddeps-flashrom) qui (pendant que libreboot est déjà exécutéé) peut utiliser flahrom pour flasher une ROM
    -   ex: "sudo ./flash bin/x60/coreboot\_serial\_ukdvorak.rom" est équivalent à "sudo ./flashrom/flashrom -p internal -w bin/x60/coreboot\_uk\_dvorak.rom"
    -   A mis à jour le script 'build' pour inclure le script 'flash' dans libreboot\_bin.tar.gz
-   Documentation: a remplacé le tutoriel par défaut flashrom pour recommander le script 'flash' à la place.
-   Ré-ajout du code source de cbstool dans libreboot\_bin.tar.gz, en tant que cbfstool\_standalone
    -   A patché cette version pour marché (capable d'être compilé et utilisée) sans avoir besoin de la totalité du code source de coreboot.
    -   A créé une version patché des fichiers sources concernés et les a ajoutés dans resources/cbfstool/patch
        -   regardez coreboot/util/cbfstoll/rmodule.c et ensuite la version patchée dans resources/cbfstool/patch/rmodule.c
        -   regardez coreboot/src/include/rmodule-defs.h et la régle dans 'build' pour inclure ceci dans ../libreboot\_bin/cbfstool\_standalone
    -   A ajouté des instructions dans le script 'build' pour appliquer ce patch sur la source cbfstool\_standalone dans libreboot\_bin
    -   A ajouté des instructions dans le script 'build' pour ensuite re-compiler cbfstool\_standalone dans libreboot\_bin après avoir appliqué le patch.
    -   A ajouté un script 'builddeps-cbfstool' (dans src, mais seulement utilisé dans bin et mis dans bin par 'build') qui compile cbfstool\_standalone dans libreboot\_bin (make), déplace les éxecutables cbfstool et rmodtool dans libreboot\_bin/ et ensuite fait 'make clean' dans libreboot\_bin/cbfstool\_standalone
    -   A mis à jour le script 'build' pour mettre 'builddep-cbfstool' dans libreboot\_bin
    -   A mis à jour le script 'build' dans cbfstool (standalone) pour s'accomoder au dessus.
    -   Documentation: a ajouté des notes à propos de cbfstool (standalone) dans libreboot\_bin
-   Documentation: a rendu docs/gnulinux/grub\_cbfs.html légèrement plus facile à suivre.
-   A annoté les scripts 'build\*' avec des commandes 'echo', pour aider l'utilisateur à comprendre qu'est ce qui en train de se passer pendant le processus de compilation.
-   Documentation: a ajouté des informations à propos de comment les données de 'dmidecode' ont été mise dans les configs de coreboot
    -   Documentation: En fait, documente comment les fichiers 'config' dans resources/libreboot/config/ ont été créé.
-   Documentation: A ajouté des informations à propos de quels ordinateurs portables ThinkPad T60 sont supportés, et ceux qui ne le sont pas.
-   Documentation: a ajouté des informations à propos des onduleurs LCD (pour une mise à niveau de l'écran LCD sur un T60 14.1' XGA ou 15.1' XGA)
    -   c'est FRU P/N 41W1478 (sur le T60 14.1") donc ça a été ajouté dans les documentations.
    -   c'est P/N 42T0078 FRU 42T0079 ou P/N 41W1338 (sur T60 15.1") donc ça a été dans la documentations.
-   Documentation: a ajouté des informations à propos des noms des écrans LCD pour le T60 dans les parties concernés de la documentation.
-   Documentation: a ajouté des informations (avec des images) à propos des différences entre le T60 avec une carte graphique Intel et le T60 avec carte graphique ATI.
-   Documentation: a ajouté des images de dispositions de claviers (US/UK Qwerty/Dvorak) à la liste des ROM, pour laisser l'utilisateur comparer avec son propre clavier.
-   A déplacé les instructions de compilation de coreboot de 'builddeps' dans 'builddeps-coreboot' et met un lien dans 'builddeps'
    -   Attache à 'builddeps-coreboot' dans le stage final de 'getcb' 'builddeps-grub', relié depuis 'builddeps'
    -   Attache à 'builddeps-grub' dans le stage final de 'getgrub'
-   Déplace les instructions de compilation de MemTest86+ de 'builddeps' dans 'builddeps-memtest86', relié depuis 'builddeps'
    -   Attache à 'builddeps-memtest86' dans le stage final de 'getmt86'
-   a rendu le script 'build' mettre le répertoire resources/ dans libreboot\_bin, pour faire marcher builddeps-flashrom dans libreboot\_bin
-   A enlevé les instructions pour compiler le code source dans le script 'get' (elles n'ont pas vraiment quelque chose à faire là-bas)
-   A ajouté libfuse-dev et liblzma-dev à la liste des dépendances de GRUB dans le script 'builddeb'.
-   A converti le fichier 'RELEASE' en 'docs/RELEASE.html'
-   A ajouté ces dépendaces au script builddeb (pour la partie GRUB): gawk libdevmapper-dev libtool libfreetype6-dev
-   A ajouté des instructions à la fin du script de compilation pour créer sha512sum.txt avec un fichier de manifeste plus des sommes de controle.
-   A supprimé les fichiers RELEASE et BACKPORT (ne sont plus nécessaires)
-   Documentation: a ajouté des informations à propos du dock 
X60/T60 (ultrabase x6 et minidock avancé) dans les sections concernées.
    -   Ajouté dans docs/\#serial

Version 20140622 (5ième version) {#release20140622}
==============================

-   7 Mars 2014
-   a revu la version du 22 Juin 2014

Officiellement supportés
--------------------

-   ThinkPad X60
-   ThinkPad X60s

Révision (22 Juin 2014 - extra)
---------------------------------

-   Documentation: a ajouté le tutoriel de débousillage du X60-   Documentation: a ajouté des infos à propos de l'activation ou la désactivation du wifi
-   Documentation: a ajouté des infos à propos de l'activation ou la désactivation du dispositif de pointage.


Révision (22 Juin 2014 - extra)
---------------------------------

-   Documentation: A amélioré les instructions pour utiliser flashrom
-   Documentation: A amélioré les instructions pour utiliser cbfstool (pour changer le menu GRUB par défaut)
-   Documentation: Nombreuses petites corrections.

Notes de révisions (22 Juin 2014)
-------------------------------

-   A mis à jour GRUB (git 4b8b9135f1676924a8458da528d264bbc7bbb301, 20 Avril 2014)
-   A fait de "DeJavu Sans Mono" la police d'écriture par défaut dans GRUB (corrige la corruption des bordures).
-   A ré-ajouté un fond d'écran dans GRUB (GNU en train de méditer)
-   A ajouté 6 images de plus:
    -   coreboot\_ukqwerty.rom (disposition de clavier UK Qwerty dans GRUB)
    -   coreboot\_serial\_ukqwerty.rom (disposition de clavier
    UK Qwerty dans GRUB)
    -   coreboot\_dvorak.rom (disposition de clavier US Dvorak dans GRUB)
    -   coreboot\_ukdvorak.rom (disposition de clavier US Dvorak dans GRUB)
    -   coreboot\_serial\_dvorak.rom (disposition de clavier US Dvorak dans GRUB)
    -   (coreboot.rom et coreboot\_serial.rom ont une disposition de clavier US Qwerty dans GRUB, comme d'habitude)
-   A amélioré la documentation:
    -   a supprimé FLASH\_INSTRUCTION et README.powertop et les a fusionné avec README.
    -   a enlevé les informations obsolètes du README et l'a nettoyé.
    -   a supprimé le README (remplacé avec docs/)
-   a nettoyé les entrées du menu dans GRUB
-   a nettoyé le répertoire racine de X60\_source/, a trié plus de fichiers dans des sous-répertoires
-   a amélioré le commentaire à l'intérieur du script 'build' (devrait rendre sa modification plus facile)
-   A renommé X60\_binary.tar.gz et X60\_source.tar.gz en libreboot\_bin.tar.gz et libreboot\_src.tar.gz, respectivement
-   A remplacé "GNU GRUB version" avec "FREE AS IN FREEDOM" sur l'écran de démarrage GNU GRUB.
-   A ajouté les fichiers sha512.txt dans libreboot\_src et libreboot\_bin. (à l'intérieur des archives)
-   A ajouté les fichiers libreboot\_bin.tar.gz.sha512.txt et libreboot\_src.tar.gz.sha512.txt (en dehors des archives)


Notes de révisions (11 Juin 2014):
--------------------------------

-   a enlevé l'option de démarrage 'CD' de coreboot.rom (non nécessaire)
-   a enlevé les options 'processor.max\_cstate=2' et 'idle=halt' (voir le fichier README.powertop)

Notes de révisions (5 Juin 2014):
-------------------------------

-   a ajouté le support du rétroéclairage (Fn+Home et Fn+End) sur le X60
-   a corrigé la 3D cassé/instable quand vous utilisez le kernel 3.12 ou plus haut
-   (voir le fichier 'BACKPORT')

Notes de révision (9 Mars 2015):
--------------------------------

-   a recrée la configuration de coreboot depuis rien
-   GRUB charge encore plus vite maintenant (moins de deux secondes).
-   Le temps de démarrage a été réduit de 5 secondes de plus.
-   A ajouté les modules crypto et cryptodisk dans GRUB
-   cbfstool est maintenant inclut dans les archives de binaire.

Notes de développement
-----------------

-   L'archive de binarie a maintenant 2 images:
    -   Avec la sortie en série activée et memtest86+ inclut (niveau de déboguage 8 dans coreboot)
    -   Avec la sortie en série désactive et memtest86+ exclut (temps de démarrage réduits) (déboguage désactivé)
-   A réduit l'impact sur la vie de la batterie:
    -   'processor.max\_cstate=2' au lieu de 'idle=halt' pour démarrer le kernel par défaut.
-   coreboot.rom (vitesses de démarrage plus rapides, déboguage désactivé):
    -   A désactivé la sortie en série de coreboot (Console-> dans "make menuconfig")
    -   A mis le niveau de déboguage de coreboot à 0 au lieu de 8 (Console-> dans "make menuconfig")
    -   A changé le délai d'inactivé à 1 seconde au lieu de 2 (dans grub.cfg)
    -   A enlevé le fond d'écran dans GRUB.
    -   A enlevé la charge utile memtest86+ (car celà dépend de la sortie série)
-   coreboot\_serial.rom (vitesses de démarrage plus lentes, déboguage activé):
    -   Temps de démarrage encore réduit, mais seulement par \~2 secondes
    -   a la charge utile memtest86+ incluse dans la ROM
    -   a le port série activé. Comment celà est achevé (depuis X60\_source): mettez le niveau de déboguage à 8, et activez la sortie série
-   (dans Console-> dans coreboot "make menuconfig")
-   (et compile avec grub\_serial.cfg et grub\_memdisk\_serial.cfg)


Version 20140221 (4ième version) {#release20140221}
==============================

-   21 Février 2014

Officiellement supportés
--------------------

-   ThinkPad X60
-   ThinkPad X60s

Notes de développement
-----------------

-   A enlevé SeaBIOS (redondant)
-   Nouvelle version GRUB (2.02\~beta2)a
    -   Corrige quelques problèmes USB
    -   Inclut l'analyseur syntaxique ISOLINUX/SYSLINUX
-   Nouveau grub.cfg
-   A enlevé les options inutiles:
    -   options pour démarrer sda 2/3/4
    -   option de démarrage seabios
-   A ajouté de nouvelles entrées de menu:
    -   Analyse syntaxique ISOLINUX (USB)
    -   Analyse syntaxique de la configuration ISOLINUX (CD)
    -   A ajouté le module 'cat' pour l'utiliser sur la ligne de commande GRUB.
-   "set pager=1" est configuré dans grub.cfg, pour une fonctionnalité comme less

Les options "Parse" lisent ./isolinux/isolinux.cfg sur un CD ou une USB, et le convertit automatiquement en une config grub et change sur le menu de démarrage de cette distribution. Celà rend le démarrage des ISOs \*beaucoup\* plus facile qu'avant.

r20131214 (3ième version) {#release20131214}
=======================

-   14 Décembre 2013

Supportés:
----------

-   ThinkPad X60
-   ThinkPad X60s

Notes de développement
-----------------

-   A ajouté la charge utile SeaBIOS à GRUB2 (pour démarrer les clés USBs)
-   nouveau grub.cfg

r20131213 (deuxième version) {#releae20131213}
=======================

-   13 Décembre 2013

Supportés:
----------

-   ThinkPad X60
-   ThinkPad X60s

Notes de développement
-----------------

-   a ajouté le fond d'écran dans GRUB2
-   a ajouté la charge utile memtest86+
-   améliorations de la documentation
-   nouveau grub.cfg

r20131212 (première version) {#release20131212}
=======================

-   12 Décembre 2013

Supportés:
----------

-   ThinkPad X60
-   ThinkPad X60s

Notes de développement
-----------------

-   version initiale
-   code source déblobbé

Copyright © 2014, 2015, 2016 Leah Rowe <info@minifree.org>\

La permission est accordé pour copier, distribuer et/ou modifier ce document sous les termes de la GNU Free Documentation Licence version 1.3 ou n'importe quelles autre version plus récentes publié par la Free Software Foundation avec aucune Sections Invariantes, Texte de Couverture, et Texte de quatrième de couverture.
Une copie de cette licence est trouvé dans [fdl-1.3.md](fdl-1.3.md)
