---
title: "Jeux de puces GM45: enlever la ME (manageability engine ou moteur d'administration)"
...

Ces sections concernent la désactivation et la suppression du ME (Intel
*M*anagement *E*ngine) sur les GM45. Originellement, ça a été fait sur le
ThinkPad X200, et plus tard adapté pour le ThinkPad R400/T400/T500.
Par principe, ça peut être fait sur n'importe quel système GM45 ou GS45.

La ME est un blob qui doît être normalement laissé à l'intérieur de la puce de
flash (dans la région ME, comme défini dans le descripteur par défaut).
Sur les GM45, il est possible de la supprimer sans mauvais effets secondaires.
Toutes les autres parts de coreboot sur les systèmes GM45 (comme le GMA
MHD4500/ graphiques Intel) peuvent être libres de blob, donc supprimer la ME
était le dernier obstacle pour faire de la GM45 une cible faisable dans
libreboot (les systèmes peuvent aussi marcher sans les blobs de microcode).

Dans Libreboot, La ME est supprimée et désactivée en modifiant le descripteur.
Plus d'informations à propos de ceci peuvent être trouvées dans le code source
d'ich9gen/ich9deblob dans le répertoire
[resources/utilities/ich9deblob](resources/utilites/ich9deblob/src/) du
projet libreboot, ou plus généralement sur cette page.

Plus d'informations à propos de la ME peuvent être trouvées sur
<http://www.coreboot.org/Intel_Management_Engine> et
<http://me.bios.io/Main_Page>.

Un autre projet récemment trouvé: <http://io.netgarage.org/me/>

