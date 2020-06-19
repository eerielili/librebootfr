---
title: charge utile GRUB
x-toc-enable: true
...

Cette section concerne la charge utile GRUB utilisée dans Libreboot.

Changer l'image de fond dans GRUB
=====================================

Utilisez cbfstool du répertoire libreboot\_util ou
libreboot\_src/coreboot/util/cbfstool si vous voulez le compiler depuis le
code source.

    $ ./cbfstool yourrom.rom remove background.png -n background.png
    $ ./cbfstool yourrom.rom add -f background.png -n background.png -t raw

Quand vous avez fait ça, reflashez votre ROM et vous devriez avoir un nouveau
fond d'écran au démarrage.

Définir la police d'écriture dans GRUB (à titre d'exemple)
====================================

Vous n'avez pas besoin de faire ça à moins que vous voulez changer la police
par défaut vous-même. (c'est juste à titre d'exemple. C'est déjà fait de base
pour vous)

L'ancienne police d'écriture utilisée était Unifont, et elle avait quelques
charactères manquants; par exemple, la bordure montrait des charactères '???'
au lieu de lignes.

J'ai essayé la police DeJavu Sans Mono de ce site web:
[dejavu-fonts.org](http://dejavu-fonts.org/wiki/Download)

Spécifiquement, la version que j'ai choisi était la dernière au temps de
l'écriture (Samedi 21 Juillet 2014): [celle ci](http://sourceforge.net/projects/dejavu/files/dejavu/2.34/dejavu-fonts-ttf-2.34.tar.bz2)

C'est une police libre qui est aussi incluse dans des distributions GNU+Linux
tel que Debian, Devuan ou Parabola.

    $ cd libreboot\_src/grub

Compilez GRUB (les scripts de compilation aident à comprendre comment faire
ceci)\ puis revenez dans libreboot\_src/ressources/grub:

    $ cd ../libreboot\_src/resources/grub/font

J'ai pris Dejavu Sans Mono de dejavu (inclus dans cette version de libreboot)
et j'ai exécuté:

    $ ../../../grub/grub-mkfont -o dejavusansmono.pf2 dejavu-fonts-ttf-2.34/ttf/DejaVuSansMono.ttf

J'ai ensuite ajouté les instructions au script 'gen.sh' dans grub-assemble
pour inclure resources/grub/dejavusansmono.pf2 dans toutes les images ROM, et
la racine du disque mémoire GRUB.\
J'ai ensuite utilisé les instructions aux fichiers grub.cfg (pour charge les
polices):

    loadfont (memdisk)/dejavusansmono.pf2

Dispositions de clavier GRUB (à titre d'exemple)
=====================================

Disposition de clavier customisée dans GRUB (à titre d'exemple)
----------------------------------------------

Les mappages de touche sont storées dans
resources/utilities/grub-assemble/keymap/.
Exemple (Azerty français):

    $ ckbcomp fr > frazerty

Allez dans le répertoiregrub:

    $ cat frazerty | ./grub/grub-mklayout -o frazerty.gkb

Vous devez être sûr que les fichiers sont nommés keymap et keymap.gkb (où
`keymap` peut être ce que vous voulez).

Puis ensuite à partir de l'exemple ci-dessus, vous pouvez mettre `frazerty`
dans `resources/utilities/grub-assemble/keymap/original/` et le fichier
`frazerty.gkb` va dans `resources/utilities/grub-assemble/keymap/`.

Les scripts de compilation voit ceci automatiquement, et compilent
automatiquement les images ROM avec votre disposition customisé (avec nom
donné) et les incluent sous le dossier `bin`. Exemple:
`libreboot_frazerty.rom`.

Disposition de clavier UK Dvorak dans GRUB (à titre d'exemple)
-------------------------------------------------

ukdvorak a du être créé manuellement, basé sur usdvorak. Faites une diff (les
fichiers sont sous resources/utilities/grub-assemble/keymap/original) pour
voir comment le fichier ukdvorak a été créé

    $ cat ukdvorak | ./grub/grub-mklayout -o ukdvorak.gkb

Copyright © 2014 Leah Rowe <info@minifree.org>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
