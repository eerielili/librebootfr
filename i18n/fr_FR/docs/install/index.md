---
Titre: Instructions d'installation
...

Cette section concerne l'installation de Libreboot sur les cibles supportées.

NOTE: si vous éxecutiez `flashrom -p internal` pour le flashage basé logiciellement,  et vous avez une erreur concernant l'accés à `/dev/mem`, vous devrez redémarrer avec le paramètre de kernel `iomem=relaxed` avant d'éxecuter flashrom, ou utiliser un kernel qui a `CONFIG_STRICT_DEVMEM ` non activé.

Quels systèmes sont compatibles avec Libreboot ?
--------------------------------------------

-   [Information à propos de la compatibilité matérielle Libreboot](../hardware)


Informations générales
-------------------

-   [Information à propos des images ROM libreboot](#rom)

Flashage via des méthodes logicielles, sur système:
-----------------------------------------

-   [Comment mettre à jour ou installer libreboot sur tout les systèmes](#flashrom)

-   [How to update or install libreboot on all systems](#flashrom)
-   [ASUS KFSN4-DRE](#flashrom)
-   [ThinkPad X60/T60 (if running Lenovo BIOS)](#flashrom_lenovobios)
-   [Apple MacBook2,1](#flashrom_macbook21)
-   [ASUS Chromebook C201](c201.md)

Mettre en place les programmeurs, pour le flashage externe SPI
-----------------------------------------------------------------

-   [BeagleBone Black Setup](bbb_setup.md)
-   [Raspberry Pi Setup](rpi_setup.md)

Flashage via les méthodes matérielles, sur le système:
-----------------------------------------

-   [Gigabyte GA-G41-ES2L](ga-g41m-es2l.md)
-   [Intel D510MO](d510mo.md)
-   [Intel D945GCLF](d945gclf.md)
-   [ASUS KGPE-D16](kgpe-d16.md)
-   [ASUS KCMA-D8](kcma-d8.md)
-   [ASUS Chromebook C201](c201.md)
-   [ThinkPad X60](x60_unbrick.md)
-   [ThinkPad X60 Tablet](x60tablet_unbrick.md)
-   [ThinkPad T60](t60_unbrick.md)
-   [ThinkPad X200/X200S/X200T](x200_external.md)
-   [ThinkPad R400](r400_external.md)
-   [ThinkPad T400](t400_external.md)
-   [ThinkPad T500](t500_external.md)
-   [ThinkPad W500](t500_external.md)

Information à propos des images ROM libreboot {#rom}
======================================

Libreboot distribue des images ROM pré-compilée, construites à partir du code source de libreboot. Ces images sont fournies pour le confort de l'utilisateur, comme ça ils n'ont pas à construire quoi que ce soit depuis la source de leur propre chef.

Ces images ROM dans chaque archive utilise le suivant à la fin de leur nom de fichier, si elles sont construites avec la charge utile GRUB: `*_*keymap*_*mode*.rom`

Modes disponibles: vesafb ou txtmode. Les images ROM vesafb sont recommandés pour un usage régulier, mais quand vous flashez pour la première fois utilisez la version txtmode, car elle vient avec Memtext86+, qui demande le text-mode au lieu du framebuffer d'habitude utilisé par l'initialisation des graphiques natifs de coreboot.
La machine devrait être testée avec Memtest86+ après chaque réassemblage ou changement du bios constructeur (d'origine) à cause de différences dans le code de raminit.

`keymap` peut être une des quelques mappage de touches que le clavier supporte (il y en a quelques un), qui affecte la configuration de la disposition du clavier qui est utilisé dans GRUB. Ca n'a pas d'importance quel image ROM vous utilisez ici, tant que le mappage des touches dans GNU+Linux est concerné.


Les mappages de touches sont nommés en accordance avec chaque disposition de clavier supportées dans GRUB. Pour apprendre comment ces mappages de touches sont créés, regardez [../grub/\#grub\_keyboard](../grub/#grub_keyboard)


QEMU
----

Libreboot arrive avec des images ROM construites pour QEMU par défaut:

Des exemples de comment utiliser les images ROM de libreboot dans QEMU:

    $ qemu-system-i386 -M q35 -m 512 -bios qemu_q35_ich9_keymap_mode.rom
    $ qemu-system-i386 -M pc -m 512 -bios qemu_i440fx_piix4_keymap_mode.rom

Vous pouvez optionnellement spécifié l'argument `-serial stdio`, comme ça QEMU émulera un terminal en série sur l'entrée/sortie standard (très certainement votre émulateur de terminal ou TTY).

D'autres arguments sont disponibles pour QEMU. Le manuel contiendra plus d'informations.

Comment mettre à jour ou installer libreboot (si vous êtes déjà en train d'éxecuter libreboot ou coreboot) {#flashrom}
=====================================================================================

Sur toutes les cibles actuelles, mettre à jour libreboot peut être accomplit sans désassemblage,
et de ce fait, sans avoir à re-flasher extérieurement en utilisant n'important quel matériel spécialisé.
En d'autres mots, vous pouvez tout faire de façon logicielle, directement depuis le système d'exploitation qui
est en cours d'éxecution sur votre système Libreboot.

*Si vous êtes en train d'utiliser `libreboot_src` ou git, alors rendez-vous bien sûr
que vous avez compilé les sources en premier (regardez [../git/\#build](../git/#build)).*

Regardez à la [liste des images ROM](#rom) pour voir quelle image est compatible avec votre appareil.


Êtes-vous en train d'exécuter le micrologiciel d'origine propriétaire ?
-------------------------------------------------------------

Si vous êtes en train d'éxecuter le micrologiciel propriétaire (ni libreboot ou coreboot), alors les instructions de flashage pour votre système vont être différentes.

Les utilisateurs du X60/T60 éxecutant le micrologiciel propriétaire devrait se référer à [\#flashrom\_lenovobios](#flashrom_lenovobios). Les utilisateurs de MacBook2,1 éxecutant l'EFI d'Apple devrait se référer à [\#flashrom\_macbook21](#flashrom_macbook21)

Les utilisateurs de X200, se référeront à [x200\_external.md](x200_external.md), ceux de R400 à [r400\_external.md](r400_external.md), ceux de T400 à [t400\_external.md](t400_external.md), et ceux de T500 et W500 à [t500\_external.md](t500_external.md)


ASUS KFSN4-DRE?
---------------

Le flashage interne devrait marcher comme il faut, même si vous êtes en train
de démarrer le micrologiciel propriétaire.

Libreboot manque en ce moment de documentation pour re-flasher 
extérieurement une puce flash LPC. Cependant, ces cartes mères ont la puce de flash 
à l'intérieur d'un socket PLCC, et il est possible d'échanger à chaud 
les puces. Si vous voulez sauvegarder votre image connu pour marcher, échangez
simplement à chaud la puce pour un qui est de même capacité, après avoir déchargé
une copie du micrologiciel courant (flashrom -p internal -r yourchosenname.rom), et
flashez ensuite cette puce avec l'image connue pour marcher. Vérifiez si le système
démarre encore, et si il démarre alors il est sécurisé de flashé la nouvelle image 
(parce que maintenant vous avez une sauvegarde de la vieille image).

Garder au moins une puce LPC PLCC en stock avec le micrologiciel fonctionnel dessus
est fortement recommandé, en cas de bousillage.

*N'ÉCHANGEZ PAS à chaud la puce avec vos mains nues. Utilisez un extracteur de puce PLCC.
Elles peuvent être trouvées en ligne. 
Voyez <http://www.coreboot.org/Developer_Manual/Tools#Chip_removal_tools>*

Vérifez la saisie HCL: [../hardware/kfsn4-dre.md](../hardware/kfsn4-dre.md)

ASUS KGPE-D16?
--------------

Si vous avez un BIOS propriétaire, vous avez besoin de flasher libreboot 
extérieurement. Voir [kgpe-d16.md](kgpe-d16.md)

Si vous déjà coreboot ou libreboot installé, sans protection contre l'écriture sur
la puce flash, alors vous pouvez le faire logiciellement (sinon, voir le lien ci-dessus).

*N'ÉCHANGEZ PAS à chaud la puce avec vos mains nues. Utilisez un extracteur de puce PDIP-8.
Elles peuvent être trouvées en ligne. 
Voyez <http://www.coreboot.org/Developer_Manual/Tools#Chip_removal_tools>*

Vérifier l'entrée HCL: [../hardware/kgpe-d16.md](../hardware/kgpe-d16.md)

ASUS KCMA-D8?
-------------

Si vous un BIOS propriétaire, nous avons besoin de flasher 
libreboot extérieurement. Voir [kmca-d8.md](kmca-d8.md).

Si vous avez déjà coreboot ou libreboot installé, sans protection 
contre l'écriture sur la puce flash,  alors vous pouvez le faire 
logiciellement (sinon, voir le lien ci-dessus).

*N'ÉCHANGEZ PAS à chaud la puce avec vos mains nues. Utilisez un extracteur de puce PDIP-8.
Elles peuvent être trouvées en ligne. 
Voyez <http://www.coreboot.org/Developer_Manual/Tools#Chip_removal_tools>*

Vérifier l'entrée HCL: [../hardware/kgpe-d16.md](../hardware/kgpe-d16.md)


Intel D945GCLF?
---------------

Si vous éxecutez le BIOS d'usine Intel original, alors vous aurez 
besoin de le flasher extérieurement. Pour des instructions sur comment
 faire celà, référez-vous [d945gclf.md](d945gclf.md).

Sinon, lisez les instructions générales en dessous pour utiliser 
le script *flash*.


Êtes-vous en train d'éxecuter libreboot (ou coreboot)?
--------------------------------------------------

Les utilisateurs X60/T60 devrait être OK avec ce guide.
Si vous avez protégé en écriture la puce flash, référez-vous 
silvouplait à [x60\_unbrick.md](x60_unbrick.md),
[x60tablet\_unbrick.md](x60tablet_unbrick.md) ou
[t60\_unbrick.md](t60_unbrick.md). *Cela ne s'applique pas probablement
 à vous. La majorité de gens ne protège pas en écriture la puce flash, donc
 vous ne l'avez probablement pas fait*

Similairement, c'est possible de protéger en écriture la puce flash dans 
coreboot ou libreboot sur les ordinateurs portables GM45 (X200/R400/T400/T500/W500).
Si vous faites ça, alors vous aurez besoin d'utiliser les liens au-dessus pour
le flashage, traitant votre ordinateur portable comme si il avait le micrologiciel
propriétaire (parce que la puce SPI protégé en écriture aura besoin d'un reflashage,
externe, comme c'est le cas quand on éxecute le micrologiciel propriétaire).

Si vous n'avez pas protégé en écriture la puce flash,ou elle est 
arrivé chez vous sans aucune protection en écriture (*libreboot ne protège 
pas en écriture la puce flash par défaut, donc ça s'applique à vous*), continuez à lire!


Adresse MAC sur GM45 (X200/R400/T400/T500/W500)
-----------------------------------------

*Utilisateurs du X200/R400/T400/T500/W500, prenez note:* L'adresse MAC pour le jeu de puces
ethernet embarqué est situé à l'intérieur de la puce flash. Les images ROM de Libreboot pour
ces ordinateurs portables contiennent une adresse MAC générique par défaut, mais ce n'est
pas que vous voulez. *Assurez vous bien de change l'adresse MAC à l'intérieur de l'image ROM 
avant de la flasher. Les instructions dans [../hardware/gm45\_remove\_me.html\#ich9gen](../hardware/gm45_remove_me.html#ich9gen)
montrent comment faire celà.

C'est important que vous changiez l'adresse MAC par défaut, avant flashage.
Elle sera imprimée sur un sticker en bas de l'ordinateur portable, ou il sera
imprimé sur un sticker à côté ou sous la RAM.
Alternativement, et assumant que votre micrologiciel en cours d'exécution à la
bonne adresse MAC en lui, vous pouvez la récupérer depuis votre système d'exploitation.


Apple iMac 5,2?
---------------

Le flashage interne marche, même quand on flashing depuis l'EFI d'Apple vers Libreboot.
Continuez à lire les instructions ci-dessous.

*NOTE*: Si vous flashez une plus vieille version de libreboot, la carte mère de l'iMac5,2 est compatible avec le MacBook2,1.
Flashez simplement une image ROM MacBook2,1, et ça devrait marcher.


Taille de la puce flash
---------------

Utilisez ceci pour trouver:

    # flashrom -p internal


Tout bon ?
---------

Excellent! On bouge...

Téléchargez l'archive *libreboot\_util.tar.xz*, et extrayez là. À
l'intérieur vous trouverez un répertoire appelé *flashrom*. Il contient
des fichiers éxecutables compilés statiquement de l'utilitaire *flashrom*, que
vous utiliserez pour re-flasher votre système libreboot.

Utilisez simplement *cd* sur votre terminal, pour vous mettre sur le répertoire
*libreboot\_util*. À l'intérieur, il y a un script appelé *flash*, qui détectera quelle
architecture de processeur vous avez (e.g. i686, x86\_64) et utilisera l'éxecutable
approprié. Il est aussi possible pour vous de construire ces éxecutables à partir
des archives de code source de libreboot.

Comment mettre à jour le contenu de la puce flash:

 `$ sudo ./flash update `[`yourrom.rom`](#rom)

Occasionellement, coreboot change le nom d'une carte mère. Si flashrom se plaint à
propos d'une carte mère qui ne correspond pas, mais vous êtes sur que vous choisissez
l'image ROM correcte, alors éxecutez cette commande alternative:

 `$ sudo ./flash forceupdate `[`yourrom.rom](#rom)

Vous devriez voir `Vérification du flash... VÉRIFIÉ.` écris à la fin de la sortie
de flashrom. *Éteignez* après que vous voyez ceci, et ensuite démarrez de nouveau après quelques secondes.

ThinkPad X60/T60: Guide d'installation initiale (si vous êtes en train d'éxecutez le micrologiciel propriétaire) {#flashroms_lenovobios}
==================================================================================

*Ceci est pour le ThinkPad X60 et T60 pendant que vous êtes en train 
d'éxecuter le BIOS de Lenovo. Si vous êtes déjà en train d'éxecuter 
coreboot ou libreboot, alors rendez-vous sur [\#flashrom](#flashrom)!*

*Si vous pouvez, rendez-vous sûr que la batterie RTC n'est pas déchargée. Une batterie RTC déchargé peut amener à un bousillage dû car la valeur du registre BUC n'est pas maintenue..*

*Si vous êtes en train de flasher un Lenovo ThinkPad T60, assurez-vous de lire
[../hardware/\#supported\_t60\_list](../hardware/#supported_t60_list)

*Si vous êtes en train d'utilisez libreboot\_src ou git, alors 
rendez-vous sûr que vous avez compilé les sources en premier 
(voir [../git/\#build](../git/#build))*

*Attention: ce guide n'instruirera pas l'utiliser sur comment sauvegarder
le micrologiciel original du BIOS Lenovo. Ces sauvegardes sont liés à chaque système, et
ne marcheront sur aucun autre. Pour ceci, référez-vous silvouplaît à <http://www.coreboot.org/Board:lenovo/x60/Installation>.*

*Si vous êtes en train d'utiliser libreboot 20150518, notez qu'il y
a une erreur dans le script de flashage. faites celà: *

    rm -f patch
    wget -O flash https://notabug.org/libreboot/libreboot/raw/9d850543ad90b72e0e333c98075530b31e5d23f1/flash
    chmod +x flash

La première moitié de la procédure est comme le suivant:

`$ sudo ./flash i945lenovo_firstflash `[`yourrom.rom`](#rom)

Vous devrez voir dans la sortie le suivant:

    Updated BUC.TS=1 - 64kb address ranges at 0xFFFE0000 and 0xFFFF0000 are
    swapped
    (A mis à jour BUC.TS=1 - La rangée d'adresses 64kb à 0xFFFE0000 et 0xFFFF0000 sont échangées.)

Vous devrez voir aussi celà dans la sortie:
    
    Your flash chip is in an unknown state
    ...
    FAILED
    ...
    DO NOT REBOOT OR POWEROFF

    (Votre puce de flash est dans un état inconnu
    ...
    A ÉCHOUÉ
    ...
    NE PAS REDÉMARRER OU ÉTEINDRE)

Voir ceci veut dire que l'opération était d'un succés *retentissant* !
*NE PANIQUEZ PAS*

Voir ce lien pour plus de détails:
<http://thread.gmane.org/gname.linux.bios.flashrom/575>.

Si le dessus est ce que vous voyez, alors *ÉTEIGNEZ* (mais n'enlevez pas le jus, espécialement la batterie RTC). Attendez 
quelques secondes et ensuite démarrez; libreboot est éxecuté, mais il y a une deuxième procédure nécessaire (voir
ci-dessous).

Quand vous avez démarré de nouveau, vous devez aussi faire ceci:

`$ sudo ./flash i945lenovo_secondflash `[`yourrom.rom`](#rom)

Si le flashage a échoué à ce stage, essayez le suivant:

`$ sudo ./flashrom/i686/flashrom -p internal:laptop=force_I_want_a_brick -w `[`yourrom.rom`](#rom)

Vous devriez voir dans la sortie le suivant:

	Updated BUC.TS=0 - 128kb address range 0xFFFE0000-0xFFFFFFFF is
    	untranslated
	(A mis à jour BUC.TS=0 - La rangé d'adresses 128kb 0xFFFE000-0xFFFFFFFF
	est non traduite)

Vous devriez aussi voir le suivant dans la sortie:

    Vérification du flash... VÉRIFIÉ.


MacBook 2,1: Guide d'installation initial (si vous êtes en train d'éxecuter le micrologiciel propriétaire) {#flashrom_macbook21}
============================================================================

*Si vous avez un MacBook1,1, référez-vous à [../hardware/\#information-about-the-macbook11](../hardware/#information-about-the-macbook11) pour les instructions de flashage.*

*Ceci est pour le MacBook2,1 pendant que vous éxecutez le micrologiciel EFI d'Apple. Si vous êtes déja en train d'éxecuter coreboot ou libreboot, alors allez à la section [\#flashrom](#flashrom)*

Rendez-vous sûr de lire les informations dans [../hardware/\#information-about-the-macbook21](../hardware/#information-about-the-macbook21)

*Attention: ce guide n'instruira pas l'utilisateur sur comment
sauvegarder le micrologiciel original EFI d'Apple. Pour ça, 
référez-vous à <http://www.coreboot.org/Board:apple/macbook21>.*

*Si vous êtes en train d'utiliser libreboot\_src ou git, alors rendez-vous sûr que vous avez compilé les sources en premier (voir [../git/\#build](../git/\#build))*

Jetez un oeil à la [liste des images ROM](#rom) pour voir quelle image est compatible avec votre appareil.

Utilisez ce script de flashage pour installer libreboot:

`$ sudo ./flash i945apple_firstflash `[`yourrom.rom`](#rom)

Vous devrez voir notamment le suivant en sortie:

    Vérification du flash.... VÉRIFIÉ.

Éteignez l'appareil.

Copyright © 2014, 2015, 2016 Leah Rowe <info@minifree.org>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
