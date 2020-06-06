---
title: Guide de recouvrement pour le ThinkPad X60
x-toc-enable: true
...


Cette section documente comment récupérer après un mauvais flashage empêchant
le démarrage de votre ThinkPad X60.



Brick type 1: bucts not reset. {#bucts_brick}
==============================

Vous avez encore le BIOS Lenono, ou Libreboot était en cours d'exécution et
vous avez flashé une autre ROM; et 'bucts 1' était défini et la ROM n'a pas
été `dd`\* ou alors le BIOS Lenovo était présent et libreboot n'était pas
flashé.\

Dans ce cas là, la récupération est facile: réinitialisez BUC.TS à 0 en
enlevant cette pièce jaune cmos (c'est une batterie) puis en la remettant une
ou deux minutes après:\
![](../images/x60_unbrick/0004.jpg)\

\*Ces commandes dd devraient être appliquées à toutes images ROM pour X60
nouvellement compilés (les images ROM dans les archives des binaires ont déjà
ça d'appliqué!):\

    dd if=coreboot.rom of=top64k.bin bs=1 skip=\$\[\$(stat -c %s coreboot.rom) - 0x10000\] count=64k
    dd if=coreboot.rom bs=1 skip=\$\[\$(stat -c %s coreboot.rom) - 0x20000\] count=64k | hexdump
    dd if=top64k.bin of=coreboot.rom bs=1 seek=\$\[\$(stat -c %s coreboot.rom) - 0x20000\] count=64k conv=notrunc

Faire ceci rend la ROM prête pour le flashage sur un système exécutant
toujours le BIOS Lenovo, en utilisant ces instructions:
<http://www.coreboot.org/Board:lenovo/x60/Installation>

Mauvaise rom (ou erreur utilisateur), le système ne démarrera pas {#recovery}
===========================================

Dans ce scénario, vous avez compilé une ROM qui avait une configuration
incorrecte, ou il y a un véritable bug empêchant votre système de démarrer.
Ou, peut-être, vous avez défini BUC.TS à 0 et avez éteint après le premier
flash pendant que le BIOS Lenovo était en train de s'exécuter. Dans n'importe
quel cas, votre système est bousillé (*bricked*, rendu aussi utile qu'une
brique) et ne voudra pas démarrer du tout.

Le "débriquage" (*unbricking*) signifie qu'on flashe une ROM connue pour
marcher. Le problème: vous ne pouvez pas démarrer le problème, rendant cette
tâche difficile. Dans cette situation, du matériel externe (voir les requis
matériels ci-dessus) est nécessaire, pouvant flasher la puce SPI (où libreboot
réside).

Enlevez ces vis:\
![](../images/x60_unbrick/0000.jpg)

Poussez le clavier vers l'avant (avec précautions):\
![](../images/x60_unbrick/0001.jpg)

Soulevez le clavier et déconnectez le de la carte mère:\
![](../images/x60_unbrick/0002.jpg)

Attrapez le côté droit du chassis et forcez le gentiment vers le dehors, et
faîtes levier pour enlever le reste du chassis:\
![](../images/x60_unbrick/0003.jpg)

Vous devriez avoir maintenant ceci:\
![](../images/x60_unbrick/0004.jpg)

Déconnectez les câbles d'antenne WiFi, les câbles du modem et le
haut-parleur:\
![](../images/x60_unbrick/0005.jpg)

Déroutez les câbles en suivant le chemin inverse, soulevant avec précaute
l'adhésif qui les maintiennent en place, puis, déconnectez les câbles du modem
(autre extrémité) et d'ailmentation et déroutez tous les câbles de façon
qu'ils pendent à côté de la charnière de l'écran sur le côté droit:\
![](../images/x60_unbrick/0006.jpg)

Déconnectez l'écran de la carte mère, et déroutez le câble antenne gris,
soulevant avec précaution l'adhésif qui le maintient en place:\
![](../images/x60_unbrick/0008.jpg)

Soulevez avec précaution l'adhésif restant et déroutez le câble antenne gauche
pour qu'il soit détendu:\
![](../images/x60_unbrick/0009.jpg)

Enlevez le vis qui est surligné (n'enlevez PAS l'autre; il maintient une
partie du dissipateur de châleur (l'autre côté) en place):\
![](../images/x60_unbrick/0011.jpg)

Enlevez ces vis:\
![](../images/x60_unbrick/0012.jpg)

Enlevez avec précaution la plaque, comme ceci:\
![](../images/x60_unbrick/0013.jpg)

Enlevez le connecteur SATA:\
![](../images/x60_unbrick/0014.jpg)

Maintenant enlevez (gentiment) la carte mère et mettez de côté l'écran LCD/ le
chassis:\
![](../images/x60_unbrick/0015.jpg)

Repliez cet adhésif et tenez le avec quelque chose. Ce qui est surligné est la
puce flash SPI:\
![](../images/x60_unbrick/0016.jpg)

Maintenant branchez le BBB et la pince Pomona avec votre alimentation.\
Référez-vous au document [bbb\_setup](bbb_setup.md) sur comment mettre en
place le BBB pour le flashage.

*Notez, le guide mentionne une alim DC 3.3V mais vous n'avez pas besoin de ça
sur le T60: si vous n'avez pas ou ne voulez pas utiliser une alimentation
externe, alors assurez-vous de ne pas connecter les fils/câbles 3.3V
mentionnés dans le guide; à la place, connectez l'adaptateur DC (celui qui
charge normalement votre batterie), comme ça la carte est alimentée (mais ne
la démarrez/l'allumez pas)*
![](../images/x60_unbrick/0017.jpg)\
Corrélez le suivant avec le lien du guide BBB partagé ci-dessus:

    POMONA 5250:
    ===  "doigt d'or" et interrupteur WiFi ====
     18              -       - 1
     22              -       - NC                    ---------- prise jack audio sont sur cette extrémité
     NC              -       - 21
     3.3V (alim)      -       - 17 - c'est le pin 1 sur la puce flash
    === ventilo processeur ===
    C'est comme ceci que vous connecterez. Les nombres font références au
    numéro de pins sur le BBB, sur les fiches près de la fiche mâle DC.

Connectez le BBB et le pomona (dans cette image, une alimentation 3.3V DC a
été utilisée):\
![](images/x60/th_bbb_flashing.jpg)

Les binaires de flashrom pour l'architecture ARM (testé sur un BBB) sont
distribués/fournis dans libreboot\_util. Alternativement, libreboot distribue
aussi le code source de flashrom pouvant être compilé.

Connectez-vous via SSH sur le BBB:

    # ./flashrom -p linux_spi:dev=/dev/spidev1.0,spispeed=512 -w yourrom.rom

La sortie de cette commande devrait dire `Verifying flash... VERIFIED` à la
fin. Si flashrom se plaint de multiples définitions de puces flash détectées,
alors choisissez l'une d'elles en suivant les instructions mentionnées dans la
sortie de la commande.

Enlevez le programmeur et mettez le ailleurs quelque part. Remettez l'adhésif
en pressant fermement:\
![](../images/x60_unbrick/0026.jpg)

Votre chassis vide:\
![](../images/x60_unbrick/0027.jpg)

Remettez la carte mère dedans:\
![](../images/x60_unbrick/0028.jpg)

Reconnectez le(s) SATA:\
![](../images/x60_unbrick/0029.jpg)

Remettez la plaque et réinsérez ces vis:\
![](../images/x60_unbrick/0030.jpg)

Reroutez ce câble antenne autour du ventilateur et réappliquez un adhésif:\
![](../images/x60_unbrick/0031.jpg)

Routez le câble ici puis, (pas montré, à cause d'une erreur de ma part)
reconnectez le câble de l'écran à la carte mère et réinsérez les vis:\
![](../images/x60_unbrick/0032.jpg)

Réinsérez ce vis:\
![](../images/x60_unbrick/0033.jpg)

Routez le câble antenne noir comme ceci:\
![](../images/x60_unbrick/0034.jpg)

Rentrez-le bien comme il faut comme ceci:\
![](../images/x60_unbrick/0035.jpg)

Routez le câble modem comme ceci:\
![](../images/x60_unbrick/0036.jpg)

Connectez le câble modem à la carte mère et rentrez-le bien comme il faut
comme ceci:\
![](../images/x60_unbrick/0037.jpg)

Routez la connection de l'alimentation et connectez-la à la carte mère comme
ceci:\
![](../images/x60_unbrick/0038.jpg)

Routez les câbles d'antenne et modem bien comme il faut comme ceci:\
![](../images/x60_unbrick/0039.jpg)

Connectez les câbles antenne WiFi. Au début du tuto, ce système avait une puce
WiFi Intel. Ici vous pouvez voir que je l'ai remplacé par un Atheros AR5B95
(supporte le protocole 802.11n et peut être utilisé sans blobs):\
![](../images/x60_unbrick/0040.jpg)

Connectez le câble modem:\
![](../images/x60_unbrick/0041.jpg)

Connectez l'haut-parleur:\
![](../images/x60_unbrick/0042.jpg)

Vous devriez maintenant avoir ceci:\
![](../images/x60_unbrick/0043.jpg)

Reconnectez le chassis supérieur:\
![](../images/x60_unbrick/0044.jpg)

Reconnectez le clavier:\
![](../images/x60_unbrick/0045.jpg)

Réinsérez les vis enlevés plus tôt:\
![](../images/x60_unbrick/0046.jpg)

Allumez!\
![](../images/x60_unbrick/0047.jpg)

Système d'exploitation:\
![](../images/x60_unbrick/0049.jpg)

Copyright © 2014, 2015 Leah Rowe <info@minifree.org>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
