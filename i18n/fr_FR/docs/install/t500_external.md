---
title: Flasher le T500 avec un BeagleBone Black
...

Instructions initiales de flashage pour le T500.

Ce guide est pour ceux voulant Libreboot sur leur ThinkPad T500 pendant qu'ils
ont toujours le BIOS Lenovo présent. Ce guide peut être aussi suivi (adapté)
au cas où vous bousillez votre T500, afin de savoir comment se remettre sur
pâte.

Le W500 est en grande partie compatible avec ce guide.

Libreboot T400 {#t400}
==============

Vous serez peut-être intéressé par le [Libreboot T400](t400_external.md), plus
petit et plus portable.

Port série {#serial_port}
-----------

Le déboguage EHCI n'est peut-être pas nécessaire. Il a été rapporté que la
station d'appareillage (dock) pour cet ordinateur portable a un port série,
donc il serait peut-être possible d'utiliser ça à la place.

Une note à propos des processeurs
=================

Le [ThinkWiki](http://www.thinkwiki.org/wiki/Category:T500) a une liste de 
processeurs pour cette machine. Les Core 2 Duo P8400, P8600 et 8700 marcherais
à priori dans Libreboot. Le T9600 a d'ailleurs été testé sur le T400 et est
confirmé fonctionnel.

Le T9550 et le T9900 a été testé par un utilisateur et est compatible, nous
a-t-on rapporté sur le canal IRC.
Le T9500 et T9400 pourrait aussi marcher, mais vos expériences peuvent
différer.

Processeurs 4 coeurs (Quad-Core)
--------------

Il est très semblable qu'ils soient compatibles, mais ça nécessite une
modification matérielle.
Basé sur l'information d'un message sur la liste de diffusion coreboot,
parlant d'un billet sur un forum allemand à propos de
l'installation d'un processeur Core Quad sur un T500.
C'est actuellement un travail en cours et aucun guide n'est disponible.

Le Q9100 est compatible et confirmé fonctionnel (après modif matérielle),
comme ont rapporté les utilisateurs sur le canal IRC

Q9100 is compatible and confirmed working (after hw mod), as reported by users in the IRC
channel

- [Billet sur la liste de diffusion de Coreboot](https://mail.coreboot.org/pipermail/coreboot/2016-November/082463.html)
- [Billet sur un forum d'un allemand à propos de l'installation d'un
  processeur Core Quad sur un T500](https://thinkpad-forum.de/threads/199129)


Une note à propos des cartes graphiques
=================

Certains modèles ont une carte graphique Intel, pendant que d'autres ont à la
fois une ATI et une Intel; c'est référencé en tant que "Double Graphiques
(Dual Graphics)" (précedemment "graphiques échangeables").
Dans le programme *BIOS setup* pour le lenovobios, vous pouvez spécifier au
système d'utiliser un des deux (mais pas les deux à la fois).

Libreboot est connu pour marcher sur des systèmes avec seulement la carte
graphique Intel, utilisant l'initialisation native des graphiques. Sur des
systèmes avec graphiques échangeables, la carte graphique Intel est utilisée
et celle ATI est désactivée, donc l'initialisation native des graphiques
marche partout pareil.

Pâte thermique processeur requise
==================

Voyez pour la [\#pâte](#paste).

Taille de puce flash {#flashchips}
===============

Utilisez ceci pour la trouver:
    
    # flashrom -p internal

Adresse MAC {#macadress}
===========

Référez-vous au document [mac\_address.md](../hardware/mac_address.md).

Configuration initiale BBB
=========================

Référez-vous au document [bbb\_setup.md](bbb_setup.md) sur comment configurer
le BBB pour le flashage.

Le suivant montre comment connecter une pince au BBB (sur la broche P9), pour
un SOIC-16 (pince: Pomona 5252):

    POMONA 5252 (corrélez avec le guide sur le BBB)
    ===  ethernet, jack et port VGA ====
     NC              -       - 21
     1               -       - 17
     NC              -       - NC
     NC              -       - NC
     NC              -       - NC
     NC              -       - NC
     18              -       - 3.3V (PSU)
     22              -       - NC - c'est le pin 1 sur la puce flash
    ===  port SATA ===
    C'est comme ceci que vous connecterez. Les nombres font références au
    numéro de pins sur le BBB, sur les fiches près de la fiche mâle DC.


Le suivant montre comment connecter une pince au BBB (sur la broche P9), pour
un SOIC-8 (pince: Pomona 5250):

    POMONA 5250 (corrélez avec le guide sur le BBB)
    ===  emplacements RAM  ====
     18              -       - 1
     22              -       - NC
     NC              -       - 21
     3.3V (PSU)      -       - 17 - c'est le pin 1 sur la puce flash
    ===  emplacement ou la prise mâle AC est connectée ===
    C'est comme ceci que vous connecterez. Les nombres font références au
    numéro de pins sur le BBB, sur les fiches près de la fiche mâle DC.

Désassemblage
-------------

Enlevez tous les vis:\
![](images/t500/0000.jpg)\

Il est conseillé, tout au long du désassemblage, de placer n'importe quel vis
que vous avez enlevé dans le même arangement/disposition. Les photos suivant
illustrent le propos:\
![](images/t500/0001.jpg) ![](images/t500/0002.jpg)

Enlevez le DD/SSD et lecteur CD:\
![](images/t500/0003.jpg) ![](images/t500/0004.jpg)

Enlevez le repose paume:\
![](images/t500/0005.jpg) ![](images/t500/0006.jpg)

Enlevez le clavier et le cadre arrière:\
![](images/t500/0007.jpg) ![](images/t500/0008.jpg)
![](images/t500/0009.jpg) ![](images/t500/0010.jpg)
![](images/t500/0011.jpg) ![](images/t500/0012.jpg)

Si vous avez une carte 3G/WWAN et/ou un lecteur de carte SIM, enlevez-les
définitivement. La carte WWAN-3G contient du logiciel propriétaire à
l'intérieur; la technologie est identitique à celle utilisée dans les
téléphones mobiles, donc ça peut aussi traquer vos mouvements.
![](images/t500/0013.jpg) ![](images/t500/0017.jpg)
![](images/t500/0018.jpg)

Enlevez ce panneau métallique, puis ensuite enlevez la puce wifi:\
![](images/t500/0014.jpg) ![](images/t500/0015.jpg)
![](images/t500/0016.jpg)

Enlevez les hauts-parleurs:\
![](images/t500/0019.jpg) ![](images/t500/0020.jpg)
![](images/t500/0021.jpg) ![](images/t500/0022.jpg)
![](images/t500/0023.jpg) ![](images/t500/0024.jpg)
![](images/t500/0025.jpg)

Enlevez la batterie NVRAM (déjà enlevée dans cette photo):\
![](images/t500/0026.jpg)

Quand vous ré-assemblez, vous serez en train de remplacer la puce WiFi 
avec une autre. Ces deux vis ne tiennent rien ensemble, mais ils sont inclus
dans la machine parce que les trous de vis pour les cartes de moitié la taille
sont différents, donc utilisez les si vous allez installer un carte de moitié
la taille:\
![](images/t500/0027.jpg)

Déroutez les fils d'antenne:\
![](images/t500/0028.jpg) ![](images/t500/0029.jpg)
![](images/t500/0030.jpg) ![](images/t500/0031.jpg)

Déconnectez le câble LCD de la carte mère:\
![](images/t500/0032.jpg) ![](images/t500/0033.jpg)

Enlevez les vis de la charnière de l'ensemble LCD, puis enlevez l'écran LCD:\
![](images/t500/0034.jpg) ![](images/t500/0035.jpg)
![](images/t500/0036.jpg)

Enlevez le ventilateur et le dissipateur de chaleur:
![](images/t500/0037.jpg) ![](images/t500/0038.jpg)
![](images/t500/0039.jpg)

Enlevez ce vis:\
![](images/t500/0040.jpg)

Enlevez ces câbles, en gardant une note de comment et dans quel
disposition/arangement sont-ils connectés:\
![](images/t500/0041.jpg) ![](images/t500/0042.jpg)
![](images/t500/0043.jpg) ![](images/t500/0044.jpg)
![](images/t500/0045.jpg) ![](images/t500/0046.jpg)
![](images/t500/0047.jpg) ![](images/t500/0048.jpg)
![](images/t500/0049.jpg)


Déconnectez la prise d'alimentation:\
![](images/t500/0050.jpg) ![](images/t500/0051.jpg)

Enlevez la carte mère et la cage de la base (le trou marqué est par où ces
câbles sont passés):\
![](images/t500/0052.jpg) ![](images/t500/0053.jpg)

Enlevez tous les vis, en les arrangeants dans la même disposition que quand
vous placez les vis sur une surface et que vous marquez chaque trou de vis
(c'est pour réduire la possibilité de les remettre dans les mauvais trous):\
![](images/t500/0054.jpg) ![](images/t500/0055.jpg)

D'ailleurs, enlevez ceci:\
![](images/t500/0056.jpg) ![](images/t500/0057.jpg)

Séparez la carte mère de la cage:\
![](images/t500/0058.jpg) ![](images/t500/0059.jpg)

La puce flash est à côté des emplacements mémoire. Sur cette machine, c'était
un puce flash SOIC-8 (4Mo ou 32Mo):\
![](images/t500/0060.jpg)

Connectez votre programmeur, puis connectez la masse (GND) et le 3.3V\
![](images/t500/0061.jpg)\
![](images/t400/0067.jpg) ![](images/t400/0069.jpg)
![](images/t400/0070.jpg) ![](images/t400/0071.jpg)

Une alimentation (NdT:*PSU*) 3.3V dédiée a été utilisé pour créer ce guide,
mais une alimentation ATX fait aussi l'affaire:\
![](images/t400/0072.jpg)

Bien sûr, soyez certain d'allumer votre alimentation:\
![](images/x200/disassembly/0013.jpg)

Maintenant, vous devriez être prêt à installer libreboot.

Les binaires de flashrom pour l'architecture ARM (testé sur un BBB) sont
distribués/fournis dans libreboot\_util. Alternativement, libreboot distribue
aussi le code source de flashrom pouvant être compilé.

Authentifiez-vous en tant que root sur votre BBB, en utilisant les
instructions dans le doc
[bbb\_setup.html\#bbb\_access](bbb_setup.html#bbb_access).

Testez afin de savoir si flashrom marche:

    # ./flashrom -p linux_spi:dev=/dev/spidev1.0,spispeed=512\

Dans ce cas là, la sortie était:

    flashrom v0.9.7-r1854 on Linux 3.8.13-bone47 (armv7l)
    flashrom is free software, get the source code at http://www.flashrom.org
    Calibrating delay loop... OK.
    Found Macronix flash chip "MX25L6405(D)" (8192 kB, SPI) on linux_spi.
    Found Macronix flash chip "MX25L6406E/MX25L6436E" (8192 kB, SPI) on linux_spi.
    Found Macronix flash chip "MX25L6445E/MX25L6473E" (8192 kB, SPI) on linux_spi.
    Multiple flash chip definitions match the detected chip(s): "MX25L6405(D)", "MX25L6406E/MX25L6436E", "MX25L6445E/MX25L6473E"
    Please specify which chip definition to use with the -c <chipname> option.

Comment sauvegarder factory.rom (changez l'option -c en accordance selon votre
puce flash):

    # ./flashrom -p linux_spi:dev=/dev/spidev1.0,spispeed=512 -r

factory.rom

    # ./flashrom -p linux_spi:dev=/dev/spidev1.0,spispeed=512 -r

factory1.rom

    # ./flashrom -p linux_spi:dev=/dev/spidev1.0,spispeed=512 -r

factory2.rom

Note: l'option `-c` n'est pas nécessaire dans la version de flashrom patchée
par libreboot, parce que les définitions redondantes de puces flash dans
*flashchips.c* ont été enlevée.

Maintenant comparez les 3 images:

    # sha512sum factory*.rom

Si les hashs correspondent, alors copiez juste l'un d'eux (le factory.rom)
dans un endroit sûr (sur un disque connecté sur un autre système, pas le BBB).
C'est utile pour le travail d'ingénérie inversé, au cas où il y a un
comportement désirable dans le micrologiciel originel qui pourrait être
répliqué dans coreboot et libreboot.

Suivez les instructions dans le document 
[../hardware/gm45\_remove\_me.html\#ich9gen](../hardware/gm45_remove_me.html#ich9gen)
pour changer l'adresse MAC à l'intérieur de l'image ROM de libreboot, avant de
la flasher. Bien qu'il y a une adresse MAC par défaut à l'intérieur de l'image
ROM, c'est ce que vous voulez. *Soyez sûr de toujours changer l'adresse MAC
par une qui est correcte pour votre machine.*

Maintenant flashez là:

    # ./flashrom -p linux_spi:dev=/dev/spidev1.0,spispeed=512 -w
        path/to/libreboot/rom/image.rom -V

![](images/x200/disassembly/0015.jpg)

Vous verrez peut-être des erreurs, mais si ça dit `Verifying flash...
VERIFIED` à la fin, alors c'est flashé et ça devrait démarrer. Si vous voyez
des erreurs, essayez encore (et encore et encore); le message `Chip content is
identical to the requested image` est aussi un indicateur d'une installation
fructueuse.

Exemple de sortie lors de l'exécution de la commande ci-dessus:

    flashrom v0.9.7-r1854 on Linux 3.8.13-bone47 (armv7l)
    flashrom is free software, get the source code at http://www.flashrom.org
    Calibrating delay loop... OK.
    Found Macronix flash chip "MX25L6405(D)" (8192 kB, SPI) on linux_spi.
    Reading old flash chip contents... done.
    Erasing and writing flash chip... FAILED at 0x00001000! Expected=0xff, Found=0x00, failed byte count from 0x00000000-0x0000ffff: 0xd716
    ERASE FAILED!
    Reading current flash chip contents... done. Looking for another erase function.
    Erase/write done.
    Verifying flash... VERIFIED.

Pâte thermique (IMPORTANT)
=========================

Parce qu'une partie de cette procédhre a nécessité d'enlever le dissipateur de
chaleur, vous aurez besoin d'appliquer une nouvelle pâte. L'Arctic MX-4 est
ok. Vous aurez aussi besoin pour nettoyer d'alcool isopropyl et de tissus anti électricité
statique.

Quand vous réinstallez le dissipateur de chaleur, vous devez d'abord nettoyer
toute la vieille pâte avec l'alcool/le tissu. Puis ensuite appliquer la
nouvelle pâte. L'Arctic MX-4 est d'ailleurs bien mieux que celle utilisée par
défaut sur ces machines.

![](images/t400/paste.jpg)

NOTE: la photo ci-dessus est dans des buts d'illustration seulement, et ne
montre pas comment appliquer proprement la pâte thermique. D'autres guides en
ligne détaillent la bonne procédure d'application.

WiFi
====

Le T500 est fournit avec un jeu de puce WiFi Intel, qui ne marche pas sans
logiciel propriétaire. Pour une liste de jeux de puces qui marchent sans
logiciel propriétaire, voyez le document
[../hardware/\#recommended\_wifi](../hardware/#recommended_wifi).

Certains ordinateurs portables T500 peuvent être fournis avec un jeu de puce
Atheros, mais ont seulement le protocole 802.11g.

Il est recommandé que vous installiez un nouveau jeu de puce WiFi. Ça peut
seulement être fait après avoir installé Libreboot, parce que le micrologiciel
original a une liste blanche de puces approuvés, et refusera de démarrer si
vous utilisez un carte wifi 'non autorisée'.

Les photos suivantes montrent un Atheros AR5B95 en train d'être installé, pour
remplacer la puce Intel dont le T500 a été fourni avec:
![](images/t400/0012.jpg) ![](images/t400/ar5b95.jpg)

WWAN
====

Si vous avez une carte WWAN/3G et/ou un lecteur de carte sim, enlevez les
définitevement. La carte WWAN-3G a du micrologiciel propriétaire à
l'intérieur; la technologie est identique à celle utilisée dans les téléphones
mobiles, car elle peut traquer vos mouvements.

Mémoire
======

Vous aurez besoin que de la RAM de type DDR3 SODIMM PC3-8500 soit installé, en
paire identique en vitesse/taille. Les paires non correspondantes ne
marcheront pas. Vous pouvez aussi installer un seul module (voulant dire que
l'un des emplacements sera vide) dans l'emplacement (*slot*) 0.

Soyez sûr que la RAM que vous achetez soit de densité 2Rx8.

[Cette page](http://www.forum.thinkpads.com/viewtopic.php?p=760721) pourrait
être utile pour des informations sur la compatibilité de la RAM (note:
l'initialisation de la RAM dans coreboot est différente, donc cette page
pourrait être des conneries)

La photo suivante montre 8Go (2x4Go) de RAM installée:\
![](images/t400/memory.jpg)

Démarrez le!
--------

Vous devriez voir quelque chose comme ceci:

![](images/t400/boot0.jpg) ![](images/t400/boot1.jpg)

Maintenant [installez GNU+Linux](../gnulinux/).

Copyright © 2015 Leah Rowe <info@minifree.org>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
