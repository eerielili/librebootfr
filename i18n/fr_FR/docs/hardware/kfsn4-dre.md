---
title: Carte mère ASUS KFSN4-DRE pour serveurs/stations de travails
...

C'est une serveur carte mère utilisant du matériel AMD (Fam10h). Ça peut aussi
être utilisé pour construire une station de travail à haute perfomances.
Fonctionnant sous libreboot.

Les instructions de flashage peuvent être trouvées dans
[../install/#flashrom](../install/#flashrom)

Flashing instructions can be found at
[../install/\#flashrom](../install/#flashrom)

Format de carte mère {#formfactor}
===========

Ces cartes mères utilisent le format SSI EEB 3.61; soyez sûr que votre boitier
supporte ceci. Ce format est similaire au E-ATX par le fait que la taille est
identique, mais la position des vis sont différentes.

Puces de flashage {#flashchips}
===========

Ces cartes mères utilisent le flashage LPC (pas par SPI) dans un réceptacle
PLCC. La taille par défaut est d'1Mo (8Mbits), et peut être améliorée jusqu'à
2Mo (16Mbits).
La puce SST49LF080A est celle que la carte mère utilise par défaut.
La SST49LF016C est un exemple d'un puce 2Mo (16Mbits), qui pourrait marcher.
Il est cru que 2Mo (16Mbits) est la taille maximum disponible pour la puce
flash.

*N'ENLEVEZ PAS LA PUCE AVEC VOS MAINS NUES. Utilise un extracteur de puce
PLCC. Ceux-ci peuvent s'acheter en ligne. Jetez un coup d'oeil à
<http://www.coreboot.org/Developer_Manual/Tools#Chip_removal_tools>*

Initialisation native des graphiques {#graphics}
==============================

L'initialisation native des graphiques existe (XGI Z9s) pour cette carte mère.
Le tampon d'image- et le mode texte aussi- marchent tout deux. Un port série
est aussi disponible.

Native graphics initialization exists (XGI Z9s) for this board.
Framebuffer- and text-mode both work. A serial port is also available.

Mémoire:
======

DDR2 533/667 Registered ECC. 16 emplacements. Mémoire maximale pouvant monter
jusqu'à 64GiB.


Processeurs 10 coeurs {#hexcore}
=============

La révision PCB version 1.05G (le numéro est imprimé de façon visible sur la carte mère) de cette carte mère est la dernière et la
meilleure, si vous voulez utiliser deux processeurs 10 coeurs (Opteron série
2400/8400), même si seulement deux configurations de carte mères sont connus
pour les faire marcher. Les autres révisions ne peuvent apparement support que
seulement deux processeurs à quatre coeurs.

Pour être sûr que votre carte mère supporte une carte mère, allez vérifier sur
le site web officiel d'ASUS ici:

<https://www.asus.com/support/cpu_support>. Note: pas tout les processeurs
sont dans la liste.

S'il se trouve que vous exécutiez un processeur 10 coeur sur n'importe quelle
version de la carte mère, merci de nous contacter.

Configurations de la carte mère {#configurations}
==============

Il y a 7 configurations différentes sur cette carte mère: "standard", 2S,
iKVM, iKVM/IST, SAS, SAS/iKVM et SAS/iKVM/IST.

Les cartes mères 2S ont deux emplacements PCI-E avec leur nombre de lignes
partagées, faisant que chaque emplacement a 8 lignes.

Les cartes mères iKVM sont nommées ainsi parce qu'elles offrent un accés en
temps réel à la machine à travers une carte PCI enlevable, leur matériel reste
le même que les cartes mères non configurées iKVM.

Les versions SAS ont un controlleur SAS à 4 ports et quatre connecteur SAS
7-pin au lieu de l'emplacement PCI-E x8 qui est présent sur toutes les autres
configurations de la carte mère.
Notez: la fonctionnalité SAS n'est **pas supportée** par libreboot.

Les versions IST avec la révision PCG version 1.05G sont celles qui sont crues
pouvant supporter les processeurs à 6 coeurs Opteron Istanbul (série 2400 et
8400).

Problèmes courants {#issues}
==============

-   Il semble y avoir un délai de 30 seconde bloquant le démarrage (observé
    par tpearson); à part ça le système démarre et marche comme convenu. Jetez
    un coup d'oeil à [text/kfsn4-dre.bootlog.txt](text/ksfn4-dre/bootlog.txt)-
    ça utilise le verouillage du démarrage 'simple', pendant que tpearson
    utilise le 'normal', et il suspecte que le mode 'normal' est une possible
    cause du délai. Cette personne dit qu'elle se penchera sur le cas.
    [Cette
    config](http://review.coreboot.org/gitweb?p=board-status.git;a=blob;f=asus/kfsn4-dre/4.0-10101-g039edeb/2015-06-27T03:59:16Z/config.txt;h=4742905c185a93fbda8eb14322dd82c70641aef0;hb=055f5df4e000a97453dfad6c91c2d06ea22b8545)
    n'a pas le problème.

-   Le mode texte est 'sautillant' et n'est peut-être pas utilisable, il est
    donc reommandé de flasher le BIOS avec une image coreboot avec tampon
    d'image intégré (kfsn4-dre-vesafb.rom).
    Les sautillements disparraissent si vous utilisez KMS (Kernel Mode
    Setting) une fois que le kernel démarre, mais ça reste si vous utilisez le
    kernel en mode texte.
-   Démarrer depuis une clé USB n'est pas possible; ni GNU ou SeaBIOS détecte
    les clefs/disques USB quand attachés. Les claviers USB fonctionnent sous
    GRUB et SeaBIOS, malgré des lenteurs sous GRUB (quelques secondes de
    décalages par charactères tapés)?
-   Pour installer un système d'exploitation vous allez avoir besoin d'un
    disque dur avec un système d'exploitation pré-installé sinon vous allez
    avoir besoin de brancher un autre disque interne ou un lecteur CD/DVD afin
    de démarrer une copie de l'installeur du système d'exploitation, puisque
    ça ne marche pas par USB.


Autres informations
=================

[Caractéristiques techniques](https://web.archive.org/web/20181212180051/http://ftp.tekwind.co.jp/pub/asustw/mb/Socket%20F/KFSN4-DRE/Manual/e3335_kfsn4-dre.pdf)

Copyright © 2015 Leah Rowe <info@minifree.org>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
