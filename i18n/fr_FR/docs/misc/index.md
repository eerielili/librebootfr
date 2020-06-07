---
title: Divers 
x-toc-enable: true
...

Plainte aigue quand le poste est vacant sur Debian ou Devuan
======================================================================

Exécutez powertop automatiquement lors du démarrage.

Un script inclut dans libreboot s'appelle 'powertop.debian'. Exécutez le en
tant que root et il configurera powertop afin qu'il s'exécute avec --auto-tune
lors du démarrage. Ouvrez/chargez le fichier dans votre éditeur de texte pour
savoir comment il fait ça.

    $ sudo ./resources/scripts/misc/powertop.debian

Vous voudriez peut-être l'exécuter (l'exécutable `powertop`) avec --calibrate
en premier.

Si powertop ne marche pas, une autre façon est d'ajouter
*processor.max\_cstate=2* à la ligne *linux* dans le fichier grub.cfg, en
utilisant [ce guide](../gnulinux/grub.cbfs.md). Cette option a l'inconvénient
d'augmenter la consommation de la batterie.

Plainte aigue quand le poste est vacant sur Parabola
==============================================================

Le suivant enlève la majorité du bruit. Ça réduit la plainte aigue (que tout
le monde n'entend pas forcément) à un léger bourdonnement (qui n'est pas
entendu par la plupart des personnes ou ne les dérange pas).

Ce n'est pas parfait! La solution complète n'est pas encore découverte mais
c'est une étape vers elle. Aussi, dans certains cas, vous aurez besoin
d'exécuter `sudo powertop --auto-tune` une nouvelle fois. Ça a besoin d'être
proprement implémenté dans coreboot lui-même!

Sur le X60 avec coreboot ou libreboot, il y a une plainte aigue lorsque
l'appareil est vacant. Jusque ici, nous avons utilisé processor.max\_cstate=2
ou idle=halt dans GRUB. Ces options augmentent la consommation d'énergie.
Arrêtez de les utilisez!

Soyez root

    $ su -

Installez powertop:

    # pacman -S powertop

et ajoutez le suivant à /etc/systemd/system/powertop.service :

    [Unit]
    Description=Powertop tunings

    [Service]
    Type=oneshot
    RemainAfterExit=no
    ExecStart=/usr/bin/powertop --auto-tune
    # "powertop --auto-tune" still needs a terminal for some reason. Possibly a bug?
    Environment="TERM=xterm"

    [Install]
    WantedBy=multi-user.target

Finalement, faites ceci en tant que root:

    # powertop --calibrate
    # systemctl enable powertop
    # systemctl start powertop

La prochaine fois que vous démarrez les système, le bourdonnement sera parti.

