---
title: Comment installer NetBSD sur un système Libreboot ?
x-toc-enable: true
...

Cette section concernent la préparation, le démarrage et l'installation de 
NetBSD sur votre système Libreboot en utilisant rien de plus qu'une clef USB (et *dd*).
Ça a seulement été testé sur un ThinkPad X60 librebooté.

C'est attendu que vous utilisiez le mode texte dans libreboot (txtmode images), pour
le début du processus de démarrage dans NetBSD.*La veille/hibernation est dysfonctionnelle
d'après au moins un utilisateur.*

Grâce à ioxcide dans [ce post Reddit](https://www.reddit.com/r/BSD/comments/53jt70/libreboot_and_bsds/)
pour les instructions initiales.

*Cette section est seulement pour la charge utile GRUB. Pour depthcharge (utilisé sur les appareils
ChromeOS dans libreboot) des instructions attendent encore d'être écrites dans la documentation de libreboot.*

netbsd.iso est l'image d'installation pour NetBSD. Adaptez le nom de fichier
en accordance selon votre version de NetBSD.

Préparer la clef USB (dans NetBSD)
---------------------------------

[Cette page](https://wiki.netbsd.org/tutorials/how_to_install_netbsd_from_an_usb_memory_stick/)
sur le site web de NetBSD montre comment créer une clef USB NetBSD démarrable
depuis NetBSD même. Vous devriez utiliser la méthode *dd* documentée là-bas.

Préparer la clef USB (dans FreeBSD)
----------------------------------
[Cette page](https://www.freebsd.org/doc/handbook/bsdinstall-pre.html) sur le
site web FreeBSD montre comment créer une clef USB démarrable pour installer FreeBSD.
Utilisez le *dd* sur cette page. Vous pouvez aussi utiliser les mêmes instructions avec une
image ISO de NetBSD.

Préparer la clef USB (sur LibertyBSD ou OpenBSD)
------------------------------------------------

Si vous avez téléchargé votre ISO sur un système LibertyBSD ou OpenBSD, voici
comment créer une clef USB NetBSD démarrable:

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
Continuez à lire pour apprendre comment faire celà.

Préparer la clef USB (dans GNU+Linux)
------------------------------------

Si vous avez téléchargé votre ISO sur système GNU+Linux, voici comment créer la clef USB NetBSD démarrable:

Connectez la clef USB. Vérifiez la sortie de dmesg:

    $ dmesg

Jetez un coup d'oeil à lsblk pour voir de quel disque il s'agit:

    $ lsblk

Vérifiez qu'il n'a pas été monté automatiquement et si c'était le cas, démontez le. Par
example:
    
    $ sudo umount /dev/sdX\*
    # umount /dev/sdX\*

dmesg vous a dit de quel appareil il s'agit. Écraser en écriture le disque en écrivant
l'ISO de la distrib dessus grâce à dd. Par example:

    $ sudo dd if=install60.fs of=/dev/sdX bs=8M; sync
    # dd if=netbsd.iso of=/dev/sdX bs=8M; sync

Vous devriez être maintenant capable de démarrer l'installateur depuis votre clef USB.
Continuez à lire pour des informations sur comment faire celà.

Installer NetBSD sans le chiffrement de tout le disque
----------------------------------------------

Vous allez peut-être avoir besoin d'un clavier USB externe durant l'installation.
Tapez sur C pour avoir accès à la console GRUB.

    grub> knetbsd -r sd0a (usb0,netbsd1)/netbsd
    grub> boot

Ça fera démarrer dans l'installateur NetBSD. Suivez la procédure d'installation normale
pour NetBSD.

Installer NetBSD avec le chiffrement de tout le disque
-------------------------------------------

TODO

Démarrer
-------

Tapez sur C dans grub pour avoir accès à la ligne de commande.

    grub> knetbsd -r wd0a (ahci0,netbsd1)/netbsd
    grub> boot

NetBSD commencera à démarrer. Youpi !

Configurer GRUB
----------------

Si vous ne voulez pas allez dans la ligne de commande GRUB et taper
une commande pour démarrer NetBSD à chaque fois, vous pouvez créer un
fichierde configuration GRUB qui sera au courant de votre installation
NetBSD et qui sera utilisé automatiquement par libreboot.

Sur votre partition racine NetBSD, créez le répertoire /grub et ajoutez-y le
fichier `libreboot_grub_cfg`. Rajoutez-y ces lignes :
    
    default=0
    timeout=3

    menuentry "NetBSD" {
        knetbsd -r wd0a (ahci0,netbsd1)/netbsd
    }

La prochaine fois que vous démarreriez, vous verrez le vieux menu GRUB pour quelque 
secondes, puis vous allez voir un nouveau menu avec seulement NetBSD dans la liste. Après
3 secondes NetBSD démarrera ou vous pouvez presser Entrée pour démarrer.

Dépannage
===============

La majorité de ces problèmes arrivent lors de l'utilisation de libreboot avec le 'text mode'
de coreboot au lieu du framebuffer de coreboot. Ce mode est utile pour démarrer des charges
utiles comme memtest86+ qui s'attendent au mode texte, mais pour NetBSD celà peut être 
problématique car il essaye d'alterner sur un framebuffer qui n'existe pas.

Dans la plupart des cas, vous devriez utiliser les images ROM vesafb. Exemple du nom de fichier:
libreoot\_ukdvorak\_vesafb.rom.

Ne démarrera pas... quelque chose à propos d'un fichier non trouvé
---------------------------------------------

Vos noms d'appareils (p.e usb0, usb1, sd0, sd1, wd0, ahci0, hd0, etc) et nombre peuvent
différer. Utilisez l'autocomplétion de TAB.

Copyright © 2016 Leah Rowe <info@minifree.org>\
Copyright © 2016 Scott Bonds <scott@ggr.com>\


Permission est donnée de copier, distribuer et/ou modifier ce document 
sous les termes de la Licence de documentation libre GNU version 1.3 ou 
quelconque autre versions publiées plus tard par la Free Software Foundation 
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [fdl-1.3.md](fdl-1.3.md).
