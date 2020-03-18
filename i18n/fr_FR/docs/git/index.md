---
title: Instructions pour la compilation du code source
x-toc-enable: true
...

Depthcharge est actuellement non documenté depuis qu'il est dans le nouveau système de construction.
Les instructions sur comment construire des cartes mères qui ont depthcharg sont incluses dans le fichier BUILD\_HOWTO sur libreboot.git ou \_src.

Cette section concerne la construction de libreboot depuis la source, et travailler avec le répertoire git.


Installer les dépendances de construction
==========================

Afin de faire quoi que soit, vous avez besoin des dépendances en premier. C'est vrai si vous voulez construire libreboot depuis la source, que ce soit avec libreboot\_src.tar.xz ou git. *Si vous utilisez libreboot\_util.tar.xz (archive binaire) alors vous pouvez ignorer celà, parce que les images ROM et les exécutables compilés statiquement pour les utilitaires sont inclus.*

Pour Debian Stretch (pourrait aussi marcher avec Debian Jessie), vous pouvez exécuter la commande suivante:

    $ sudo ./oldbuild dependencies debian

(celà marchera aussi dans Devuan)

Pour Parabola, vous pouvez exécuter la commande suivante:

   $ sudo ./oldbuild dependencies parabola\

ou:

   # ./oldbuild dependencies parabola

Pour les autres distributions GNU+Linux, vous pouvez adapter les scripts existants.


Récupérer le code source complet depuis les métadonnées (git clon)
==================================================

Si vous avez téléchargé Libreboot depuis git, alors il y a quelques étapes pour télécharger et patcher le code source pour toutes les dépendances importantes. L'archive dans le répertoire git était disponible en tant qu'une tarball nommée 'libreboot\_meta.tar.gz'.
Elle contient des 'métadonnées' (scripts) qui définissent comment la source a été créée (d'où est-ce qu'elle est venue).

Vous pouvez utiliser les scripts inclus pour tout télécharger.

