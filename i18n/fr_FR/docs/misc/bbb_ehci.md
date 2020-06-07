---
titre: Déboguage EHCI sur le BeagleBone Black
...

Déboguage EHCI
==============

1.  [Trouver un port USB sur la cible qui supporte le déboguage EHCI](#FindUSBportonthetargetthatsupportsEHCIdebug)

2.  [Configuration initiale du BBB pour agir en tant que dongle de déboguage
EHCI](#InitialsetupofBBBtoactasEHCIdebugdongle)

3.  [Patcher le module `g_dbgp` du BBB (optionnel, mais hautement recommandé)](#PatchBBBsgdbgpmoduleoptionalbuthighlyrecommended)

4.  [Configurer libreboot pour le déboguage EHCI]((#ConfigurelibrebootwithEHCIdebug)
    1.  [Sélectionner `HCD Index` et `USB Debug port`](#SelectingHCDIndexandUSBDebugport)
5.  [Comment obtenir les journaux de déboguage](#Howtogetthedebuglogs)
6.  [Activer le déboguage EHCI sur le kernel cible (optionnel, recommandé)](#EnebleEHCIDebugonthetargetskerneloptionalrecommended)
7.  [Réferences](#References)

*NOTE: cette documentation peut être périmé, et discute sur la configuration
du déboguage EHCI sur le système Debian de base que le BBB est parfois fournit
avec. Si vous voulez vous faciliter la taĉhe, utilisez juste le [tournevis
BBB](https://www.coreboot.org/BBB_screwdriver) qui est fournit pré-configuré.*

Si votre ordinateur ne démarre pas après avoir installé libreboot, il est très
utile d'obtenir ses journaux de debug, de la charge utile (grub) et/ou le
kernel (si ça arrive jusqu'à là). Tous diffusent leurs journaux de déboguage
sur le port série disponible (RS-232) par défaut. Cependant, la majorité des
ordinateurs portables d'aujourd'hui manquent d'un port RS-232.
L'autre option est de diffuser les journaux au port USB EHCI de déboguage.

Cette section explique étapes par étapes comment configurer le BBB comme
"Dongle de déboguage USB EHCI" et configure libreboot et le kernel linux pour
qu'ils diffusent les journaux dessus (TODO: grub).

Je ferais références à trois ordinateurs:

-   *hôte* - c'est l'ordinateur que vous utilisez, qui a des outils,
    compilateurs, Internet, etc
-   *BBB* - Beaglebone Black (rév. B ou plus haut, j'utilise la révision C.)
-   *cible* - l'ordinateur où on essaye d'installer libreboot

### Trouver le port USB sur la cible qui supporte le déboguage EHCI {#FindUSBportonthetargetthatsupportsEHCIdebug}

Pas tous les contrôleurs USB supportent le déboguage EHCI (voyez: [Port de
déboguage EHCI](http://www.coreboot.org/EHCI_Debug_Port#Hardware_capability) ).
Même, de plus, si un contrôleur USB supporte le déboguage EHCI, il est
disponible seulement *sur un port* qui peut ou ne peut pas être exposé à
l'extérieur.

-   Vous avez besoin d'un système d'exploitation (GNU+Linux) en cours
    d'exécution sur
    votre cible pour cet étape (si vous avez flashé libreboot et ça ne démarre
    pas, vous avez à flasher le bios d'usine).
-   Vous avez besoin d'une clé USB mémoire (les données sur celle-ci ne seront
    pas touchées).
-   Le déboguage EHCI ne peut pas être fait depuis un concentrateur (*hub*) USB
    externe, le BBB doit être connecté directement au port de déboguage du
    contrôleur (donc, pas de concentrateurs).

-   Téléchargez^[1](#___fn1)^
    [ce](http://www.coreboot.org/pipermail/coreboot/attachments/20080909/ae11c291/attachment.sh)
    script shell.

1.  Branchez la clé USB sur le premier port USB disponible.
2.  Exécutez le script, vous obtiendrez une sortie similaire au suivant:
3.  The buses the support debug are Bus 3 (0000:00:1a.0) on Port 1 and
    Bus 4 (0000:00:1d.0) on port 2. Your usb stick is plugged on Bus 1,
    Port 3
    (fr: Les bus supportant le déboguage sont le Bus 3 (0000:00:1a.0) sur le
    Port 1, et Bus 4 (0000:00:1d.0) sur le port 2. Votre clé USB est branchée
    sur le Bus 1, Port 3.
4.  Répétez les étapes, en branchant la clé sur le port USB disponible
suivant.
5.  Faites ça pour tout les ports disponibles et souvenez-vous (mettez sur
papier) de ceux dont le bus/port de la clé USB correspondent aux bus/port qui
support le déboguage (en gras).

Souvenez-vous (mettez sur papier) pour chaque port (branchement externe) que
vous avez trouvez qui supporte le déboguage: 
*ID du périphérique PCI, id du bus, le numéro du port, et l'emplacement
physique du branchement USB*

Si vous n'avez pas trouvé de correspondant, vous ne pouvez pas déboguer à
travers EHCI. Désolé.

^1^ Les mecs de coreboot parlaient d'inclure le script dans la distribution de
coreboot (vérifiez le status de la discussion).

### Configuration initiale du BBB pour agir en tant que dongle de déboguage EHCI{#InitialsetupofBBBtoactasEHCIdebugdongle}

BBB doit être alimenté avec un [connecteur d'alimentation
coaxial](https://en.wikipedia.org/wiki/Coaxial_power_connector) puisque le
port USB mini-B sera utilisé pour la réception de la diffusion du déboguage
EHCI. Vous aurez donc besoin:

-   d'une alimentation (5V, 2A(10W) est suffisant).
-   un câble usb en plus: A à mini-B

(Sur le BBB) le kernel linux inclut le module g\_dbgp qui permet à un des
ports USB de l'ordinateur d'agir comme un dongle de déboguage EHCI.
Assurez-vous que vous avez ce module disponible sur votre BBB (Debian version
7.8 fournit avec le BBB devrait l'avoir), si non, vous devriez le compiler
vous-même (voir section suivante):

    ls /lib/modules/3.8.13-bone70/kernel/drivers/usb/gadget/g_dbgp.ko

Déchargez tout les autres modules g\_\*

    # lsmod
    # rmmod g_multi
    ...

Puis chargez g`dbgp:

    # modprobe g_dbgp
    # lsmod # should show that g_dbgp is loaded, and no other g_*

Branchez le côté mini-B du câble USB dans votre BBB et le côté A dans votre
cible. Ensuite, un des périphériques USB sur votre cible (avec `lsusb`)
devrait être:

    Bus 001 Device 024: ID 0525:c0de Netchip Technology, Inc.

Si vous voyez le périphérique sur la cible, vous êtes bon pour passer à l'étape
suivante.

### Patcher le module g\_dbgp du BBB (optionnel, mais hautement recommandé){#PatchBBBsgdbgpmoduleoptionalbuthighlyrecommended}

Sur les raisons de pourquoi vous avez besoin de ça, voyez: [Déboguage Gadget
EHCI](http://www.coreboot.org/EHCI_Gadget_Debug).\

Soyez sur que vous avez un environnement de compilation croisée configurée sur
votre hôte pour
l'architecture arm-linux-gnueabihf.

-   Sur le BBB: `uname -r` vous donnera le numéro de la version, tel que
    3.8.13-bone70 (je ferais référence à celà comme tel: \$mav.\$miv-\$lv: où
    mav=3.8, miv=13, lv=bone70).


-   Préparez le kernel BBB sur votre hôte pour la compilation croisée:    

<!-- -->

    $ cd $work_dir
    $ git clone https://github.com/beagleboard/kernel.git
    $ cd kernel
    $ git checkout $mav (see above)
    $ ./patch.sh
    $ wget http://arago-project.org/git/projects/?p=am33x-cm3.git\;a=blob_plain\;f=bin/am335x-pm-firmware.bin\;hb=HEAD -O kernel/firmware/am335x-pm-firmware.bin
    $ cp configs/beaglebone kernel/arch/arm/configs/beaglebone_defconfig

-   Téléchargez la rustine (*patch*) depuis [coreboot.org](http://www.coreboot.org/images/8/88/Ehci-debug-gadget-patches.tar.gz)

-   `tar -xf Ehci-debug-gadget-patches.tar.gz` (créera le répertoire:
    usbdebug-gadget)
-   Notez qu'il y a deux rustines (patch\_1 et patch\_2) pour chacune des
    différentes versions du kernel (3.8 et 3.10). J'utiliserais le 3.8. (Si
    vous utilisez le kernel 3.1, patch\_1 n'est pas nécessaire)
-   `cd kernel` (vous devriez être dans \$rep\_de\_travail/kernel/kernel)

-   Appliquez les rustines:

    git apply ../usbdebug-gadget/v3.8-debug-gadget/0001-usb-dbgp-gadget-Fix-re-connecting-after-USB-disconne.patch
    git apply ../usbdebug-gadget/v3.8-debug-gadget/0002-usb-serial-gadget-no-TTY-hangup-on-USB-disconnect-WI.patch
    ;
    make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- beaglebone_defconfig -j4@

-   Vous devriez aussi appliquez les scripts *deblob* de linux-libre pour le
    transformer le kernel en linux-libre (supprime tout les blobs du kernel
    linux). [site web de fsfla](http://www.fsfla.org/ikiwiki/selibre/linux-libre/) -
    jetez un coup d'oeil aux
    [scripts](http://www.fsfla.org/svn/fsfla/software/linux-libre/scripts/).
-   Obtenez la configuration actuelle du kernel de votre BBB depuis le fichier
    `/boot/config-<version>`, puis copiez là sur votre hôte en tant que
    \$rep\_de\_travail/kernel/kernel/.config .
-   Mettez un numéro de version propre:
    -   Sur votre hôte, éditez le fichier de configuration du kernel que vous
        venez juste de copier du BBB, trouvez la ligne 
        CONFIG\_LOCALVERSION="<qqch ou vide>" et changez là par
        CONFIG\_LOCALVERSION="-\$lv", donc ça ressemblera à quelque chose comme:
        CONFIG\_LOCALVERSION="-bone70"

-   Aussi, assurez-vous que: CONFIG\_USB\_G\_DBGP=m (si non, exécutez `make
    menuconfig` et définissez @Device Drivers-> USB Support -> USB
    Gadget Support -> EHCI Debug Device Gadget=m
-   Compilez le module:

    $ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j4 (is it possoble to build only the gadget modules)
    $ mkdir ../tmp && make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- INSTALL_MOD_PATH=../tmp modules_install

-   sur le BBB, sauvegardez
    `/lib/modules/3.8.13-bone70/kernel/drivers/usb/gadget`
    (p.ex en exécuant `mv /lib/modules/3.8.13-bone70/kernel/drivers/usb/gadget
    \$HOME)
-   copiez le dossier usb/gadget fraîchement compilé dans
    `/lib/modules/3.8.13-bone70/kernel/drivers/usb`
-   redémarrez le BBB
-   Enlevez tout les modules g\_\* (`rmmod g\_<>`)
-   `modprobe g\_dbgp` pour charger le module

### Configurer libreboot avec déboguage EHCI {#ConfigurelibrebootwithEHCIdebug}

Libreboot(coreboot) devrait être configuré avec le déboguage activé, et de
pousser les messages de déboguage vers le port de déboguage EHCI.\
Si vous avez téléchargé la distribution binaire, vous pouvez vérifier si elle
est proprement configurée de la manière suivante:

-   Allez dans le répertoire racine `dist` de libreboot:
        
        $ cd libreboot\_bin
-   Situez l'image ROM destinée à votre cible (je l'appelerais: \$img\_path)
-   Exécuter la commande suivante extraira la config dans un fichier nommé
    ./my\_config:

        $   ./cbfstool/i686/cbfstool $img_path extract -n config -f ./my_config

-   Soyez sûr que les paramètres suivants dans la config sont définis:

    CONFIG_USBDEBUG=y (Generic Drivers -> USB 2.0 EHCI debug dongle support)
    CONFIG_USBDEBUG_IN_ROMSTAGE=y (Generic Drivers -> Enable early (pre-RAM) usbdebug)
    CONFIG_USBDEBUG_HCD_INDEX=<HCD Index of usb controller - see below> (Generic Drivers -> Index for EHCI controller to use with usbdebug)
    CONFIG_USBDEBUG_DEFAULT_PORT=<USB Debug port - see below> (Generic Drivers -> Default USB port to use as Debug Port)

Les trois suivants sont derrières trois boutons radios dans le menu. Seulement
le premier ^[2](#___fn2)^ devrait être égal à 'y' 

    USBDEBUG_DONGLE_STD=y                       (Generic Drivers -> Type of dongle (Net20DC or compatible) -> Net20DC or compatible)
    CONFIG_USBDEBUG_DONGLE_BEAGLEBONE=n         (Generic Drivers -> Type of dongle (Net20DC or compatible) -> BeagleBone)
    CONFIG_USBDEBUG_DONGLE_BEAGLEBONE_BLACK=n   (Generic Drivers -> Type of dongle (Net20DC or compatible) -> BeagleBone Black)

^2^ Le module g\_dbpg sur le BBB (Rév. C) se rapporte lui-même en tant que
Net20DC, les autres options étant pour des BB(B) plus vieux - ver1. C'est
aussi testé, vérifié, et documenté [sur le blog de John Lewis](https://johnlewis.ie/coreboot-ehci-debug-gadget-demonstration/)

Ensuite:\

    CONFIG_CONSOLE_USB=y (Console -> USB dongle console output)

Aussi, les options Debugging \---> Output verbose blahblahblah)
(*FIXME*: quelqu'un devrait vérifier celà)

    CONFIG_DEBUG_CBFS=y (Output verbose CBFS debug messages )
    CONFIG_HAVE_DEBUG_RAM_SETUP=y (??? What/where is this)
    CONFIG_DEBUG_RAM_SETUP=y (Output verbose RAM init debug messages)
    CONFIG_DEBUG_SMI=y      (Output verbose SMI debug messages)
    CONFIG_DEBUG_ACPI=y     (Output verbose ACPI debug messages )
    CONFIG_DEBUG_USBDEBUG=y (Output verbose USB 2.0 EHCI debug dongle messages)

Si certaines des options de configuration mentionnées ci-dessus ne sont pas
comme spécifiée de base, vous avez à configurer et compiler libreboot
vous-même.

#### Sélectionner l'Index HCD et le port de déboguage USB {#SelectingHCDIndexandUSBDebugport}

Ceci s'applique (et marche) seulement si le contrôleur USB qui supporte le
déboguage (trouvé dans la première section de ce guide) provient d'Intel.
Si l'ID du PCI du port que vous avez trouvé dans la première section est 0000:00:1a.0
ou 0000:00:1d.0 , vous êtes ok. Sinon vous devez réessayez sans garantie que
ça marchera.

Si le port exposé extérieurement est sur un bus avec un ID du PCI égal à 0000:00:1a.0
alors choisissez 2 pour l'option CONFIG\_USBDEBUG\_HCD\_INDEX dans le kernel
sinon 0.

Pour CONFIG\_USBDEBUG\_DEFAULT\_PORT choisissez le port qui correspond à l'ID
du PCI, trouvé aussi dans la première section de ce guide.

Notes:\
Le dessus est basé sur l'implémentation du code
coreboot/src/southbridge/intel/common/usb\_debug.c :
pci\_ehci\_dbg\_dev() .\
C'est suffisant car ça s'applique aux ThinkPads GM45/G45 supportés.
Coreboot supporte d'autres contrôleurs aussi, mais ils n'ont pas (pour
l'instant) d'importance pour libreboot.

-   Sur le T500 (avec graphiques échangeables) les ports de déboguage pour les
    deux contrôleurs Intel sont exposés.
-   Sur le X200T les ports de déboguage pour les
    deux contrôleurs Intel sont exposés.

### Comment obtenir les journaux de déboguage {#Howtogetthedebuglogs}

-   Branchez le câble USB dans le port de déboguage de la cible (celui que
    vous avez trouvé dans l'étape 1) et l'USB mini-B BBB (côté A) sur l'hôte.
-   Soyez sûr qu'aucun autre modules des g\_\* à part le g\_dbpg est chargé.
-   Sur le BBB:

        $ stty -icrnl -inlcr -F /dev/ttyGS0
        $ cat /dev/ttyGS0

-   Allumez la cible avec Libreboot
-   Vous devriez voir des journaux de déboguage s'afficher dans votre console
    BBB

Notez que ce n'est pas permanent sur le BBB, si vous le redémarrez, vous avez
à `rmmod g\_\*` puis `modprobe g\_dbgp`

### Activer le déboguage EHCI sur le kernel cible (optionnel, recommandé {#EnebleEHCIDebugonthetargetskerneloptionalrecommended}

Vous devez savoir comment compiler le kernel pour votre cible.

1.  Vérifiez si le déboguage précoce est déjà activé:
        
       $ grep CONFIG\_EARLY\_PRINTK\_DBGP /boot/config-<version>
2.  Si c'est activé, vous n'avez pas à compiler le kernel (sautez cette
étape). Sinon, préparez les sources du kernel pour votre distribution et
sélectionnez l'option (Kernel hacking -> Early printk via EHCI debug port).
Compilez et installez le nouveau kernel.
3.  Éditez votre configuration GRUB (/etc/default/grub)  et ajoutez le suivant aux paramètres du
kernel ^[20](#___fn20)[21](#___fn21)^: earlyprintk=dbgp,keep.
    Aussi, essayez: earlyprintk=dbgp<N>,keep où N est l'ID du port de
    déboguage si le premier ne marche pas.
    
### Réferences {#References}

^10^ [EHCI Debug Port](http://www.coreboot.org/EHCI_Debug_Port)

^11^ [coreboot EHCI debug gadget
demonstration](https://johnlewis.ie/coreboot-ehci-debug-gadget-demonstration/)

^12^ [EHCI Gadget Debug](http://www.coreboot.org/EHCI_Gadget_Debug)

^13^
[Ehci-debug-gadget-patches.tar.gz](http://www.coreboot.org/images/8/88/Ehci-debug-gadget-patches.tar.gz)

^14^ [Compiling the BeagleBone Black
Kernel](http://wiki.beyondlogic.org/index.php/BeagleBoneBlack_Building_Kernel)

^15^
http://dumb-looks-free.blogspot.ca/2014/06/beaglebone-black-bbb-compile-kernel.html

^16^
http://dumb-looks-free.blogspot.fr/2014/06/beaglebone-black-bbb-kernal-headers.html

^17^ [Building BBB Kernel](http://elinux.org/Building_BBB_Kernel)

^18^
http://komposter.com.ua/documents/USB-2.0-Debug-Port%28John-Keys%29.pdf

^19^ [Exploring USB at the Hardware/Software
Interface](http://cs.usfca.edu/~cruse/cs698s10/)

^20^ https://www.kernel.org/doc/Documentation/x86/earlyprintk.txt

^21^ https://wiki.ubuntu.com/Kernel/Debugging/USBearlyprintk

*TODO*:

1.  GRUB n'envoie pas les messages au déboguage EHCI. Investiguer.
2.  La section "Configurer Libreboot avec le déboguage EHCI" peut être
simplifiée/sautée si une configuration commune marchant pour toutes les cibles
concernées est sélectionnée par défaut.
3.  Patcher et compiler le module g\_dbgp sur le BBB au lieu de faire de la
compilation croisée.
4.  Trouver une simple façon d'envoyer les messages de débogage depuis
l'espace utilisateur.

Copyright © 2015 Alex David <opdecirkel@gmail.com>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
A copy of this license is found in [../fdl-1.3.md](../fdl-1.3.md)
