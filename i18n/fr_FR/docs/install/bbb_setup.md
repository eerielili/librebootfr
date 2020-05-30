---
title: Comment programmer une puce flash SPI avec le BeagleBone Black ou Teensy 3.1
...

Ce document existe en tant que guide pour lire ou écrire dans une puce
flash SPI avec le BeagleBone Black, utilisant le logiciel
[flashrom](http://flashrom.org/Flashrom). Un BeagleBone Black, révision C a
été utilisé lors de la création de ce guide, mais des révisions plus ancienne
pourrait peut-être aussi marcher.

***À NOTER: l'utilisation du BeagleBone Black est seulement à titre d'exemple,
ne l'achetez à moins que vous LE voulez spécifiquement.*** *Il y a beaucoup
d'ordinateur monocarte (SBC, Single Board Computer) à architecture ARM qui
sont capables de programmation système interne (par flashage externe) et ils
exécutent aussi bien cette tâche. L'utilisation commune d'arbre des
périphériques (devicetree) sur ces appareils permet de les configurer de façon
similaire, mais pas identique, donc un peu de recherche est à faire de votre
côté. Mais il est possible d'utiliser des appareils plus petits comme la stm32
bluepill avec un autre ordinateur pour arriver à des résultats semblables.*

*Note: ce guide a été écrit pour Debian Stretch 9.5, qui est le dernier
système d'exploitation pour BeagleBone Black à la date de Juin 2019.
Il est possible que ces instructions sont obsolètes si de nouvelles versions
de systèmes d'exploitations supportées ont été publiées depuis.*

Il n'y avait pas de justification pour une section plus poussé pour le Teensy.
Référez-vous simplement à [cette page sur
flashrom.org](https://www.flashrom.org/Teensy_3.1_SPI_%2B_LPC/FWH_Flasher#ISP_Usage)
pour des informations sur comment le configurer, et corrélez celà avec les
PINs de la puce de flash SPI montrés dans d'autres guides de la documentation
libreboot sur chaque carte mère.
Au temps de l'écriture de ceci, le teensy est testé bon pour le flashage sur
le ThinkPad X200, mais ça devrait marcher pour d'autres cibles.
Voici une photo de la mise en place du teensy:
<http://h5ai.swiftgeek.net/IMG_20160601_120855.jpg>

Passons au BeagleBone Black...

Matériel requis
=====================

Liste de courses (des images du matos sont montrées plus tard):

-   Un programmeur SPI externe compatible [Flashrom](http://flashrom.org)
    *BeagleBone Back*, référé souvent en tant que 'BBB' (rev. C), est
    hautement recommandé. Vous pouvez en acheter un depuis
    [Adafruit](https://www.adafruit.com) (USA),
    [ElectroKit](http://electrokit.com) (Sweden) ou de n'importe qui figurant
    dans la liste des fournisseurs listés sur le [site web de
    BeagleBoard](http://beagleboard.org/black) (regardez en dessous de
    'Acheter'). Nous recommandons ce produit parce que nous savons qu'il
    marche bien pour nos missions et ne nécessite aucun logiciel non-libre.

-   Scotch isolant: couvrez entièrement le dessous du BBB (la partie qui
    repose sur la surface de la carte mère). C'est important de s'assurer que
    rien ne court-circuite lorsqu'on place le BBB sur une carte mère. La
    majorité des magasins de bricolage/électronique en ont. Optionnellement,
    vous pouvez utiliser la partie basse d'une [enclosure plastique
    Hammond](http://www.hammondmfg.com/1593HAM.htm#BeagleBoneBlack).
-   Des pinces pour se connecter sur la puce de flash: si vous avez une puce
    flash SOIC-16 (16 pins), vous aurez besoin des *Pomona 5252* ou équivalent.
    Pour les puces flash SOIC-8 (8 pins), vous aurez besoin des *Pomona 5250*
    ou équivalent. Vérifiez quelle puce vous avez avant de commander une
    pince.
    Aussi, vous ferez bien d'acheter deux pinces ou plus car elles se cassent
    facilement. [Farnell element 14](http://farnell.com/) vend celles-ci et
    livre dans de nombreux pays. Certaines personnes trouvent qu'il est
    difficile de s'en procurer, espécialement en Amérique du Sud.
    Si vous connaissez de bon fournisseurs, veuillez silvouplait contacter le
    projet libreboot avec les informations concernées. *Si vous ne pouvez pas
    vous procurer une pince pomona, d'autres pinces marcheront peut-être, p.ex
    3M, mais elles ne sont pas aussi fiables. Vous pouvez aussi souder
    directement les fils sur la puce si ça vous convient; les pinces sont
    juste là pour le comfort pour tout dire..*
-   *Alimentation externe 3.3V DC* pour alimenter la puce flash: une
    alimentation ATX / PSU (ou électrique, commune sur les ordinateurs de bureau Intel/AMD)
    feront l'affaire pour ça. Un PSU de laboration (DC, ajusté 3.3V)
    fonctionnera aussi.
        -    Avoir un multimètre peut être utile pour vérifier que l'on
             fournit bien 3.3V.
-   *Alimentation externe 5.5V DC* ([connecteur coaxial](https://en.wikipedia.org/wiki/Coaxial_power_connector), pour
    alimenter le BBB: ce dernier peut se faire alimenter via USB, mais une
    alimentation dédiée est recommandée. Celles-ci devrait être facile à
    trouver dans la plupart des magasins vendant de l'électronique. OPTIONEL.
    Seulement nécessaire si vous n'alimentez pas par USB, ou si vous voulez
    utiliser le [debug EHCI](../misc/bbb_ehci.md).
-   *Broches / câbles volants* (broches 2.54mm/0.1"): vous devrez vous
    procurer des câbles male--male, male--femelle et femelle--femelle de 10cm.
    Prenez-en beaucoup. D'autres noms possibles pour ces cables/fils/cuivrés
    sont:
        - câbles volants
        - cables de montage (puisqu'ils sont souvent utilisés sur les planches
          de montages)
        - vous pouvez aussi peut-être faire ces câbles vous-memes.

    [Adafruit](https://www.adafruit.com) en vend comme beaucoup d'autres.
    *Certaines personnes trouvent qu'ils sont difficiles à acheter. veuillez
    silvouplait contacter le project libreboot si vous connaissez de bon
    vendeurs.* Il est aussi possible que vous fabriquez ces câbles vous-même.
    Pour les connecteurs PSU, utiliser de long câbles, p.ex 20cm, est
    acceptable et vous pouvez les étendre si besoin.
-   *Câble USB Mini A-B* (le BeagleBone est déjà probablement fournit avec
    l'un d'eux.) - *OPTIONEL - seulement nécessaire pour le [déboguage
    EHCI](../misc/bbb_ehci.md) ou pour un accés ssh/série sans câble Ethernet
    (module kernel g\_multi)*
-   *Cable FTDI TTL ou carte de déboguage*: utilisé pour accéder la console
    série sur le BBB. [Cette
    page](http://elinux.org/Beagleboard:BeagleBone_Black_Serial) contient une
    liste de tels câbles. *OPTIONNEL\---seulement nécessaire pour la console
    série sur le BBB, si vous n'utilisez pas SSH via ethernet.*

Mettre en place le PSU 3.3V DC
==========================

Le brochage des PSU ATX peut être lu sur cette [page
Wikipédia](https://fr.wikipedia.org/wiki/Bloc_d%27alimentation#Alimentation_ATX).

Vous pouvez utiliser le pin 1 ou 2 (fil orange) sur un PSU ATX 20-pin ou
24-pin pour du 3.3V, et n'importe quels sources terre/masse (câbles noir)
pour la terre.
Court-circuitez (?) PS\_ON\# / Power on (fil vert; pin 16 sur le PSU ATX 24
pin, ou le pin 14 sur un PSU ATX 20 pin) à la terre (fil noir juste à côté) en
utilisant un câble/attache-feuilles/câble volant, puis allumez le PSU en
connectant à la masse PS\_ON\# (c'est aussi la manière donc une carte mère ATX
allume un PSU).


You can use pin 1 or 2 (orange wire) on a 20-pin or 24-pin ATX PSU for
3.3V, and any of the ground/earth sources (black cables) for ground.
Short PS\_ON\# / Power on (green wire; pin 16 on 24-pin ATX PSU, or pin
14 on a 20-pin ATX PSU) to a ground (black; there is one right next to
it) using a wire/paperclip/jumper, then power on the PSU by grounding
PS\_ON\# (this is also how an ATX motherboard turns on a PSU).

*DO NOT use pin 4, 6, do NOT use pin 19 or 20 (on a
20-pin ATX PSU), and DO NOT use pin 21, 22 or 23 (on a 24-pin
ATX PSU). Those wires (the red ones) are 5V, and they WILL kill
your flash chip. NEVER supply more than 3.3V to your flash
chip (that is, if it's a 3.3V flash chip; 5V and 1.8V SPI flash chips
do exist, but they are rare. Always check what voltage your chip takes.
Most of them take 3.3V).*

You only need one 3.3V supply and one ground for the flash chip, after
grounding PS\_ON\#.

The male end of a 0.1" or 2.54mm header cable is not thick enough to
remain permanently connected to the ATX PSU on its own. When connecting
header cables to the connector on the ATX PSU, use a female end attached
to a thicker piece of wire (you could use a paper clip), or wedge the
male end of the jumper cable into the sides of the hole in the
connector, instead of going through the centre.

Here is an example set up:\
![](images/x200/psu33.jpg "Copyright © 2015 Patrick "P. J." McDermott <pj@pehjota.net> see license notice at the end of this document")

Accéder au système d'exploitation du BBB
=========================================

Suivez les instructions [de démarrage](https://beagleboard.org/getting-started)
pour installer la dernière version de Debian sur le BBB. Il est recommandé de
télécharger l'édition eMMC IoT Flasher, qui écrira son image sur l'eMMC
intégrée.

Le système d'exploitation sur le BBB peut être accédé via SSH, avec comme nom
d'utilisateur 'debian' et comme mot de passe 'temppwd'. Suivez les
instructions sur la page de démarrage pour plus de détails.

Vous allez aussi utiliser le système d'exploitation sur votre BBB pour
programmer une puce flash SPI.


Alternatives to SSH (in case SSH fails)
---------------------------------------

You can also use a serial FTDI debug board with GNU Screen, to access
the serial console.
    # screen /dev/ttyUSB0 115200

Here are some example photos:\
![](images/x200/ftdi.jpg) ![](images/x200/ftdi_port.jpg)\

You can also connect the USB cable from the BBB to another computer and
a new network interface will appear, with its own IP address. This is
directly accessible from SSH, or screen:

    # screen /dev/ttyACM0 115200

You can also access the uboot console, using the serial method instead
of SSH.

Setting up spidev on the BBB
============================

Log in to the BBB using either SSH or a serial console as
described in [\#bbb\_access](#bbb_access).

*Note: The following commands are run as root. To run them from a normal user
account, add yourself to the `gpio` group to configure the pins and the `spi`
group to access spidev.*

Run the following commands to enable spidev:

    # config-pin P9.17 spi_cs
    # config-pin P9.18 spi
    # config-pin P9.21 spi
    # config-pin P9.22 spi_sclk

Verify that the spidev devices now exist:

    # ls /dev/spidev*

Output:

    /dev/spidev1.0  /dev/spidev1.1  /dev/spidev2.0  /dev/spidev2.1

Now the BBB is ready to be used for flashing. The following systemd service
file can optionally be enabled to make this persistent across reboots.

```
[Unit]
Description=Enable SPI function on pins

[Service]
Type=oneshot
ExecStart=config-pin P9.17 spi_cs
ExecStart=config-pin P9.18 spi
ExecStart=config-pin P9.21 spi
ExecStart=config-pin P9.22 spi_sclk
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

Get flashrom from the libreboot\_util release archive, or build it from
libreboot\_src/git if you need to. An ARM binary (statically compiled)
for flashrom exists in libreboot\_util releases. Put the flashrom binary
on your BBB.

You may also need ich9gen, if you will be flashing an ICH9-M laptop
(such as the X200). Get it from libreboot\_util, or build it from
libreboot\_src, and put the ARM binary for it on your BBB.

Finally, get the ROM image that you would like to flash and put that on
your BBB.

Now test flashrom:

    # ./flashrom -p linux_spi:dev=/dev/spidev1.0,spispeed=512

Output:

    Calibrating delay loop... OK.
    No EEPROM/flash device found.
    Note: flashrom can never write if the flash chip isn't found automatically.

This means that it's working (the clip isn't connected to any flash
chip, so the error is fine).

Connecting the Pomona 5250/5252
===============================

Use this image for reference when connecting the pomona to the BBB:
<http://beagleboard.org/Support/bone101#headers> (D0 = MISO or connects
to MISO).

The following shows how to connect clip to the BBB (on the P9 header),
for SOIC-16 (clip: Pomona 5252):

     NC              -       - 21
     1               -       - 17
     NC              -       - NC
     NC              -       - NC
     NC              -       - NC
     NC              -       - NC
     18              -       - 3.3V (PSU)
     22              -       - NC - this is pin 1 on the flash chip
    This is how you will connect. Numbers refer to pin numbers on the BBB, on the plugs near the DC jack.

    You may also need to connect pins 1 and 9 (tie to 3.3V supply). These are HOLD# and WP#.
    On some systems they are held high, if the flash chip is attached to the board.
    If you're flashing a chip that isn't connected to a board, you'll almost certainly
    have to connect them.

    SOIC16 pinout (more info available online, or in the datasheet for your flash chip):
    HOLD    1-16    SCK
    VDD 2-15    MOSI
    N/C 3-14    N/C
    N/C 4-13    N/C
    N/C 5-12    N/C
    N/C 6-11    N/C
    SS  7-10    GND
    MISO    8-9 WP

The following shows how to connect clip to the BBB (on the P9 header),
for SOIC-8 (clip: Pomona 5250):

     18              -       - 1
     22              -       - NC
     NC              -       - 21
     3.3V (PSU)      -       - 17 - this is pin 1 on the flash chip
    This is how you will connect. Numbers refer to pin numbers on the BBB, on the plugs near the DC jack.

    You may also need to connect pins 3 and 7 (tie to 3.3V supply). These are HOLD# and WP#.
    On some systems they are held high, if the flash chip is attached to the board.
    If you're flashing a chip that isn't connected to a board, you'll almost certainly
    have to connect them.

    SOIC8 pinout (more info available online, or in the datasheet for your flash chip):
    SS  1-8 VDD
    MISO    2-7 HOLD
    WP  3-6 SCK
    GND 4-5 MOSI

`NC = no connection`

*DO NOT connect 3.3V (PSU) yet. ONLY connect this once the pomona is
connected to the flash chip.*

*You also need to connect the BLACK wire (ground/earth) from the 3.3V
PSU to pin 2 on the BBB (P9 header). It is safe to install this now
(that is, before you connect the pomona to the flash chip); in fact, you
should.*

if you need to extend the 3.3v psu leads, just use the same colour M-F
leads, *but* keep all other leads short and equal length (30cm or less).
Keep in mind that length isn't inversely proportional to signal quality,
so trying out different lengths will yield different results.
Same goes for spispeed.

You should now have something that looks like this:\
![](images/x200/5252_bbb0.jpg) ![](images/x200/5252_bbb1.jpg)

Copyright © 2014, 2015 Leah Rowe <info@minifree.org>\
Copyright © 2015 Patrick "P. J." McDermott <pj@pehjota.net>\
Copyright © 2015 Albin Söderqvist\

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License Version 1.3 or any later
version published by the Free Software Foundation
with no Invariant Sections, no Front Cover Texts, and no Back Cover Texts.
A copy of this license is found in [../fdl-1.3.md](../fdl-1.3.md)
