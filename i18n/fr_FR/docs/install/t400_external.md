---
title: Flashage du T400 avec un BeagleBone Black
...

Instructions initiales de flashage pour le T400.

Ce guide est pour ceux voulant Libreboot sur leur ThinkPad T400 alors qu'ils
ont toujours le BIOS de Lenovo présent. Ce guide peut aussi être suivi
(adapté) si vous bousillez (*brick*) votre T400, pour vous montrer comment le
remettre d'aplomb.

Un ["MMM"](https://download.lenovo.com/ibmdl/pub/pc/pccbbs/mobiles_pdf/43y6629_05.pdf#page=386)
(Manuel de Maintenance Matérielle <=> Hardware Maintenance Manual)
qui détaille le processus de \[dés\]assemblage de ce modèle est disponible. Soyez
vigilants lors du réassemblage de l'ordinateur portable car les vis sur la
page 114 (avec le titre "1130 Keyboard bezel") sont échangées/inversées et si
vous suivez le manuel vous percerez un trou à travers le coin
supérieur droit du cadre.

Port série {#serial_port}
-----------

Le déboguage EHCI n'est peut-être pas nécessaire. Il a été rapporté que la
station d'appareillage (dock) pour cet ordinateur portable a un port série,
donc il serait peut-être possible d'utiliser ça à la place.

Une note à propos des processeurs
=================

Le [ThinkWiki](http://www.thinkwiki.org/wiki/Category:R400) a une liste de
processeurs pour ce système. Le Core 2 Duo P8400, P8600 et P8700 marcherait dans
libreboot. 
Les Core 2 Duo T9600, T9500, T9550 et T9900 sont tous compatibles, ça nous a
été rapporté par des utilisateurs. 

Processeurs 4 coeurs
--------------

Il est très semblable que ça soit compatible, mais ça demande des
modifications matérielles.
C'est basé sur des informations d'un billet d'un allemand sur un forum, trouvé
sur les listes de diffusions de coreboot, à
propos de l'installation d'un processeur Core Quad sur un T500. Actuellement
un travail en cours et aucun guide n'est disponible.

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

Enlevez *tous* les vis, les plaçant dans l'ordre dans lesquels vous les avez
enlevés:
![](images/t400/0001.jpg) ![](images/t400/0002.jpg)

Enlevez ces trois vis puis enlevez le cadre arrière:\
![](images/t400/0003.jpg) ![](images/t400/0004.jpg)
![](images/t400/0005.jpg) ![](images/t400/0006.jpg)

Enlevez les haut-parleurs:\
![](images/t400/0007.jpg) ![](images/t400/0008.jpg)
![](images/t400/0009.jpg) ![](images/t400/0010.jpg)
![](images/t400/0011.jpg)

Enlevez la WiFi:\
![](images/t400/0012.jpg) ![](images/t400/0013.jpg)

Enlevez ce câble:\
![](images/t400/0014.jpg) ![](images/t400/0015.jpg)
![](images/t400/0016.jpg) ![](images/t400/0017.jpg)
![](images/t400/0018.jpg)

Déroutez ces fils d'antenne:\
![](images/t400/0019.jpg) ![](images/t400/0020.jpg)
![](images/t400/0021.jpg) ![](images/t400/0022.jpg)
![](images/t400/0023.jpg)

Enlevez l'assemblage de l'écran LCD:\
![](images/t400/0024.jpg) ![](images/t400/0025.jpg)
![](images/t400/0026.jpg) ![](images/t400/0027.jpg)
![](images/t400/0028.jpg) ![](images/t400/0029.jpg)
![](images/t400/0030.jpg) ![](images/t400/0031.jpg)

Déconnectez la batterie NVRAM:\
![](images/t400/0033.jpg)

Déconnectez le ventilateur:\
![](images/t400/0034.jpg)

Dévissez ces vis:\
![](images/t400/0035.jpg) ![](images/t400/0036.jpg)
![](images/t400/0037.jpg) ![](images/t400/0038.jpg)

Dévissez le dissipateur de chaleur, puis soulevez-le:\
![](images/t400/0039.jpg) ![](images/t400/0040.jpg)

Déconnectez la prise d'alimentation:\
![](images/t400/0041.jpg) ![](images/t400/0042.jpg)

Désserrez ceci:\
![](images/t400/0043.jpg)

Enlevez ceci:\
![](images/t400/0044.jpg) ![](images/t400/0045.jpg)
![](images/t400/0046.jpg) ![](images/t400/0047.jpg)
![](images/t400/0048.jpg)

Dévissez ces vis:\
![](images/t400/0049.jpg) ![](images/t400/0050.jpg)

Enlevez ça:\
![](images/t400/0051.jpg) ![](images/t400/0052.jpg)

Dévissez ceci:\
![](images/t400/0053.jpg)

Enlevez la carte mère (la cage est encore attaché) depuis le côté de votre
main droite, puis soulevez là en dehors:\
![](images/t400/0054.jpg) ![](images/t400/0055.jpg)
![](images/t400/0056.jpg)

Enlevez toutes les vis, marquez chaque trou de telle façon que vous saurez les
réinsérer. Vous devriez placer les vis dans une disposition correspondant à
l'ordre qu'ils étaient avant enlevage:\
![](images/t400/0057.jpg) ![](images/t400/0058.jpg)
![](images/t400/0059.jpg) ![](images/t400/0060.jpg)
![](images/t400/0061.jpg) ![](images/t400/0062.jpg)

Séparez la carte mère de l'ensemble:\
![](images/t400/0063.jpg) ![](images/t400/0064.jpg)

Connectez votre programmeur, puis connectez la masse et le 3.3V\
![](images/t400/0065.jpg) ![](images/t400/0066.jpg)
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

Le T400 est fournit avec un jeu de puce WiFi Intel, qui ne marche pas sans
logiciel propriétaire. Pour une liste de jeux de puces qui marchent sans
logiciel propriétaire, voyez le document
[../hardware/\#recommended\_wifi](../hardware/#recommended_wifi).

Certains ordinateurs portables T400 peuvent être fournis avec un jeu de puce
Atheros, mais ont seulement le protocole 802.11g.

Il est recommandé que vous installiez un nouveau jeu de puce WiFi. Ça peut
seulement être fait après avoir installé Libreboot, parce que le micrologiciel
original a une liste blanche de puces approuvés, et refusera de démarrer si
vous utilisez un carte wifi 'non autorisée'.

Les photos suivantes montrent un Atheros AR5B95 en train d'être installé, pour
remplacer la puce Intel dont le T400 a été fourni avec:
![](images/t400/0012.jpg) ![](images/t400/ar5b95.jpg)

WWAN
====

Si vous avez une carte WWAN/3G et/ou un lecteur de carte sim, enlevez les
définitevement. La carte WWAN-3G a du micrologiciel propriétaire à
l'intérieur; la technologie est identique à celle utilisée dans les téléphones
mobiles, car elle peut traquer vos mouvements.

À ne pas confondre avec le WiFi (le WiFi est OK).

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