Utilitaire ICH9 gen {#ich9gen}
================

Il n'est désormais plus nécessaire d'utiliser [ich9deblob](#ich9deblob) pour
générer un descripteur déblobbé + une image GBE pour les cibles GM45. ich9gen
est un petit utilitaire dans ich9deblob qui peut les générer à partir de rien,
sans un cliché mémoire du factory.bin .

Les exécutables d'ich9gen peuvent être trouvés sous
[./ich9deblob/](resources/utilities/ich9deblob/src/), compilés
statiquement dans libreboot\_util. Si vous utilisez src ou git, compilez
ich9gen depuis la source avec:

    $ ./oldbuild module ich9deblob

L'exécutable apparaîtra sous
[resources/utilities/ich9deblob/](../../resources/utilities/ich9deblob/)

Exécutez:

    $ ./ich9gen

Exécuter ich9gen de cette façon (sans aucun arguments) genère un descripteur
par défaut + une image GBE avec une adresse MAC générique.
Vous ne voulez probablement pas utilisez celle de base; les images ROM dans
libreboot contiennent un image descripteur+gbe par défaut (déjà insérée) juste
pour prévénir ou mitiger le risque du bousillage de l'ordinateur portable 
(NdT:*bricking*, rendre telle une brique), mais avec une adresse MAC générique
(le projet libreboot ne sait en rien qu'elle est votre réelle adresse MAC).

Dans GNU+Linux, vous pouvez trouver votre adresse MAC grâce à la sortie des
commandes `ip addr` ou `ifconfig`.
Alternativement, si vous marchez déjà sous libreboot (avec la bonne adresse
MAC dans votre ROM), faîtes en un cliché (`flashrom -r`) et lisez les 6
premiers octets depuis la position 0x1000 (ou 0x2000) dans un éditeur
hexadécimal (ou, renommez-le factory.rom et exécutez le par ich9deblob: dans le
mkgbe.c fraîchement créé il y aura les octets individuels de votre adresse
MAC).
Si vous êtes en train d'exécuter le micrologiciel de base et vous n'avez pas
encore installé libreboot, vous pouvez aussi l'exécuter par ich9debkib pour
récupérer l'adresse mac.

Une façon encore plus simple d'avoir l'adresse MAC serait de lire
la petite étiquette en bas/sur la base de l'ordinateur portable.

Sur les ordinateur portables GM45 qui utilisent les descripteurs flash,
l'adresse MAC ou le jeu de puces  (chipset) ethernet embarqué est flashé (dans
l'image ROM). Vous devriez générer votre propre adresse MAC à l'intéreur (avec
la somme de contrôle du GBE mise à jour afin qu'elle corresponde). Exécutez:

    $ ./ich9gen --macaddress XX:XX:XX:XX:XX:XX

(remplaçez les charactères XX par les valeurs hexadécimales de l'adresse MAC
que vous désirez).

Trois nouveaux fichiers seront créés:

-    `ich9fdgbe_4m.bin`: destiné aux ordinateurs portables GM45 avec la puce flash
     de 4Mo.
-    `ich9fdgbe_8m.bin`: idem, mais pour puce flash de 8Mo.
-    `ich9fdgbe_16m.bin`: idem, mais pour puce flash de 16Mo.

En assumant que votre image libreboot est nommée **libreboot.rom**, copiez le
fichier qui va bien dans le même répertoire que votre image et ensuite insérer
le fichier descripteur+GBE dans l'image ROM.\
Pour les puces flash de 16Mo:

    # dd if=ich9fdgbe_16m.bin of=libreboot.rom bs=12k count=1 conv=notrunc

Celles de 8Mo:

    # dd if=ich9fdgbe_8m.bin of=libreboot.rom bs=12k count=1 conv=notrunc

Et enfin, pour celles de 4Mo:

    # dd if=ich9fdgbe_4m.bin of=libreboot.rom bs=12k count=1 conv=notrunc

Votre image libreboot.rom est maintenant prête à être flashée sur le système.
Référez vous à [../install/\#flashrom](../install/#flashrom) pour savoir
comment la flasher.

Protéger en écriture la puce de flashage
-------------------------------

Cherchez les lignes suivantes dans la fonction *descriptorHostRegionsUnlocked*
se trouvant dans le fichier
*resources/utilities/ich9deblob/src/descriptor/descriptor.c* :

    descriptorStruct.masterAccessSection.flMstr1.fdRegionWriteAccess = 0x1;
    descriptorStruct.masterAccessSection.flMstr1.biosRegionWriteAccess = 0x1;
    descriptorStruct.masterAccessSection.flMstr1.meRegionWriteAccess = 0x1;
    descriptorStruct.masterAccessSection.flMstr1.gbeRegionWriteAccess = 0x1;
    descriptorStruct.masterAccessSection.flMstr1.pdRegionWriteAccess = 0x1;

Regardez aussi dans le fichier *resources/utilities/ich9deblob/src/ich9gen/mkdescriptor.c*
pour les lignes suivantes:

    descriptorStruct.masterAccessSection.flMstr1.fdRegionWriteAccess = 0x1; /* see ../descriptor/descriptor.c */
    descriptorStruct.masterAccessSection.flMstr1.biosRegionWriteAccess = 0x1; /* see ../descriptor/descriptor.c */
    descriptorStruct.masterAccessSection.flMstr1.meRegionWriteAccess = 0x1; /* see ../descriptor/descriptor.c */
    descriptorStruct.masterAccessSection.flMstr1.gbeRegionWriteAccess = 0x1; /* see ../descriptor/descriptor.c */
    descriptorStruct.masterAccessSection.flMstr1.pdRegionWriteAccess = 0x1; /* see ../descriptor/descriptor.c */

NOTEZ: Quand vous protégez en écriture la puce de flash, le reflashage n'est
alors plus possible à moins que vous utilisiez de l'équipement externe
dédicacé à cela, ce qui implique un désassemblage de l'ordinateur portable.
Le même équipement peut être aussi utilisé pour enlever plus tard la
protection en écriture, si vous le voulez.
Protégez en écriture la puce \*seulement\* si vous avez le bon équipement qui
va avec, afin de flasher extérieurement plus tard; par exemple, jetez un coup
d'oeil à [../install/bbb\_setup.md](../install/bbb_setup.md).

Modifiez les valeurs de variables à 0x0, puis recompilez ich9gen. Après que
vous avez fait celà, suivez les notes dans [\#ich9gen](#ich9gen) pour générer
une nouvelle image de descripteur+gbe puis insérez la dans votre image ROM, et
ensuite flashez la.
La prochaine fois que vous démarrez, la puce de flash sera en mode "lecture
seulement" dans les logiciels (le reflashage matériel marchera encore, vous en
aurez besoin pour reflasher la puce après l'avoir protégée en écriture,
nettoyer la protection en écriture ou alors flasher une autre image ROM avec
la protection en écriture définie dans le descripteur).

Flashrom vous dira qu'il peut forcer le reflashage, en utilisant  *-p
internal:ich\_spi\_force=yes* mais en fait ça ne marchera pas; ça va juste
bousiller votre ordinateur portable.

Pour des guides sur le flashage externe, référez vous à
[../install/](../install/).

L'utilitaire ICH9 deblob {#ich9deblob}
===================

**Ce n'est plus strictement nécessaire. Les images ROM de Libreboot
contiennent désormais par défaut le descripteur+gbe de 12Ko généré par
ich9gen.**

C'était l'outil originnellement utilisé pour désactivé la ME sur le X200 (et
plus tard adapté pour les autres systèmes utilisant le jeu de puces GM45).
[ich9gen](#ich9gen) le supplante; ich9gen est mieux parce qu'il ne dépend pas
du cliché mémoire de l'image factory.rom (tandis que ich9deblob oui).


C'est que vous utiliserez pour générer les régions descripteur+gbe déblobbées
pour votre image ROM de Libreboot.

Si vous êtes en train de travaillez avec git ou libreboot\_src, vous pouvez
trouver ses sources sous le répertoire
[resources/utilities/ich9deblob/](../resources/utilities/ich9deblob/) et sera
déjà compilé si vous avez exécuté **./oldbuild module all** ou **./oldbuild
module ich9deblob** depuis le répertoire racine, sinon vous pouvez le compilez
en faisant ceci:

    $ ./oldbuild module ich9deblob

Un fichier exécutable nommé **ich9deblob** apparaîtra maintenant dans le
répertoire resources/utilities/ich9deblob/

Si vous travaillez avec l'archive des versions de libreboot\_util, vous
trouvez l'utilitaire sous ./ich9deblob/, compilé statiquement (pour i686 et
x86\_64 sur GNU+Linux). 


Mettez le factory.rom de votre système (pouvant être obtenu en suivant les
guides de flashage externe pour les cibles GM45, [../install/](../install/))
dans le répertoire où se trouve l'exécutable de ich9deblob, puis exécutez
l'utilitaire:

    $ ./ich9deblob

Un fichier de 12Ko nommé **deblobbed\_descriptor.bin** apparaîtra maintenant.
**Gardez-le ainsi que le factory.rom dans un endroit sûr!** Le premier bloc de
4Ko contient la région de données du descripteur pour votre système, et le
bloc suivant de 8Ko contient la région GBE (données de configuration pour
votre carte réseau gigabit). Ces deux régions pourrait en fait être dans des
fichiers séparés, mais ils sont joints en un seul fichier dans ce cas là.

Un fichier de 4Ko nommé **deblobbed\_4kdescriptor.bin** apparaîtra
alternativement, si aucune région GBE n'a été détectée dans l'image ROM.
C'est souvent le cas lorsque une carte réseau discrète est utilisé (p.e.
Broadcom) au lieu d'Intel. Seulement les cartes réseaux Intel ont besoin d'une
région GBE dans la puce flash.

En assumant que votre image libreboot est nommée **libreboot.rom**, copier
le fichier **deblobbed\_descriptor.bin** où **libreboot.rom** est située puis
ensuite exécutez:

    # dd if=deblobbed_descriptor.bin of=libreboot.rom bs=12k count=1 conv=notrunc

Alternativement, si vous avez le fichier **deblobbed\_4kdescriptor.bin** (pas
de GBE défini), faites ceci:

    # dd if=deblobbed_4kdescriptor.bin of=libreboot.rom bs=4k count=1 conv=notrunc

L'utilitaire va aussi générer 4 fichier supplémentaires:

-   mkdescriptor.c
-   mkdescriptor.h
-   mkgbe.c
-   mkgbe.h

Ce sont des fichiers sources en C qui peuvent regénérer les mêmes structs Gbe
et Descriptor (de ich9deblob/ich9gen). Pour les utiliser, placez les dans
src/ich9gen/ dans ich9deblob, puis recompilez. L'exécutable **ich9gen**
nouvellement compilé sera capable de recréer les fichiers de 12Ko à partir de
rien, en se basant sur les structures en C, cette fois **sans** avoir besoin
d'un cliché mémoire factory.rom !

Vous devriez maintenant avor une image **libreboot.rom** contenant le
descripteur de 4K de correct et les régions GBE de 8Ko, qui seront ensuite
flashable sûrement.
Référez vous à [../install/\#flashrom](../install/#flashrom) sur comment la
flasher.

Utilitaire demefactory {#demefactory}
===================

Il prend un cliché mémoire factory.rom et désactive le ME/TPM, mais laisse la
région intacte. Ça rend aussi toutes les régions accessibles en
lecture/écriture.

La ME intéfère avec la lecture/écriture du flash dans flashrom, et le
descripteur par défaut verrouille quelques régions. L'idée est que cet outil
enlevera toutes ces restrictions.

Exécutez simplement (avec factory.rom dans le même répertoire):

    $ ./demefactory

Ça générera un fichier descripteur de 4Ko (seulement le descripteur, pas de
GBE). Insérez ça dans une image factory.rom (NOTE: faites ceci sur une copie.
Gardez le factory.rom original dans un endroit sûr):

    # dd if=demefactory_4kdescriptor.bin of=factory_nome.rom bs=4k count=1 conv=notrunc


AFAIRE: tester cela.\
AFAIRE: lenovobios (GM45 thinkpads) protège encore en lecture/écriture des
parties du flash. Modifier à l'intérieur le code assembleur. Note: le
factory.rom ( region BIOS) du lenovobios est dans un format compressé que vous
avez à extraire.
bios\_extract en amont ne marchera pas, mais le suivant a été dit dans dans
IRC sur freenode, canal \#coreboot:
    
    <roxfan> vimuser: try bios_extract with ffv patch http://patchwork.coreboot.org/patch/3444/
    <roxfan> or https://github.com/coreboot/bios_extract/blob/master/phoenix_extract.py
    <roxfan> what are you looking for specifically, btw?

    0x74: 0x9fff03e0 PR0: Warning: 0x003e0000-0x01ffffff is read-only.
    0x84: 0x81ff81f8 PR4: Warning: 0x001f8000-0x001fffff is locked.

Cas d'utilisation: une image factory.rom modifiée d'une façon qu'il n'y a
aucune protection de flash, rendant facile le changement usine/libreboot
depuis le logiciel, sans n'avoir plus à faire de désassemblage et de
reflashage externe à moins que vous bousillez l'appareil.

demefactory fait partie de la source de ich9deblob, trouvée dans
*resources/utilities/ich9deblob/*

Les sections ci-dessous sont adaptés (en majorités) de journaux de discussions
IRC concernant le début du développement sur comment enlever la ME sur le
GM45. Ils sont utiles en tant qu'historique. Ce n'aurait pas pu être possible
sans l'aide de sgsit.

Notes du début {#early_notes}
-----------

-   <http://www.intel.co.uk/content/dam/doc/datasheet/io-controller-hub-10-family-datasheet.pdf>
    la page 230 mentionne un mode descripteur et non-descripteur (qui enlève
    le GBE et la ME/AMT).
-   ~~**regarder la référence à HDA\_500 (désactiver la sécurité du
    descripteur)**~~
    est ce que le pin GPI033 attaché est sur le ICH9-M (X200). HDA\_SDO
    s'applique à des jeux de puces plus tardif (séries 6 ou plus haut.
    Désactiver la sécurité du descripteur désactive aussi l'ethernet d'après
    sgsit. La méthode de sgsit implique l'utilisation "d'attaches douces"
    (voyez les journaux IRC ci-dessous) au lieu de désactiver le descripteur.
-   **et la place du GPIO33 sur les x200: (c'était un lien externe. On le met
    plutôt ici à la place)**
    [images/x200/gpio33\_location.jpg](images/x200/gpio33_location.jpg) -
    c'est au dessus du nombre 7 dans TP37 (qui est au-dessus de la grande puce
    Intel en bas)
-   La fiche de caractéristiques techniques de la ME n'est peut être pas pour
    les appareils mobiles (NdT: on entend ordinateurs portables ici), mais ça
    ne change pas grand chose. Celle ci couvre et donne quelques détails sur
    le QM67 que le X201 utilise:
    <http://www.intel.co.uk/content/dam/www/public/us/en/documents/datasheets/6-chipset-c200-chipset-datasheet.pdf>

Puces flash {#flashchips}
-----------
-   Ordi portable X200 (Mocha-1):
    ICH9-M supplante les permissions ifd grâce à une attache connecté au pin
    GPIO33 (voyez les notes IRC ci-dessous)
    
    - Le X200 peut être trouvé avec n'importe quel des puces de flash
      suivantes:
        -   ATMEL AT26DF321-SU 72.26321.A01 - c'est une puce flash de 32Mbits
            (4Mo).
        -   MXIC (Macronix?) MX25L3205DM2I-12G 72.25325.A01 - idem
        -   MXIC (Macronix?) MX25L6405DMI-12G 41R0820AA - c'est une puce de
            64Mbits (8Mo)
        -   Winbond W25X64VSFIG 41R0820BA - idem

    sgsit dit que les X200 (Pecan-1) avec les puces flash de 64Mbits sont
    problablement celles avec AMT (à côte de la ME), avec que les puces
    32Mbits contiennent seulement la ME.

Notes de développement précoces {#early_development_notes}
-----------------------

    Start (hex) End (hex)   Length (hex)    Area Name
    ----------- ---------   ------------    ---------
    00000000    003FFFFF    00400000    Flash Image

    00000000    00000FFF    00001000    Descriptor Region
    00000004    0000000F    0000000C        Descriptor Map
    00000010    0000001B    0000000C        Component Section
    00000040    0000004F    00000010        Region Section
    00000060    0000006B    0000000C        Master Access Section
    00000060    00000063    00000004            CPU/BIOS
    00000064    00000067    00000004            Manageability Engine (ME)
    00000068    0000006B    00000004            GbE LAN
    00000100    00000103    00000004        ICH Strap 0
    00000104    00000107    00000004        ICH Strap 1
    00000200    00000203    00000004        MCH Strap 0
    00000EFC    00000EFF    00000004        Descriptor Map 2
    00000ED0    00000EF7    00000028        ME VSCC Table
    00000ED0    00000ED7    00000008            Flash device 1
    00000ED8    00000EDF    00000008            Flash device 2
    00000EE0    00000EE7    00000008            Flash device 3
    00000EE8    00000EEF    00000008            Flash device 4
    00000EF0    00000EF7    00000008            Flash device 5
    00000F00    00000FFF    00000100        OEM Section
    00001000    001F5FFF    001F5000    ME Region
    001F6000    001F7FFF    00002000    GbE Region
    001F8000    001FFFFF    00008000    PDR Region
    00200000    003FFFFF    00200000    BIOS Region

    Start (hex) End (hex)   Length (hex)    Area Name
    ----------- ---------   ------------    ---------
    00000000    003FFFFF    00400000    Flash Image

    00000000    00000FFF    00001000    Descriptor Region
    00000004    0000000F    0000000C        Descriptor Map
    00000010    0000001B    0000000C        Component Section
    00000040    0000004F    00000010        Region Section
    00000060    0000006B    0000000C        Master Access Section
    00000060    00000063    00000004            CPU/BIOS
    00000064    00000067    00000004            Manageability Engine (ME)
    00000068    0000006B    00000004            GbE LAN
    00000100    00000103    00000004        ICH Strap 0
    00000104    00000107    00000004        ICH Strap 1
    00000200    00000203    00000004        MCH Strap 0
    00000ED0    00000EF7    00000028        ME VSCC Table
    00000ED0    00000ED7    00000008            Flash device 1
    00000ED8    00000EDF    00000008            Flash device 2
    00000EE0    00000EE7    00000008            Flash device 3
    00000EE8    00000EEF    00000008            Flash device 4
    00000EF0    00000EF7    00000008            Flash device 5
    00000EFC    00000EFF    00000004        Descriptor Map 2
    00000F00    00000FFF    00000100        OEM Section
    00001000    00002FFF    00002000    GbE Region
    00003000    00202FFF    00200000    BIOS Region

    Build Settings
    --------------
    Flash Erase Size = 0x1000

Ces données ont été produites par un utilitaire nommé 'Flash Image Tool' pour
la ME version 4.x. Vous y faites glisser dedans une image complète et
l'utilitaire décompose les nombreux composants, vous permettant de mettre des
attaches douces.

Cet outil est propriétaire, pour Windows seulement, mais a été utilisé pour
déblob le X200. La fin a justifié les moyens, et l'utilitaire n'est désormais
plus nécessaires puisque l'utilitaire ich9deblob (documenté sur cette page)
peut être maitenant utilisé pour créer des descripteurs déblobés.


GBE (gigabit ethernet) region in SPI flash {#gbe_region}
------------------------------------------

Of the 8K, about 95% is 0xFF. The data is the gbe region is fully
documented in this public datasheet:
<http://www.intel.co.uk/content/dam/doc/application-note/i-o-controller-hub-9m-82567lf-lm-v-nvm-map-appl-note.pdf>

The only actual content found was:

    00  1F  1F  1F  1F  1F  00  08  FF  FF  83  10  FF  FF  FF  FF  
    08  10  FF  FF  C3  10  EE  20  AA  17  F5  10  86  80  00  00  
    01  0D  00  00  00  00  05  06  20  30  00  0A  00  00  8B  8D  
    02  06  40  2B  43  00  00  00  F5  10  AD  BA  F5  10  BF  10  
    AD  BA  CB  10  AD  BA  AD  BA  00  00  00  00  00  00  00  00  
    00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  
    00  01  00  40  28  12  07  40  FF  FF  FF  FF  FF  FF  FF  FF  
    FF  FF  FF  FF  FF  FF  FF  FF  FF  FF  FF  FF  FF  FF  D9  F0  
    20  60  1F  00  02  00  13  00  00  80  1D  00  FF  00  16  00  
    DD  CC  18  00  11  20  17  00  DD  DD  18  00  12  20  17  00  
    00  80  1D  00  00  00  1F  

The first part is the MAC address set to all 0x1F. It's repeated haly
way through the 8K area, and the rest is all 0xFF. This is all
documented in the datasheet.

The GBe region starts at 0x20A000 bytes from the \*end\* of a factory
image and is 0x2000 bytes long. In libreboot (deblobbed) the descriptor
is set to put gbe directly after the initial 4K flash descriptor. So the
first 4K of the ROM is the descriptor, and then the next 8K is the gbe
region.

### GBE region: change MAC address {#gbe_region_changemacaddress}

According to the datasheet, it's supposed to add up to 0xBABA but can
actually be others on the X200.
<https://web.archive.org/web/20150912070329/https://communities.intel.com/community/wired/blog/2010/10/14/how-to-basic-eeprom-checksums>

*"One of those engineers loves classic rock music, so they selected
0xBABA"*

In honour of the song *Baba O'Reilly* by *The Who* apparently. We're
not making this stuff up...

0x3ABA, 0x34BA, 0x40BA and more have been observed in the main Gbe
regions on the X200 factory.rom dumps. The checksums of the backup
regions match BABA, however.

By default, the X200 (as shipped by Lenovo) actually has an invalid main
gbe checksum. The backup gbe region is correct, and is what these
systems default to. Basically, you should do what you need on the
\*backup\* gbe region, and then correct the main one by copying from the
backup.

Look at resources/utilities/ich9deblob/ich9deblob.c.

-   Add the first 0x3F 16bit numbers (unsigned) of the GBe descriptor
    together (this includes the checksum value) and that has to add up
    to 0xBABA. In other words, the checksum is 0xBABA minus the total of
    the first 0x3E 16bit numbers (unsigned), ignoring any overflow.

Flash descriptor region {#flash_descriptor_region}
-----------------------

<http://www.intel.co.uk/content/dam/doc/datasheet/io-controller-hub-9-datasheet.pdf>
from page 850 onwards. This explains everything that is in the flash
descriptor, which can be used to understand what libreboot is doing
about modifying it.

How to deblob:

-   patch the number of regions present in the descriptor from 5 - 3
-   originally descriptor + bios + me + gbe + platform
-   modified = descriptor + bios + gbe
-   the next stage is to patch the part of the descriptor which defines
    the start and end point of each section
-   then cut out the gbe region and insert it just after the region
-   all this can be substantiated with public docs (ICH9 datasheet)
-   the final part is flipping 2 bits. Halting the ME via 1 MCH soft
    strap and 1 ICH soft strap
-   the part of the descriptor described there gives the base address
    and length of each region (bits 12:24 of each address)
-   to disable a region, you set the base address to 0xFFF and the
    length to 0
-   and you change the number of regions from 4 (zero based) to 2

There's an interesting parameter called 'ME Alternate disable', which
allows the ME to only handle hardware errata in the southbridge, but
disables any other functionality. This is similar to the 'ignition' in
the 5 series and higher but using the standard firmware instead of a
small 128K version. Useless for libreboot, though.

To deblob GM45, you chop out the platform and ME regions and correct the
addresses in flReg1-4. Then you set meDisable to 1 in ICHSTRAP0 and
MCHSTRAP0.

How to patch the descriptor from the factory.rom dump

-   map the first 4k into the struct (minus the gbe region)
-   set NR in FLMAP0 to 2 (from 4)
-   adjust BASE and LIMIT in flReg1,2,3,4 to reflect the new location of
    each region (or remove them in the case of Platform and ME)
-   set meDisable to 1/true in ICHSTRAP0 and MCHSTRAP0
-   extract the 8k GBe region and append that to the end of the 4k
    descriptor
-   output the 12k concatenated chunk
-   Then it can be dd'd into the first 12K part of a coreboot image.
-   the GBe region always starts 0x20A000 bytes from the end of the ROM

This means that libreboot's descriptor region will simply define the
following regions:

-   descriptor (4K)
-   gbe (8K)
-   bios (rest of flash chip. CBFS also set to occupy this whole size)

The data in the descriptor region is little endian, and it represents
bits 24:12 of the address (bits 12-24, written this way since bit 24 is
nearer to left than bit 12 in the binary representation).

So, *x << 12 = address*

If it's in descriptor mode, then the first 4 bytes will be 5A A5 F0 0F.

platform data partition in boot flash (factory.rom / lenovo bios) {#platform_data_region}
-----------------------------------------------------------------

Basically useless for libreboot, since it appears to be a blob. Removing
it didn't cause any issues in libreboot.

This is a 32K region from the factory image. It could be data
(non-functional) that the original Lenovo BIOS used, but we don't know.

It has only a 448 byte fragment different from 0x00 or 0xFF.

Copyright © 2014, 2015 Leah Rowe <info@minifree.org>\

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License Version 1.3 or any later
version published by the Free Software Foundation
with no Invariant Sections, no Front Cover Texts, and no Back Cover Texts.
A copy of this license is found in [../fdl-1.3.md](../fdl-1.3.md)