En premier, [installer les dépendances de constructions](#dependances_de_build).

Depuis que libreboot fait une utilisation extensive, vous avez besoin de configurer git correctement.
Si vous n'avez pas encore configuré git, alors les exigences minimales sont:

    $ git config --global user.name "Your name"
    $ git config --global user.email your@emailadress.com

C'est ce qui apparaitra aussi dans les logs de git si jamais vous commitez vos propres changements dans un répertoire donné . Pour plus d'informations, regardez <http://git-scm.com/doc>

Une autre bonne config pour vous (optionnelle, mais recommandée) :

    $ git config --global core.editor nano
    $ git config --global color.status auto
    $ git config --global color.branch auto
    $ git config --global color.interactive auto
    $ git config --global color.diff

Après cela, exécutez le script:

    $ ./download all

Ce que cela a fait est de tout télécharger (grub, coreboot, memtest86+, bucts, flashrom) des versions dernièrement testé pour cette publication, et les patchs. Lisez le script dans un éditeur de texte pour en apprendre plus.

Pour construire les images ROM, regardez [\#build](#build)


Comment construire des "bucts" (pour LenovoBIOS X60/X60S/X60T/T60)
=========================================================

*Ceci est pour les utilisateurs du BIOS de Lenovo sur le ThinkPad X60/X60S, Tablette X60 et T60. Si vous exécutez déjà coreboot et libreboot, ignorez cela*

BUC.TS n'est pas réellement spécifique à ces ordinateurs portables,  mais c'est un bit dans le registre dans le jeu de puces (chipset) sur certains systèmes Intel.

Bucts est nécessaire lors du flashage de logiciel sur la X60/X60S/X60T/T60 ROM pendant que le BIOS Lenovo s'exécute; hors de ça le flashage externe sera sans danger

Chaque ROM contient des données identiques à l'intérieur des deux régions finales de 64K dans le fichier \*. Cela correspond aux deux regions finales de 64K dans la puce flash.
Le BIOS de Lenovo vous empêchera d'écrire la dernière région, donc exécuter `bucts 1` paramètrera le système pour démarrer sur l'autre bloc (qui est écrivable ainsi tout en dessous quand vous utilisez une flashrom patchée. Regardez [\#build\_flashrom](#build_flashrom )).
Après l'arrêt et le démarrage après le premier flash de lireboot, le bloc final de 64K est écrivable donc vous pouvez encore flasher la ROM avec une flashrom non patchée et exécuter `butcs 0` pour faire encore démarrer le système du bloc normal (le plus haut).


\*Les images de ROM Libreboot ont des données identiques dans ces deux régions de 64Ko car dd est utilisé pour faire ça par l'intermédiaire du système de construction.
Si vous contruisez depuis l'amont (coreboot), vous devez le faire manuellement.

BUC.TS est soutenu (alimenté) par la batterie NVRAM (ou la batterie CMOS, comme certaines personnes l'appelent).
Sur les thinkpads, c'est typiquement dans un paquet en plastique jaune avec la batterie à l'intérieur, connecté via les lignes électriques à la carte mère.
Enlever cette batterie enlève l'alimentation au BUC.TS, réinitialisant le bit à 0 (si vous l'avez mis précédemment à 1).

L'utilitaire BUC.TS est inclus dans libreboot\_src.tar.xz et libreboot\_util.tar.xz.

Si vous avez téléchargé depuis git, suivez la [\#build\_meta](#build_meta) avant de procéder.

*BUC* signifie "*B*ack*u*p *C*ontrol" (c'est un registre) et "TS" signifie *T*op *S*wap" (c'est un bit de status).
D'où le nom "bucts" (BUC.TS). TS 1 et TS 0 correspondent à bucts 1 et bucts 0.


If you downloaded from git, follow [\#build\_meta](#build_meta) before
you proceed.

"BUC" means "*B*ack*u*p *C*ontrol" (it's a register) and
"TS" means "*T*op *S*wap" (it's a status bit). Hence "bucts"
(BUC.TS). TS 1 and TS 0 corresponds to bucts 1 and bucts 0.

Si vous avez l'archive de publications des binaires, vous trouverez des exécutables en dessous ./bucts/. Sinon si vous avez besoin de construire depuis la source, continuez à lire.

Premièrement, [installez les dépendances de constructions](#build_dependencies).

Pour construire les ducts, faîtes ceci dans le répertoire principal:

    $ ./oldbuild module bucts

Pour le compiler statiquement, faîtes ceci:

    $ ./oldbuild module bucts static

Le script "builddeps" dans libreboot\_src fait aussi l'utlisation de builddeps-bucts.


How to build "flashrom" 
=========================

Flashrom is the utility for flashing/dumping ROM images. This is what
you will use to install libreboot.

Flashrom source code is included in libreboot\_src.tar.xz and
libreboot\_util.tar.xz.

*If you downloaded from git, follow [\#build\_meta](#build_meta) before
you proceed.*

If you are using the binary release archive, then there are already
binaries included under ./flashrom/. The flashing scripts will try to
choose the correct one for you. Otherwise if you wish to re-build
flashrom from source, continue reading.

First, [install the build dependencies](#build_dependencies).

To build it, do the following in the main directory:

    $ ./oldbuild module flashrom

To statically compile it, do the following in the main directory:

    $ ./oldbuild module flashrom static

After you've done that, under ./flashrom/ you will find the following
executables:

-   `flashrom`
    -   For flashing while coreboot or libreboot is running.
-   `flashrom_lenovobios_sst`
    -   This is patched for flashing while Lenovo BIOS is running on an
        X60 or T60 with the SST25VF016B (SST) flash chip.
-   `flashrom_lenovobios_macronix`
    -   This is patched for flashing while Lenovo BIOS is running on an
        X60 or T60 with the MX25L1605D (Macronix) flash chip.

The "builddeps" script in libreboot\_src also makes use of
builddeps-flashrom.

How to build the ROM images 
===========================

You don't need to do much, as there are scripts already written for you
that can build everything automatically.

You can build libreboot from source on a 32-bit (i686) or 64-bit
(x86\_64) system. Recommended (if possible): x86\_64. ASUS KFSN4-DRE has
64-bit CPUs. On a ThinkPad T60, you can replace the CPU (Core 2 Duo
T5600, T7200 or T7600. T5600 recommended) for 64-bit support. On an
X60s, you can replace the board with one that has a Core 2 Duo L7400
(you could also use an X60 Tablet board with the same CPU). On an X60,
you can replace the board with one that has a Core 2 Duo T5600 or T7200
(T5600 is recommended). All MacBook2,1 laptops are 64-bit, as are all
ThinkPad X200, X200S, X200 Tablet, R400, T400 and T500 laptops. Warning:
MacBook1,1 laptops are all 32-bit only.

First, [install the build dependencies](#build_dependencies).

If you downloaded libreboot from git, refer to
[\#build\_meta](#build_meta).

Build all of the components used in libreboot:

    $ ./oldbuild module all

You can also build each modules separately, using *./oldbuild module
modulename*. To see the possible values for *modulename*, use:

    $ ./oldbuild module list

After that, build the ROM images (for all boards):

    $ ./oldbuild roms withgrub

Alternatively, you can build for a specific board or set of boards. For
example:

    $ ./oldbuild roms withgrub x60
    $ ./oldbuild roms withgrub x200_8mb
    $ ./oldbuild roms withgrub x60 x200_8mb

The list of board options can be found by looking at the directory names
in `resources/libreboot/config/grub/`.

To clean (reverse) everything, do the following:

    $ ./oldbuild clean all

The ROM images will be stored under `bin/payload/`, where `payload`
could be `grub`, `seabios`, or whatever other payload those images were
built for.

Preparing release archives (optional)
-------------------------------------

*This is only confirmed to work (tested) in Debian Stretch. Parabola fails at
this stage (for now). For all other distros, YMMV. This will also work in
Devuan.*

This is mainly intended for use with the git repository. These commands
will work in the release archive (\_src), unless otherwise noted below.

The archives will appear under *release/oldbuildsystem/\${version}/*;
\${version} will either be set using *git describe* or, if a *version*
file already exists (\_src release archive), then it will simply re-use
that.

Tag the current commit, and that version will appear in both the
\${version} string on the directory under *release/oldbuildsystem/*, and
in the file names of the archives. Otherwise, whatever git uses for *git
describe --tags HEAD* will be used.

Utilities (static executables):

    $ ./oldbuild release util

Archive containing flashrom and bucts source code:

    $ ./oldbuild release tobuild

Documentation archive (*does not work on \_src release archive, only
git*):

    $ ./oldbuild release docs

ROM image archives:

    $ ./oldbuild release roms

Source code archive:

    $ ./oldbuild release src

SHA512 sums of all other release archives that have been generated:

    $ ./oldbuild release sha512sums

If you are building on an i686 host, this will build statically linked
32-bit binaries in the binary release archive that you created, for:

    nvramtool, cbfstool, ich9deblob, cbmem

If you are building on an x86\_64 host, this will build statically
linked 32- \*and\* 64-bit binaries for `cbmem`, `ich9deblob`,
`cbfstool` and `nvramtool`.

*To include statically linked i686 and x86\_64 binaries for bucts and
flashrom, you will need to build them on a chroot, a virtual system or a
real system where the host uses each given architecture. These packages
are difficult to cross-compile, and the libreboot project is still
figuring out how to deal with them.*

The same applies if you want to include statically linked flashrom
binaries for ARM.

armv7l binaries (tested on a BeagleBone Black) are also included in
libreboot\_util, for:

-   cbfstool
-   ich9gen
-   ich9deblob
-   flashrom

If you are building binaries on a live system or chroot (for
flashrom/bucts), you can use the following to statically link them:

    $ ./oldbuild module flashrom static
    $ ./oldbuild module bucts static

The same conditions as above apply for ARM (except, building bucts on
ARM is pointless, and for flashrom you only need the normal executable
since the lenovobios\_sst and \_macronix executables are meant to run on
an X60/T60 while lenovo bios is present, working around the security
restrictions).

The command that you used for generating the release archives will also
run the following command:

    $ ./oldbuild release tobuild

The archive `tobuild.tar.xz` will have been created under
`release/oldbuildsystem/`, containing bucts, flashrom and all other
required resources for building them.

You'll find that the files libreboot\_util.tar.xz and
libreboot\_src.tar.xz have been created, under
`release/oldbuildsystem/`.

The ROM images will be stored in separate archives for each system,
under `release/oldbuildsystem/rom/`.

Copyright © 2014, 2015, 2016 Leah Rowe <info@minifree.org>\

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License Version 1.3 or any later
version published by the Free Software Foundation
with no Invariant Sections, no Front Cover Texts, and no Back Cover Texts.
A copy of this license is found in [../fdl-1.3.md](../fdl-1.3.md)
