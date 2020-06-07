--- 
title: Instructions pour la compilation du code source
x-toc-enable: true
...

Depthcharge est actuellement non documenté depuis qu'il est dans le nouveau
système de construction.  Les instructions sur comment compiler des cartes
mères qui ont depthcharg sont incluses dans le fichier BUILD\_HOWTO sur
libreboot.git ou \_src.

Cette section concerne la construction de libreboot depuis la source, et
travailler avec le répertoire git.


Installer les dépendances pour la compilation
==========================

Afin de faire quoi que soit, vous avez besoin des dépendances en premier.
C'est vrai si vous voulez compiler libreboot depuis la source, que ce soit
avec libreboot\_src.tar.xz ou git. *Si vous utilisez libreboot\_util.tar.xz
(archive binaire) alors vous pouvez ignorer celà, parce que les images ROM et
les exécutables compilés statiquement pour les utilitaires sont inclus.*

Pour Debian Stretch (pourrait aussi marcher avec Debian Jessie), vous pouvez
exécuter la commande suivante:

    $ sudo ./oldbuild dependencies debian

(celà marchera aussi dans Devuan)

Pour Parabola, vous pouvez exécuter la commande suivante:

   $ sudo ./oldbuild dependencies parabola\

ou:

   # ./oldbuild dependencies parabola

Pour les autres distributions GNU+Linux, vous pouvez adapter les scripts
existants.


Récupérer le code source complet depuis les métadonnées (git clon)
==================================================

Si vous avez téléchargé Libreboot depuis git, alors il y a quelques étapes
pour télécharger et patcher le code source pour toutes les dépendances
importantes. L'archive dans le répertoire git était disponible en tant qu'une
tarball nommée 'libreboot\_meta.tar.gz'.  Elle contient des 'métadonnées'
(scripts) qui définissent comment la source a été créée (d'où est-ce qu'elle
est venue).

Vous pouvez utiliser les scripts inclus pour tout télécharger.

