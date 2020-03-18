---
title: How to install LibertyBSD or OpenBSD on a libreboot system
x-toc-enable: true
...

NOTE: Ce guide a été écrit pour OpenBSD par la personne qui y a contribué, mais
le projet libreboot recommande LibertyBSD. LibertyBSD est une version d'OpenBSD 
sans logiciel propriétaire dans les dépôts logiciels (OpenBSD distribue des blobs de micrologiciels 
pour des périphériques dans son kernel). Allez sur le [site web de LibertyBSD](http://libertybsd.net/)
-- À FAIRE: tester sur LibertyBSD et donner la priorité aux explications dans ce guide.

Cette section concerne la préparation, le démarrage et l'installation d'OpenBSD
sur votre système libreboot en utilisant rien de plus qu'un clef USB (et `dd`).
Ça a seulement été testé sur un ThinkPad X200 de chez Lenovo.

*Cette section est seulement pour la charge utile GRUB. Pour depthcharge (utilisé
sur les appareils CrOS dans libreboot), des instructions ont encore à être écrite
dans la documentation libreboot.*

install61.fs est l'image d'installation pour OpenBSD 6.1. Adaptez le nom du
fichier en concordance avec ce que vous avez, que ce soit pour une version
différente d'OpenBSD ou LibertyBSD.

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

Continuez à lire pour des informations sur comment faire celà.


Préparer la clef USB (dans NetBSD)
---------------------------------

[Cette page](https://wiki.netbsd.org/tutorials/how_to_install_netbsd_from_an_usb_memory_stick/)
sur le site web de NetBSD montre comment créer une clef USB NetBSD démarrable, depuis NetBSD même.
Vous devriez utilisé la méthode *dd* documentée sur cette page; vous pouvez utiliser celle-ci avec
n'importe quelle ISO (image disque), incluant FreeBSD.


Préparer la clef USB (dans FreeBSD)
----------------------------------

[Cette page](https://www.freebsd.org/doc/handbook/bsdinstall-pre.html) sur le
site web de FreeBSD montre comment créer une clef USB démarrable pour installer
FreeBSD. Utilisez le *dd* sur cette page.


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

Installer OpenBSD sans le chiffrement de tout le disque
-----------------------------------------------

Pressez C dans GRUB pour avoir accés à la ligne de commande:

    grub> kopenbsd (usb0,openbsd1)/6.1/amd64/bsd.rd
    grub> boot

Ça commencera le démarrage sur l'installateur d'OpenBSD. Suivez la
procédure normale pour installer OpenBSD.

Installer OpenBSD avec le chiffrement de tout le disque
--------------------------------------------

Ne marche pas. Vous pouvez modifier la procédure ci-dessus (installation
sans chiffrement pour installer OpenBSD avec le chiffrement de tout le disque, et
ça semble marcher mais il n'est pas encore très clair de savoir comment *démarrer* 
une installation OpenBSD+CDE(Chiffrement du Disque en Entier) en utilisant libreboot+GRUB2.
Si vous réusissez à le faire marcher, merci de nous le faire savoir.

Si vous démarrez en mode texte (le mode tampon d'image, aussi appelé framebuffer, peut
aussi marcher), il serait possible de chainload le chargeur d'amorçage d'OpenBSD ou LibertyBSD 
depuis la section MBR sur le périphérique de stockage interne. De cette façon, il serait possible
de démarrer avec une installation OpenBSD ou LibertyBSD chiffrée. Merci de nous le faire savoir 
(les informations de contact sont sur la page d'accueil de libreboot) si vous arrivez à le faire
marcher de cette façon.

Alternativement, il serait bon de porter OpenBSD soit nativement en tant qu'une charge
utile coreboot, ou de le porter dans libpayload (bibliothèque de charge utile dans coreboot;
elle a une bibliothèque C basique et quelques fonctions pour certaines opérations p.e. texte/bitmap).
*Celà serait ideal parce qu'alors il serait possible de démarrer de vraiment
démarrer une installation OpenBSD ou LibertyBSD entièrement chiffrée, en mettant
tout dans la puce de flashage.*

Alternativement, modifier GRUB pour supporter le démarrage d'installations
OpenBSD entièrement chiffrées serait possible, mais probablement pas faisable; 
c'est une base de code totalement étrangère au projet OpenBSD, pas étroitement
intégrée et le chargeur d'amorçage d'OpenBSD marche déjà.

Démarrer
-------

Tapez C dans GRUB pour accéder à la ligne de commande :

    grub> kopenbsd -r sd0a (ahci0,openbsd1)/bsd
    grub> boot

Et OpenBSD démarrera. Hourra !

Configurer Grub
----------------

Si vous ne voulez pas rentrer dans la console GRUB et taper
une commande pour démarrer OpenBSD à chaque fois, vous pouvez créer un
fichier de configuration GRUB qui sera au courant de votre installation
OpenBSD et qui sera utilisé automatiquement par libreboot.

Sur votre partition racine NetBSD, créez le répertoire `/grub` et ajoutez-y le
fichier `libreboot_grub_cfg`. Rajoutez-y ces lignes :
    
    default=0
    timeout=3

    menuentry "OpenBSD" {
        kopenbsd -r wd0a (ahci0,openbsd1)/openbsd
    }

Si votre installation OpenBSD utilise une table de partition GPT, utilisez la
partition `gpt4` à la place de `openbsd1`.

La prochaine fois que vous démarreriez, vous verrez le vieux menu GRUB pour quelque 
secondes, puis vous allez voir un nouveau menu avec seulement OpenBSD dans la liste. Après
3 secondes OpenBSD démarrera ou vous pouvez presser Entrée pour démarrer.

Dépannage
===============

La majorité de ces problèmes arrivent lors de l'utilisation de libreboot avec le 'text mode'
de coreboot au lieu du framebuffer de coreboot. Ce mode est utile pour démarrer
des charges
utiles comme memtest86+ qui s'attendent au mode texte, mais pour OpenBSD celà peut être
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