[Note: l'utilisation d'un kernel avec grsec activé désactivera la
fonctionnalité de
powertop](https://en.wikibooks.org/wiki/Grsecurity/Appendix/Grsecurity_and_PaX_Configuration_Options#Deny_reading/writing_to_/dev/kmem,_/dev/mem,_and_/dev/port)


X60/T60: Port série - comment l'utiliser (pour les propriétaires d'une station d'appareillage).
===================================================

Pour le ThinkPad X60 vous pouvez utiliser la station "UltraBase X6" (pour le
X60 Tablet elle s'appelle "X6 Tablet UltraBase"). Pour le ThinkPad T60, vous
pouvez utiliser le "Advanced Mini Dock".

Si vous êtes en train d'utiliser une des images ROM avec 'serial' dans le nom,
alors vous avez le port série activé dans libreboot et memtest86+ inclut dans
la ROM. Connectez votre câble modem au port série de la station et connectez
l'autre extrémité sur un second système en utilisant votre adaptateur USB
série.

Sur le second système, vous pouvez essayez ceci, en utilisant GNU Screen):

    $ sudo screen /dev/ttyUSB0 115200

Comment quitter GNU Screen: Ctrl+A puis relâchez et pressez K, puis pressez Y.

Il y a des alternatives comme Minicom, mais j'aime GNU Screen.

En faisant ceci avant de démarrer le X60/T60, vous verrez la sortie console
produite par libreboot, ainsi que GRUB et MemTest86+. Vous pouvez aussi
configurer votre distribution afin que un terminal (TTY) est accessible depuis
la console série.

Le guide suivant est pour Ubuntu, mais devrait marcher dans Debian et
Devuan, servant à  activer une console série en utilisant GeTTY:\

The following guide is for Ubuntu, but it should work in Debian and
Devuan, to enable a serial console using GeTTY:\
<https://help.ubuntu.com/community/SerialConsoleHowto> (nous ne recommandons
PAS Ubuntu, parce qu'elle contient des logiciels non-libre dans les
répertoires par défaut. Utilisez Debian ou Devuan.

Note: une partie du tutoriel ci-dessus nécessite de changer votre grub.cfg.
Changez juste la ligne `linux` pour ajouter des options activant getty. Voyez
le document [../gnulinux/grub\_cbfs.md](../gnulinux/grub_cbfs.md).

Control fin du rétroéclairage sur les cartes graphiques Intel
=========================================

Défois la valeur du contrôle du rétroéclairage (BLC\_PWN\_CTL) définie par
Libreboot n'est pas idéale. Le résultat est soit des scintillements, qui
pourraient causer des nausées ou épilepsies ou un rétroéclaire désiquilibré
et/ou plainte de l'inverseur de l'écran. Pour corriger celà une valeur
différente pour le registre BLC\_PWM\_CTL de la carte graphique doit être
défini. Voir page 94 de 
<https://01.org/sites/default/files/documentation/g45_vol_3_register_0_0.pdf>
pour plus d'informations sur ce registre. L'outil pour définir les valeurs de
registres dans les cartes graphiques Intel est inclut dans le paquet
intel-gpu-tools. Installez-le:

    sudo apt-get install intel-gpu-tools

Vous pouvez maintenant définir des valeurs:

    sudo intel_reg write 0x00061254 votre_valeur_au_format_hexadécimal_de_C

NOTE: dans des versions plus anciennes de cet utilitaire, il faut utiliser
`intel_reg_write` à la place.

L'ensemble de valeur à la structure suivante: les bits \[31:16\] représente le
diviseur PWM. PWM / PWM\_divider = bits de fréquence \[15:0\], le nombre
réprésentant le rapport
cyclique et détermine la portion de la fréquence de modulation du
rétroéclairage. Une valeur égale à la fréquence de modulation du
rétroéclairage signifie complétement en marche/allumé. Elle ne devrait pas
être plus grande que la fréquence de modulation du rétroéclairage.

Sur des affichages avec un écran CCFL, le rétroéclairage comme de: 0x60016001.
Pour vérifier si tout les crans/modes marchent comme voulus, exécutez
`xbacklight -set 10` et augmentez jusqu'à 100. Les écrans avec un
rétroéclairage par LED nécessitent une modulation du rétroéclairage plus
basse. Faites la même chose mais en commençant de 0x01290129.
Essayez de définir différentes valeurs jusqu'à que vous trouviez une valeur
qui ne présente aucun problèmes.

Il est important de savoir qu'il y a quatre modes d'échecs:

1.  Scintillement très rapide, qui pourrait causer de l'epilepsie (la
fréquence est trop basse, réduisez le diviseur)
2.  Scintillements aléatoires à des intervalles
aléatoires - le pilote IC n'arrive pas à changer l'état du
[mosfet](https://fr.wikipedia.org/wiki/Transistor_%C3%A0_effet_de_champ_%C3%A0_grille_m%C3%A9tal-oxyde)
(fréquence trop rapide, augmentez le diviseur)
3.  La fréquence est audible et cause la plainte de l'inverseur (fréquence
trop haute, augmentez le diviseur)
4.  Le rétroéclairage n'est pas également réparti. (Spécifique au CCFL, la
fréquence est trop haute. Augmentez le diviseur).

Pour vérifier le scintillement, essayez de bouger votre ordinateur portable
pendant que vous le regardiez.

Une fréquence plus haute signifie une consommation de mémoire plus haute. Vous
voulez trouver la valeur fonctionnelle la plus haute.

Ensuite, cette valeur devrait être défini dès le démarrage: soit vous ajoutez
la ligne

    intel_reg write 0x00061254 &ltyour_ideal_value>

(NOTE: dans des versions plus anciennes de cet utilitaire, il faut utiliser
`intel_reg_write` à la place.)

avant `exit 0` dans `/etc/rc.local` ou créez un fichier de service systemd
nommé `/etc/systemd/system/backlight.service` contenant:
    [Unit]
    Description=Set BLC_PWM_CTL to a good value
    [Service]
    Type=oneshot
    RemainAfterExit=no
    ExecStart=/usr/bin/intel_reg write 0x00061254 &ltyour_value>
    [Install]
    WantedBy=multi-user.target
            
Maintenant, démarrez-le et activez le:

    sudo systemctl start backlight && sudo systemctl enable backlight

Note spéciale sur i945:

i945 se comporte de manière différente. Le bit 16 a besoin d'être à 1 et le
cycle périodique n'est mis à jour quand le rétroéclairage change. Il n'y pas
de fiches techniques disponibles sur cette cible, donc le pourquoi de ce
comportement n'est pas connu.
Donc, pour trouver une valeur fonctionnelle de BLC\_PWM\_CTL définissez le bit
16 à 1 et soyez sûr que le diviseur PWM == cycle périodique.
Voyez <https://review.coreboot.org/#/c/10624/> par rapport au bit 16.
La cause de ce problème est que i945, en contraste avec GM45, est défini pour
marcher dans le mode classique BLM. Ça rend le rétroéclairage plus compliqué
puisque le cycle périodique est dérivé de 3 registres au lieu de 2, utilisant
la formule suivante: 
    
    Si(BPC\[7:0\] <> xFF)
        alors BPCR\[15:0\] \* BPC\[7:0\]
    Sinon BPCR\[15:0\] BPC est LBB
    
