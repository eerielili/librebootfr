---
title: Modifier la configuration GRUB dans les systèmes Libreboot
x-toc enable: true
...

Ce guide montrera toutes les étapes nécessaires à la modification du fichier
de configuration GRUB dans Libreboot; de telle manière que l'utilisateur n'a
pas à démarrer manuellement son système d'exploitation à chaque fois en tapant
des commandes dans console GRUB.

Pour les buts de ce guide, vous pouvez modifier le fichier de 
configuration GRUB qui est dans la ROM de l'ordinateur ou alors modifier
celle qui existe dans le système d'exploitation même; les deux options
seront expliquées ici.

## Comment récupérer le fichier de configuration GRUB

La première étape du processus est de mettre la main sur le fichier de configuration
GRUB que nous avons besoin de modifier. Il y a deux manières de faire ça:

1. Nous pouvons extraire celle déjà existante dans la ROM.
2. Nous pouvons utiliser une des ROMs pré-compilées fournise par le projet Libreboot.

Cependant, ces deux manières nous amènerons à télécharger l'Archive Utilitaire de Libreboot.

### Télécharger l'Archive Utilitaire Libreboot

L'Archive Utilitaire Libreboot contient les programmes dont nous
aurons besoin pour obtenir notre fichier **grubtest.cfg**. La dernière
version de l'Archive Utilitaire Libreboot peut être téléchargée [depuis libreboot.org](https://www.mirrorservice.org/sites/libreboot.org/release/stable/20160907/libreboot_r20160907_util.tar.xz).
La manière la plus rapide de la télécharger serait d'utiliser le programme `wget`, qui 
(si vous ne savez pas déjà) permet de télécharger des fichiers de l'Internet.

Si vous ne l'avez pas déjà installé, vous pouvez l'installer en utilisant
la commande `apt-get` (sur les distributions basées Debian):

    $ sudo apt-get install wget

Vous pouvez l'installer sur les systèmes basés Arch en utilisant `pacman`:

    $ sudo pacman -S wget