En premier, [installer les dépendances de
constructions](#dependances_de_build).

Depuis que libreboot fait une utilisation extensive, vous avez besoin de
configurer git correctement.  Si vous n'avez pas encore configuré git, alors
les exigences minimales sont:

    $ git config --global user.name "Your name" $ git config --global
    user.email your@emailadress.com

C'est ce qui apparaitra aussi dans les logs de git si jamais vous commitez vos
propres changements dans un répertoire donné . Pour plus d'informations,
regardez <http://git-scm.com/doc>

Une autre bonne config pour vous (optionnelle, mais recommandée) :

    $ git config --global core.editor nano $ git config --global color.status
    auto $ git config --global color.branch auto $ git config --global
    color.interactive auto $ git config --global color.diff

Après cela, exécutez le script:

    $ ./download all

Ce que cela a fait est de tout télécharger (grub, coreboot, memtest86+, bucts,
flashrom) des versions dernièrement testé pour cette publication, et les
patchs. Lisez le script dans un éditeur de texte pour en apprendre plus.

Pour compiler les images ROM, jetez un coup d'oeil à la section [\#build](#build)


Comment compiler des "bucts" (pour LenovoBIOS X60/X60S/X60T/T60)
=========================================================

*Ceci est pour les utilisateurs du BIOS de Lenovo sur le ThinkPad X60/X60S,
Tablette X60 et T60. Si vous exécutez déjà coreboot et libreboot, ignorez
cela*

BUC.TS n'est pas réellement spécifique à ces ordinateurs portables,  mais
c'est un bit dans le registre dans le jeu de puces (chipset) sur certains
systèmes Intel.

Bucts est nécessaire lors du flashage de logiciel sur la X60/X60S/X60T/T60 ROM
pendant que le BIOS Lenovo s'exécute; hors de ça le flashage externe sera sans
danger

Chaque ROM contient des données identiques à l'intérieur des deux régions
finales de 64K dans le fichier \*. Cela correspond aux deux regions finales de
64K dans la puce flash.  Le BIOS de Lenovo vous empêchera d'écrire la dernière
région, donc exécuter `bucts 1` paramètrera le système pour démarrer sur
l'autre bloc (qui est écrivable ainsi tout en dessous quand vous utilisez une
flashrom patchée. Regardez [\#build\_flashrom](#build_flashrom )).  Après
l'arrêt et le démarrage après le premier flash de lireboot, le bloc final de
64K est écrivable donc vous pouvez encore flasher la ROM avec une flashrom non
patchée et exécuter `butcs 0` pour faire encore démarrer le système du bloc
normal (le plus haut).


\*Les images de ROM Libreboot ont des données identiques dans ces deux régions
de 64Ko car dd est utilisé pour faire ça par l'intermédiaire du système de
construction.  Si vous contruisez depuis l'amont (coreboot), vous devez le
faire manuellement.

BUC.TS est alimenté par la batterie NVRAM (ou la batterie CMOS, comme
certaines personnes l'appelent).  Sur les thinkpads, c'est typiquement dans un
paquet en plastique jaune avec la batterie à l'intérieur, connecté via les
lignes électriques à la carte mère.  Enlever cette batterie enlève
l'alimentation au BUC.TS, réinitialisant le bit à 0 (si vous l'avez mis
précédemment à 1).

L'utilitaire BUC.TS est inclus dans libreboot\_src.tar.xz et
libreboot\_util.tar.xz.

Si vous avez téléchargé depuis git, suivez la [\#build\_meta](#build_meta)
avant de procéder.

*BUC* signifie "*B*ack*u*p *C*ontrol" (c'est un registre) et "TS" signifie
*T*op *S*wap" (c'est un bit de status).  D'où le nom "bucts" (BUC.TS). TS 1 et
TS 0 correspondent à bucts 1 et bucts 0.


If you downloaded from git, follow [\#build\_meta](#build_meta) before you
proceed.

"BUC" means "*B*ack*u*p *C*ontrol" (it's a register) and "TS" means "*T*op
*S*wap" (it's a status bit). Hence "bucts" (BUC.TS). TS 1 and TS 0 corresponds
to bucts 1 and bucts 0.

Si vous avez l'archive de publications des binaires, vous trouverez des
exécutables en dessous ./bucts/. Sinon si vous avez besoin de compiler
depuis la source, continuez à lire.

Premièrement, [installez les dépendances de
constructions](#build_dependencies).

Pour compiler les bucts, faîtes ceci dans le répertoire principal:

    $ ./oldbuild module bucts

Pour le compiler statiquement, faîtes ceci:

    $ ./oldbuild module bucts static

Le script "builddeps" dans libreboot\_src fait aussi l'utlisation de
builddeps-bucts.

Comment compiler "flashrom"
=========================

Flashrom est l'utilitaire pour flasher ou faire un cliché mémoire des images
ROM. C'est ce que vous utiliserez pour installer Libreboot.

Le code source de flashrom est inclut dans libreboot\_src.tar.xz et
libreboot\_util.tar.xz.

*Si vous l'avez téléchargé depuis git, suivez la section
[\#build\_meta](#build_meta) avant de continuer.*

Si vous utilisez l'archive des publications des binaires, alors il y a déjà
des binaires inclus sous ./flashrom/. Les scripts de flashage essaieront de
choisir le bon pour vous. Sinon, si vous souhaitez re-compiler flashrom depuis
la source, continuez à lire.

Premiérement, [installez les dépendances de compilation](#build_dependencies)

Pour le compiler, exécutez le suivant dans le répertoire principal:

    $ ./oldbuild module flashrom

Pour le compiler statiquement, exécutez le suivant dans le répertoire
principal:

    $ ./oldbuild module flashrom static

Après que vous avez fait ceci, vous trouverez les exécutables suivants dans le
répertoire ./flashrom/ :

-   `flashrom`
    -   Pour le flashage pendant que coreboot ou libreboot est en cours
        d'exécution.
-   `flashrom_lenovobios_sst`
    -   Il est patché pour le flashage pendant que le BIOS Lenovo est en cours
        d'exécution sur un X60 ou T60 avec la puce flash SST25VF016B (SST).
-   `flashrom_lenovobios_macronix`
    -   Il est patché pour le flashage pendant que le BIOS Lenovo est en cours
        d'exécution sur un X60 ou T60 avec la puce flash SMX25L1605D (Macronix).

Le script "builddeps" dans libreboot\_src se sert aussi du script builddeps-flashrom.

Comment compiler les images ROM
===========================

Vous n'avez pas besoin de faire grand chose, puisqu'il y a des scripts déjà
écrit pour vous qui peuvent compiler tout automatiquement.

Vous pouvez compiler libreboot depuis la source sur un système à architecture
32bit (i686) ou 64bit (x86\_64).
Recommandé (si possible): x86\_64. La carte mère ASUS KFSN4-DRE a des
processeurs 64-bit, par exemple.
Sur un ThinkPad T60, vous pouvez remplacer le processeur (Core 2 Duo T5600, T7200 or T7600.
T5600 recommended) pour avoir un support 64bit. 
Sur un X60s, vous pouvez remplacer la carte avec une qui a un Core 2 Duo L7400
(vous pourrez aussi utiliser une carte mère d'un X60 Tablet avec le même
processeur). Sur un X60, vous pouvez remplacez la carte mère avec une qui a un
Core 2 Duo T5600 (recommandé) ou T7200. Tout les ordinateurs portables
Macbook2,1 sont d'architecture 64-bit, comme tout les ordinateurs portables
ThinkPad X200, X200S, X200 Tablet, R400, T400 et T500.
Avertissement: tout les ordinateurs portables Macbook1,1 sont 32bit seulement.

Premièrement, [installez les dépendances de compilation](#build_dependencies)

Si vous avez téléchargé libreboot depuis git, référez-vous à
la section [\#build\_meta](#build_meta).

Compilez tout les composants utilisés dans Libreboot:

    $ ./oldbuild module all

Vous pouvez aussi compiler chaque modules séparément, en utilisant *./oldbuild
module nom_du_module*. Pour les valeurs possibles pour *nom_du_module*,
utilisez:

    $ ./oldbuild module list

Après ça, compilez les images ROM (pour toutes les cartes):

    $ ./oldbuild roms withgrub

Alternativement, vous pouvez compiler pour une carte mère spécifique ou un
ensemble de cartes. Par exemple:

    $ ./oldbuild roms withgrub x60 $ ./oldbuild roms withgrub x200_8mb $
    ./oldbuild roms withgrub x60 x200_8mb

La liste des options de carte mère peut être trouvé en regardant les noms de
répertoire dans `resources/libreboot/config/grub/`.

Pour tout (inverser) nettoyer, faites le suivant:

    $ ./oldbuild clean all

Les images ROM seront stockées sous `bin/payload`, où `payload` pourrait être
`grub`, `seabios`, ou n'importe quel autre charge utile pour lesquelle les
images ont été compilés pour.

Préparer l'archive des publications/versions (optionnel)
-------------------------------------

*C'est seulement confirmé être fonctionnel (testé) sur Debian Stretch. Ça
marchera également sur Devuan. Parabola échoue à cette étape (pour l'instant).
Pour tout autre distribution, votre expérience peut varier.*

C'est principalement prévu pour l'utilisation avec le répertoire git. Ces
commandes marcheront dans l'archive des publications (\_src), à moins qu'autre
chose soit noté ci-dessous.

Les archives apparaitront sous le répertoire
*release/oldbuildsystem/\${version}/*; \${version} sera soit défini en
utilisant *git describe* ou, si un fichier *version* existe déjà (archive des
versions \_src), alors ça le réutilisera tout simplement.

Marquez (*tag*) le commit en cours, et cette version apparaîtra à la fois dans
la chaîne de caractère \${version} sur le nom du répertoire dans
*release/oldbuildsystem/*, et dans les noms de fichier des archives. Sinon,
n'importe quoi que git utilise pour *git describe --tags HEAD* sera utilisé.

Utilitaires (exécutables statiques):

    $ ./oldbuild release util

Archive contenant le code source de flashrom et bucts:

    $ ./oldbuild release tobuild

Archive de la documentation (*ne marchera pas sur l'archive des versions
\_src, seulement git*):

    $ ./oldbuild release docs

Archives des images ROM

    $ ./oldbuild release roms

Archive du code source:

    $ ./oldbuild release src

Sommes de contrôle SHA512 de toutes les autres archives de versions qui ont
été générée:

    $ ./oldbuild release sha512sums

Si vous compilez sur une hôte à architecture i686, ça compilera statiquement
des binaires 32bit dans l'archive des versions des binaires que vous avez
créé, pour:

    nvramtool, cbfstool, ich9deblob, cbmem

Si vous compilez sur un hôte à architecture x86\_64, ça compilera statiquement
des binaires 32bit \*et\* 64bit pour `cbmem`, `ich9deblob`, `cbfstool` and
`nvramtool`.

*Pour inclure des binaires d'architecture i686 et x86\_64 liés statiquement pour bucts
et flashrom, vous aurez besoin de les compiler dans une chroot, un système
virtuel ou un vrai système où l’hôte utilise chaque architecture donnée. Ces
paquets sont difficile à compiler de façon croisée, et le projet libreboot est
toujours en train de chercher comment les gérer.*

La même s'applique si vous voulez inclure des binaires de flashrom
statiquements liés pour l'architecture ARM.

Les binaires d'arch. armv7l (testés sur un BeagleBone Black) sont aussi inclus
dans libreboot\_util, pour:

-   cbfstool
-   ich9gen
-   ich9deblob
-   flashrom

Si vous compilez les binaires sur un système en direct (live) ou une chroot
(pour flashrom/bucts) vous pouvez utiliser le suivant pour les lier
statiquement:

    $ ./oldbuild module flashrom static $ ./oldbuild module bucts static

Les mêmes conditions que ci-dessus s'appliquent pour ARM (excepté, compiler
bucts sur ARM est inutile, et pour flashrom vous avez seulement besoin de
l'exécutable normal, puisque les exécutables lenovobios\_sst et \_macronix
sont conçus pour être exécuté sur un X60/T60 pendant que le BIOS Lenovo est
présent, contournant les restrictions de sécurité).

La commande que vous avez utilisé pour générer l'archives des versions
exécutera aussi la commande suivante:

    $ ./oldbuild release tobuild

L'archive `tobuild.tar.xz` aura été créé sous `release/oldbuildsystem/`,
contenant bucts, flashrom et toutes les autres ressources requises pour les
compiler.

Vous trouvez que les fichiers libreboot\_util.tar.xz et libreboot\_src.tar.xz
ont été créé, sous le répertoire `release/oldbuildsystem/`.

Les images ROM seront créés dans des archives séparées pour chaque système,
dans le répertoire  `release/oldbuildsystem/rom/`.

Copyright © 2014, 2015, 2016 Leah Rowe <info@minifree.org>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