Registre PCI de contrôle du rétroéclairage décrit dans
<http://www.mouser.com/pdfdocs/945gmedatasheet.pdf>, page
315. 
BPCR est BLC\_PWM\_CTL, décrit dans 
<https://01.org/sites/default/files/documentation/g45_vol_3_register_0_0.pdf>,
page 94. Plus de recherches doivent être faites sur cette cible, donc procédez
avec précautions.

Beeps de gestion de l'alimentation sur les ThinkPads
===================================

Lors de la déconnexion ou connexion du chargeur, un beep est émit. Quand la
charge de la batterie atteint un seuil critiquement bas, un beep est aussi
émit.
Nvramtool est inclut dans libreboot, et peut être utilisé pour activer ou
désactiver ce comportement.

Vous aurez besoin d'écrire les changements dans une image ROM de libreboot,
puis la flasher, afin de les appliquer. Vous pouvez soit utiliser une image
ROM pré-compilée, ou en créer une depuis celle en cours sur votre ordinateur.
Jetez un coup d'oeil ici
<https://libreboot.org/docs/gnulinux/grub_cbfs.html#get-the-rom-image> pour
plus d'informations sur comment faire ça. 

Une fois que vous avez une image ROM de libreboot, disons 'libreboot.rom',
vous pouvez écrire les changements grâce aux commandes suivantes.

Désactiver ou activer les beeps quand on enlève/met le chargeur:

    $ sudo ./nvramtool -C libreboot.rom -w power_management_beeps=Enable
    $ sudo ./nvramtool -C libreboot.rom -w power_management_beeps=Disable

Désactiver ou activer les beeps quand le niveau de batterie est faible:

    $ sudo ./nvramtool -C libreboot.rom -w low_battery_beep=Enable
    $ sudo ./nvramtool -C libreboot.rom -w low_battery_beep=Disable

Vous pouvez vérifier que les paramètres sont définis dans l'image avec:

    $ sudo ./nvramtool -C libreboot.rom -a

Finalement, vous avez besoin de flasher la ROM avec cette nouvelle image.
Voyez 
[ici](https://libreboot.org/docs/gnulinux/grub_cbfs.html#with-re-flashing-the-rom)
pour une explication détaillée

Obtenez l'EDID: trouvez le nom (modèle) de votre écran LCD
=====================================================

Obtenez le modèle de l'écran:

    sudo get-edid | strings

Ou jetez un coup d'oeil dans `/sys/class/drm/card0-LVDS-1/edid`

Alternativement vous pouvez utiliser i2cdump. Dans Debian et Devuan, c'est
dans le paquet i2c-tools.

    $ sudo modprobe i2c-dev
    $ sudo i2cdump -y 5 0x50 (vous aurez peut-être à changer la valeur pour -y)
    $ sudo rmmod i2c-dev

Vous verrez le nom de l'écran dans la sortie (depuis le cliché mémoire EDID).

Si aucune de ces options marche (ou sont indisponibles), enlever physiquement
l'écran LCD est une option. Normalement, il y aura des informations imprimées
à l'arrière.

Dépannage du pilote e1000e (NICs Intel)
===========================================

Un example d'erreur, qui peut peut-être arriver avec des schémas de routage complexes et
bizarres (citation nécessaire pour la cause de ceci):

    e1000e 0000:00:19.0 enp0s25: Detected Hardware Unit Hang

Possible solution de contournement, testée par Nazara: désactivez les
C-STATES.

*NOTE: ça désactive aussi la gestion de l'alimentation, parce que désactiver
les C-States veut dire que votre processeur s'exécutera avec ses pleines
capacités (et utilisera donc plus batterie) non-stop, ce qui drainera la vie
de la batterie si c'est un ordinateur portable. Si l'utilisation de la
batterie est une préoccupation, alors vous ne devriez pas utiliser ceci. (nous
ne sommes aussi pas sûr que ce contournement est approprié.)*

Pour désactiver les c-states, faites ceci dans une distribution GNU+Linux:

    for i in /sys/devices/system/cpu/cpu/cpuidle/state/disable;
    do
        echo 1 > $i;
    done

Vous pouvez reproduire ce problème plus facilement en envoyant beaucoup de
traffic à travers des sous-réseaux sur la même interface (NIC).

Plus d'informations, incluant des journaux, peuvent être trouvés sur [cette
page](https://notabug.org/libreboot/libreboot/issues/23).

Réveil par clavier USB sur les ordinateurs portables GM45
===================================

Jetez un coup d'oeil au script resources/scripts/helpers/misc/libreboot\_usb\_bugfix

Mettez le dans le répertoire /etc/init.d sur les systèmes basés Debian.

Copyright © 2014, 2015, 2016 Leah Rowe <info@minifree.org>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).

