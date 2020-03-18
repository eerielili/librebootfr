---
title: Comment installer FreeBSD sur un système Libreboot
x-toc-enable: true
...

Cette section se rapporte à la préparation, au démarrage et à l'installation
de FreeBSD sur votre système libreboot, en utilisant rien de plus qu'une clef
USB (et *dd*). Ça a été seulement testé sur un Lenovo ThinkPad x200.

C'est attendu que vous utilisiez le mode texte dans libreboot (txtmode images), pour
le début du processus de démarrage dans FreeBSD. *Démarrer l'installateur résulte en
un affichage rouge clignotant, et ne démarre pas.*

Les remerciements vont à ioxcide dans [ce billet Reddit](https://www.reddit.com/r/BSD/comments/53jt70/libreboot_and_bsds/)
pour les instructions initiales.

À FAIRE: tester FreeBSD de manière plus extensive, et s'assurer qu'il est fonctionnel (et corriger
celà le cas échéant). Les instructions sont données ici pour démarrer et installer FreeBSD, mais
nous ne sommes par sûr si oui ou non c'est en ce moment complétement compatible avec libreboot. 

* Cette section est seulement pour la charge utile GRUB. Pour depthcharge (utilisée sur
les appareils CrOs dans libreboot), les instructions attendent d'être écrite.*

freebsd.img est l'image d'installation pour FreeBSD. Adaptez concordément
le nom de fichier, pour n'importe quelle version de FreeBSD que vous utilisez.


Préparer la clef USB (dans FreeBSD)
----------------------------------

[Cette page](https://www.freebsd.org/doc/handbook/bsdinstall-pre.html) sur le
site web de FreeBSD montre comment créer une clef USB démarrable pour installer
FreeBSD. Utilisez le *dd* sur cette page.

Préparer la clef USB (dans NetBSD)
---------------------------------

[Cette page](https://wiki.netbsd.org/tutorials/how_to_install_netbsd_from_an_usb_memory_stick/)
sur le site web de NetBSD montre comment créer une clef USB NetBSD démarrable, depuis NetBSD même.
Vous devriez utilisé la méthode *dd* documentée sur cette page; vous pouvez utiliser celle-ci avec
n'importe quelle ISO (image disque), incluant FreeBSD.

Préparer la clef USB (sur LibertyBSD ou OpenBSD)
------------------------------------------------

Si vous avez téléchargé votre ISO sur un système LibertyBSD ou OpenBSD, voici
comment créer une clef USB FreeBSD démarrable:

Connectez la clef USB. Regardez la sortie de dmesg:

    $ dmesg | tail

Vérifiez et confirmer de quel USB il s'agit si vous pensez par exemple que c'est sd3:

    $ disklabel sd3

Vérifiez qu'elle n'a pas été montée automatiquement. Sinon, on l'éjecte. Par exemple :

    $ doas umount /dev/sd3i

dmesg vous a dit quel appareil/bloc c'était, donc on peut maintenant écrire par dessus
l'installateur FreeBSD grâce à dd. Par exemple :

    $ doas dd if=freebsd.img of=/dev/rsdXc bs=1M; sync

Vous devriez être maintenant capable de démarrer l'installeur depuis votre clef USB.
Continuez à lire pour apprendre comme faire ceci.

Préparer la clef USB (dans GNU+Linux)
------------------------------------

Si vous avez téléchargé votre ISO sur système GNU+Linux, voici comment créer la
clef USB NetBSD démarrable:

Connectez la clef USB. Vérifiez la sortie de dmesg:

    $ dmesg

Jetez un coup d'oeil à lsblk pour voir de quel disque il s'agit:

    $ lsblk

Vérifiez qu'il n'a pas été monté automatiquement et si c'était le cas, démontez-le. Par example:

    $ sudo umount /dev/sdX\*
    # umount /dev/sdX\*

dmesg vous a dit de quel appareil il s'agit. Écraser en écriture le disque en écrivant l'ISO de la distrib dessus grâce à dd. Par example:

    $ sudo dd if=freebsd.img of=/dev/sdX bs=8M; sync
    # dd if=freebsd.img of=/dev/sdX bs=8M; sync

Vous devriez être maintenant capable de démarrer l'installateur depuis votre clef USB.
Continuez à lire pour des informations sur comment faire celà.


Installer FreeBSD sans le chiffrement de tout le disque
-----------------------------------------------

Tapez sur C pour avoir accès à la console GRUB.

    grub> kfreebsd (usb0,gpt3)/boot/kernel/kernel
    grub> set FreeBSD.vfs.mountfrom=ufs:/dev/da1p3\
    grub> boot

Ça fera démarrer dans l'installateur FreeBSD. Suivez la procédure d'installation
normale pour FreeBSD.

Installer FreeBSD avec le chiffrement de tout le disque
--------------------------------------------

TODO

Démarrer
-------

TODO

Configurer Grub
----------------

TODO

Dépannage
===============

La majorité de ces problèmes arrivent lors de l'utilisation de libreboot avec le 'text mode'
de coreboot au lieu du framebuffer de coreboot. Ce mode est utile pour démarrer
des charges
utiles comme memtest86+ qui s'attendent au mode texte, mais pour NetBSD celà peut être
problématique car il essaye d'alterner sur un framebuffer qui n'existe pas.

Dans la plupart des cas, vous devriez utiliser les images ROM vesafb. Exemple du nom de fichier:
libreoot\_ukdvorak\_vesafb.rom.

Ne démarrera pas... quelque chose à propos d'un fichier non trouvé
---------------------------------------------

Vos noms d'appareils (p.e usb0, usb1, sd0, sd1, wd0, ahci0, hd0, etc) et nombre
peuvent
différer. Utilisez l'autocomplétion de TAB.

Copyright © 2016 Leah Rowe <info@minifree.org>\
Copyright © 2016 Scott Bonds <scott@ggr.com>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [fdl-1.3.md](fdl-1.3.md).