Une fois que vous avez installé `wget`, utilisez le pour télécharger
le fichier en y passant simplement l'URL comme argument; vous pouvez enregistrer
le fichier n'importe où mais pour ce guide, enregistrez-le dans **~/Downloads**
(le dossier Téléchargements de votre dossier personnel/maison/**Home**).
Tout d'abord, changez le répertoire de travail courant sur **~/Downloads**:

    $ cd ~/Downloads

Ce guide assume que vous utilisiez la version **20160907** de Libreboot:
si vous utilisez une version différente, modifiez les commandes suivantes
en accordance :

    $ wget https://www.mirrorservice.org/sites/libreboot.org/release/stable/20160907/\
    >libreboot_r20160907_util.tar.xz

Après que le fichier est téléchargé, utilisez la commande `tar` pour extraire son contenu:

    $ tar -xf libreboot_r20160907_util.tar.xz

Après extraction, le dossier aura le même nom que l'archive: dans ce cas là, 
**libreboot\_r20160907\_util**. Au nom de la simplicité, nous le renommerons **libreboot\_util**,
en utilisant la commande `mv`:

    $ mv "libreboot_r20160907_util" "libreboot_util"

Maintenant vous avez le dossier avec tout les utilitaires nécessaire pour lire et modifier les
contenus de la ROM.

### Obtenir les utilitaires nécessaires
Une fois que vous avez l'archive **libreboot\_util**, vous pouvez trouvez respectivement 
les utilitaires `cbfstool` et `flashrom` dans **libreboot\_util/cbfstools/x86\_64/cbfstool** et
**libreboot\_util/flashrom/x86\_64/flashrom**.

**NOTE: Ce guide assume que vous utilisez un appareil avec l'architecture **x86\_64**;
si vous utilisez un appareil avec une architecture différente (p.e, **i686** ou **armv71**),
la bonne version de **`cbfstool`** et **`flashrom`** seront dans ce dossier,
dans leurs répertoires respectifs.**

Vous pouvez aussi compilez ces deux utilitaires; jetez donc un coup d'oeil à [Comment compiler flashrom](../git/#build_flashrom).

`flashrom` est aussi disponible dans les répertoires de paquets logiciels; si vous utilisez
une distribution basée Arch, utilisez `pacman`:

    $ sudo pacman -S flashrom

Ou si vous avez une distribution basée Debian, utilisez `apt-get`:

    $ sudo apt-get install flashrom

### Obtenir l'image ROM
Vous pouvez soi travailler directement avec une des images ROM déjà inclue
dans les archives de ROMs de Libreboot, ou réutiliser la ROM courante que vous avez
déjà flashée.
Pour ce tutoriel, on va partir du principe que votre image ROM est nommée
**libreboot.rom**, adaptez donc en accordance.

Il y a deux façons d'obtenir une image ROM pré-compilée:

#### 1. Télécharger une image pre-compilée depuis le site web de Libreboot
Pour la version courante, **20160907**, elles peuvent être trouvée [sur un des mirroirs de Libreboot](https://www.mirrorservice.org/sites/libreboot.org/release/stable/20160907/rom/grub/); veuillez silvouplait adapter l'URL si vous utilisez une version différente de Libreboot.

Vous avez aussi besoin de vous assurez que vous sélectionnez à la fois la bonne ROM pour l'appareil
que vous allez utiliser, et la bonne taille flash de la puce (si possible): **4Mo**, **8Mo**, ou **16Mo**;
les tailles flash des puces variantes ne s'appliquent que pour les ThinkPads que Libreboot supportent (en mettant de côté le X60 et T60).

Vous pouvez trouver la taille flash de la puce en exécutant la commande suivante:

    # flashrom -p internal

Recherchez une ligne semblable à celle-ci:

    Found Macronix flash chip "MX25L6406E/MX25L6408E" (8192 kB, SPI) \
    mapped at physical address 0x00000000ff800000.

Exécuter cette commande sur un ThinkPad X200 me donne le résultat ci-dessus, donc je sais
que ma taille flash de puce est de **8Mo**

Une fois que vous avez déterminé la bonne ROM et la taille flash de puce correcte, téléchargez les depuis
le site web.
Puisque j'utilise en ce moment même un ThinkPad X200 pour écrire ce guide, je montrerai comment
télécharger les bonnes images de ROM pour ce modèle.

En premier lieu, naviguons vers le dossier **libreboot\_util**:

    $ cd ~/Downloads/libreboot_util/

Ensuite nous téléchargerons les images de ROM en utilisant `wget`:

    $ wget https://www.mirrorservice.org/sites/libreboot.org/release/stable/\
    >20160907/rom/grub/libreboot_r20160907_grub_x200_8mb.tar.xz

Extrayez l'archive en utilisant `tar`:

    $ tar -xf libreboot_r20160907_grub_x200_8mb.tar.xz

Naviguez dans le répertoire fraichement créé:

    $ cd libreboot_r20160907_grub_x200_8mb

Maintenant que nous sommes dans l'archive, nous devons choisir la bonne image de ROM.
Pour deviner laquelle est la bonne, nous devons d'abord analyser grammaticalement les
noms de fichier de chaque ROM.
Par exemple, pour le fichier nommé **x200_8mb_usqwerty_vesafb.rom**:

    Model Name: x200
    Flash Chip Size: 8mb
    Country: us
    Keyboard Layout: qwerty
    ROM Type: vesafb or txtmode

Puisque j'utilise un clavier QWERTY, j'ignorerai toutes les options non-QWERTY.
Notez bien qu'il y a deux types de ROMs: **vesafb** et **txtmode**;
les images de ROM **vesafb** sont recommandées dans la majorité des cas; les
images de ROM **txtmode** embarque `MemTest86+` qui nécessite le mode texte au lieu du
tampon d'image habituel utilisé par l'initialisation graphique native de coreboot.

Je choisirais **x200_8mb_usqwerty_vesafb.rom**; je copierai le fichier (dans le dossier `cbfstool`)
et le renommerai avec une commande:

    $ mv "x200_8mb_usqwerty_vesafb.rom" ../cbfstool/x86_64/libreboot.rom

#### 2. Créer une image depuis la ROM courante
Une manière plus simple d'obtenir une image ROM est juste de la créer à partir de
votre ROM courante en utilisant flashrom, assurez-vous de l'enregistrer dans le dossier
`cbfstool`, à l'intérieur de **libreboot\_util**:

    $ sudo flashrom -p internal -r ~/Downloads/libreboot_util/cbfstool/x86_64/libreboot.rom

Si on vous dit de spécifier la puce, ajoutez l'option `-c {votre puce} à la commande, comme ceci:

    $ sudo flashrom -c MX25L6405 -p internal -r ~/Downloads/libreboot_util/cbfstool/x86_64/libreboot.rom

Vous êtes maintenant prêt à extraire les fichiers de configuration GRUB de la ROM, et de les modifier de
la façon que vous le voulez.

### Copier grubtest.cfg depuis l'image de la ROM
Vous pouvez jeter un coup d'oeil sur les contennus de l'image ROM, dans CBFS, en utilisant `cbfstool`.
Premièrement, naviguez dans le dossier cbfstool:

    $ cd ~/Downloads/libreboot_util/cbfstool/x86_64/

Ensuite, exécutez la commande `cbfstool` avec l'option print; celà affichera une liste de tout les fichiers
contenus dans la ROM:

    $ ./cbfstool libreboot.rom print

Vous devrez voir **grub.cfg** et **grubtest.cfg** dans la liste. **grub.cfg** est
chargé par défaut avec un menu pour changer sur **grubtest.cfg**. Dans ce tutoriel
vous allez d'abord modifier et tester **grubtest.cfg**. C'est pour réduire la
possibilité de bousiller/briquer votre appareil, donc **NE PASSEZ PAS OUTRE!**

Extrayez **grubtest.cfg** de l'image de la ROM:

    $ ./cbfstool libreboot.rom extract -n grubtest.cfg -f grubtest.cfg

Par défaut `cbfstool` extraiera les fichiers dans le répertoire courant,
donc **grubtest.cfg** devrait apparaitre dans le même dossier que
**libreboot.rom**.

## Comment modifier le fichier de configuration GRUB
Cette partie instruiera l'utilisation sur comment modifier leur fichier de
configuration GRUB; qu'ils décident d'utiliser la version située dans le 
dossier **/** de leur système d'exploitation ou celui situé dans la ROM, les
modifications seront les mêmes.

Une fois que le fichier est ouvert, cherchez la ligne suivante (elle doit être
au alentours de la fin du fichier):

    menuentry 'Load Operating System [o]' --hotkey='o' --unrestricted

Après cette ligne il y aura une accolade ouvrante **{** suivie par quelques lignes
de code, puis ensuite une accolade fermante **}**; supprimez tout ce qui est entre
ces deux accolades et remplacez le par le code suivant si vous utilisez une distribution
basée sur Arch (p.e, Parabola GNU+Linux-Libre):

    cryptomount -a
    set root='lvm/matrix-root'
    linux /boot/vmlinuz-linux-libre root=/dev/matrix/root cryptdevice=/dev/sda1:root \
    cryptkey=rootfs:/etc/mykeyfile
    initrd /boot/initramfs-linux-libre.img

Ou remplacez par ceci si vous utilisez une distribution basée sur Debian (p.e, Trisquel GNU+Linux):

    cryptomount -a
    set root='lvm/matrix-rootvol'
    linux /vmlinuz root=/dev/mapper/matrix-rootvol cryptdevice=/dev/mapper/matrix-rootvol:root
    initrd /initrd.img

Rappelez-vous que ces noms viennent des instructions sur comment installer
GNU+Linux sur des systèmes Libreboot, situés [dans la documentation](index.md).
Si vous avez suivi des instructions différentes (ou pour d'autres raisons, utilisé des
noms différents), mettez simplement les noms de vos volumes **root** et **swap** à la place
de ceux utilisés ici.

Ceci couvre les changements basiques que nous pouvons faire à GRUB; toutefois plus de changements
sont faisables pour améliorer la sécurité de votre configuration GRUB.
Si vous êtes intéressé par ces modifications, référez-vous au guide de Libreboot sur
[endurcir GRUB](grub_hardening.md).

C'est tout pour les modifications ! Maintenant tout ce dont vous avez besoin est
de suivre les instructions ci-dessous afin d'utiliser cette nouvelle configuration
et de démarrer votre système.

## Changer le fichier de configuration GRUB que le système d'exploitation utilise
Maintenant que nous avons expliqué *comment* modifier le fichier en lui-même, nous avons
besoin d'expliquer comment faire pour que notre système *utilise* le nouveau fichier de
configuration GRUB pour démarrer.

### Sans reflasher la ROM
Pour changer la configuration GRUB que notre système utilise sans avoir à reflasher la ROM,
nous avons besoin de prendre notre fichier **grubtest.cfg**, la renommer en **libreboot\_grub**;
c'est parce que par défaut GRUB dans Libreboot est configuré pour scanner toutes les partitions
sur l'espace de stockage principal à la recherche de **/boot/grub/libreboot\_grub.cfg** ou
**/grub/libreboot\_grub.cfg** (pour les systèmes ou **/boot** est sur une partition dédicacée) afin
de l'utiliser automatiquement.

Par conséquent nous avons besoin de copier **libreboot\_grub.cfg** dans **/grub**, ou dans 
**/boot/grub**:

    $ sudo cp ~/Downloads/libreboot_util/cbfstool/x86_64/libreboot_grub.cfg /boot/grub    # ou /grub

Maintenant, la prochaine fois que nous allons démarrer notre ordinateur, GRUB (dans Libreboot) changera
automatiquera sur ce fichier de configuration. *Celà veut dire que vous n'avez pas à reflasher, recompiler 
ou modifier Libreboot de quelque façon que ce soit!*

### En reflashant la ROM
Changer la configuration GRUB qui réside dans la ROM est un peu plus compliqué
que celle dans **/**, mais la majorité du travail est déjà faite.

#### Changer le grubtest.cfg dans la ROM
Maintenant que vous avez le **grubtest.cfg** modifié, nous avons besoin 
d'enlever l'ancien **grubtest.cfg** de la ROM et de mettre notre nouveau.
Pour enlever l'ancien nous utiliserons `cbfstool`:

    $ ./cbfstool libreboot.rom remove -n grubtest.cfg

Ensuite, ajoutez le nouveau à la ROM:

    $ ./cbfstool libreboot.rom add -n grubtest.cfg -f grubtest.cfg -t raw

#### Changer l'adresse MAC dans la ROM {#changeMAC}
La dernière étape avant de flasher la nouvelle ROM est de changer l'adresse MAC
à l'intérieur. Toute image ROM libreboot contient une adresse MAC générique; vous
voulez être sûr que votre image ROM contient la votre pour ne pas créer des problèmes
sur votre réseau (disons par example que de multiples membres de famille ont des ordinateurs
librebootés, et ont utilisé la même image ROM pour flasher ces ordinateurs).

Pour accomplir ceci, nous utiliserons l'utilitaire `ich9gen`, se trouvant aussi dans
**libreboot\_util**.

D'abord, vous avez besoin de trouver l'adresse MAC courante de votre ordinateur; il y 
a deux façons de faire ceci:

1. Lisez l'étiquette blanche à l'arrière de l'ordinateur (toutefois, ça marchera seulement
si votre carte mère n'a jamais été remplacée).
2. Exécutez `ifconfig`; recherchez votre périphérique ethernet (p.e **enpXXX** dans
les distributions basées Arch ou **eth0** dans les distributions basées Debian),
et cherchez pour un ensemble de charactères semblable à ceci: `00:f3:f0:45:91:fe`.

Ensuite, vous avez besoin de déplacer **libreboot.rom** dans le répertoire suivant; c'est
là où l'éxecutable `ich9gen` est situé:

    $ mv libreboot.rom ~/Downloads/libreboot_util/ich9deblob/x86_64

Une fois là, éxecutez la commande suivante en vous assurant bien d'utiliser votre 
propre adresse MAC au lieu de ce qui est écrit ci-dessous:

    $ ./ich9gen --macaddress XX:XX:XX:XX:XX:XX

Trois nouveaux fichiers seront créés:

*     **ich9fdgbe_4m.bin**: c'est pour les ordinateurs portables GM45 avec la puce flash de 4Mo
*     **ich9fdgbe_4m.bin**: c'est pour les ordinateurs portables GM45 avec la puce flash de 8Mo
*     **ich9fdgbe_4m.bin**: c'est pour les ordinateurs portables GM45 avec la puce flash de 16Mo

Cherchez celle qui correspond à la taille de votre image de ROM; par example,
si votre taille flash de puce est de 8Mo, vous utiliseriez **ich9fdgbe_8m.bin**.

Maintenant, insérez ce fichier (appelé le `descriptor+gbe`) dans l'image ROM en
utilisant `dd`:

    $ dd if=ich9fdgbe_8m.bin of=libreboot.rom bs=1 count=12k conv=notrunc

Déplacez **libreboot.rom** de nouveau dans le répertoire **libreboot\_util**:

    $ mv libreboot.rom ~/Downloads/libreboot_util

Vous êtes finalement prêt à flasher la ROM!

#### Flasher l'image ROM mise à jour
La dernière étape du flashage de la ROM nous demande de changer le répertoire de 
travail courant sur **libreboot\_util**

    $ cd ~/Downloads/libreboot_util

Maintenant tout ce que nous avons à faire est d'utiliser le script `flash` dans
ce répertoire avec l'option `update`, en utilisant **libreboot.rom** comme argument:

    $ sudo ./flash update libreboot.rom

Occasionnellement, coreboot change le nom d'une carte mère donnée. Si `flashrom`
se plaint d'une disparité de carte mère mais que vous êtes sûr d'avoir choisi la
bonne image ROM, alors éxecutez cette commande alternative:

    $ sudo ./flash forceupdate libreboot.rom

Vous allez voir le programme `flashrom` s'éxecuter pendant un petit moment, et vous verrez
peut-être des erreurs mais si ça dit `Verifying flash... VERIFIED` à la fin, alors c'est flashé
et ça devrait démarrer. Si vous voyez des erreurs, réessayez (encore et encore).
Le message, `Chip content is identical to the requested image` est aussi une indication d'une
installation réussie.

#### Redémarrer l'ordinateur
Maintenant que vous avez flashé l'image, redémarrez l'ordinateur. Tapotez continuellement
la barre espace dès le démarrage jusqu'à que vous voyez le menu GRUB, afin d'empêcher
libreboot d'essayer de charger votre système d'exploitation automatiquement.

Défilez vers le bas avec les touches directionnelles et choisissez l'option
`Load test configuration (grubtest.cfg) inside of CBFS`; ça changera la configuration GRUB
sur votre version de test. Si tout se passe bien, ça devrait vous demander un nom d'utilisateur
et mot de passe GRUB puis ensuite votre mot de passe LUKS.

Une fois que le système d'exploitation commence à se charger, ça vous demandera une nouvelle fois
votre mot de passe LUKS. Si ça continue et charge le système d'exploitation sans erreurs alors celà
veut dire que votre tentative de flashage était une réussite.

#### Étapes finales
Quand vous êtes satisfait du démarrage depuis **grubtest.cfg**, vous pouvez créer une 
copie de **grubtest.cfg**, appelée **grub.cfg**.

Premièrement, allez dans le répertoire `cbfstool`:

    $ cd ~/Downloads/libreboot_util/cbfstool/x86_64/

Puis créez une copie de **grubtest.cfg** nommée **grub.cfg**:

    $ cp grubtest.cfg ./grub.cfg

Maintenant vous utiliserez la commande `sed` pour faire quelque changement dans le fichier:
l'entrée de menu `Switch to grub.cfg` sera changée en `Switch to grubtest.cfg`,
et à l'intérieur de celle-ci, toutes les instances de **grub.cfg** en **grubtest.cfg**.
Comme ça la configuration principale fait encore le lien (dans le menu) à **grubtest.cfg**,
de ce fait vous n'avez pas à basculer manuellement dessus dans le cas où vous voudriez
resuivre ce guide dans le future (modifiant la config déjà modifiée): 

    $ sed -e 's:(cbfsdisk)/grub.cfg:(cbfsdisk)/grubtest.cfg:g' -e \
    >'s:Switch to grub.cfg:Switch to grubtest.cfg:g' < grubtest.cfg > \
    >grub.cfg

Déplacez **libreboot.rom** depuis **libreboot\_util** dans votre répertoire courant:

    $ mv ~/Downloads/libreboot_util/libreboot.rom .

Supprimez le **grub.cfg** qui est déjà à l'intérieur de la ROM:

    $ ./cbfstool libreboot.rom remove -n grub.cfg

Ajoutez votre **grub.cfg** modifié à la ROM:

    $ ./cbfstool libreboot.rom add -n grub.cfg -f grub.cfg -t raw

Déplacez de nouveau **libreboot.rom** dans **libreboot\_util**:

    $ mv libreboot.rom ../..

Si vous ne vous souvenez pas comment la flasher, référez-vous à *Flasher l'image ROM mise à jour* au dessus; c'est la même méthode que vous avez utilisé avant.
Par la suite, redémarrez votre machine dotée de la nouvelle configuration.

Copyright © 2014, 2015, 2016 Leah Rowe <info@minifree.org>

Copyright © 2017 Elijah Smith <esmith1412@posteo.net>

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
