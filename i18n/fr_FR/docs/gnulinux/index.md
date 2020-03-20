---
title: Instructions d'installations de distributions GNU+Linux
...

Cette sections explique comment se débrouiller avec des systèmes d'exploitations variés (GNU+Linux and non-GNU+Linux) dans Libreboot (e.g. Créer des clef USB démarrables, Installer des systèmes d'exploitations, Changer le menu par défaut de GRUB, etc.).

**NOTE: Cette section est seulement pour la charge utile GRUB. Pour la charge utile depthcharge (utilisée sur les appareils CrOS, comme le Chromebook ASUS C201), des instructions sont encore à écrire.**

Libreboot utilise la charge utile GRUB par défaut, ce qui veut dire que le fichier de configuration GRUB (d'où le menu GRUB apparait) est stocké directement aux côtés de Libreboot et sa charge utile exécutable GRUB, dans la puce de flashage.
Dans le contexte, celà veut dire qu'installer des distributions et les administrer est légérement différent comparé à un système BIOS traditionnel.

Dans la majorité des systèmes, **/boot** (le dossier qui contient tout les fichiers nécessaires pour le démarrage de votre système d'exploitation) doit être sur sa propre partition et non chiffré (alors que les autres partitions sont chiffrées); c'est pour celà que GRUB (et donc le kernel) peut être chargé et exécuté puisque un micrologiciel traditionnel ne peut pas ouvrir un volume LUKS.

Cependant avec Libreboot, GRUB est déjà inclus en tant que charge utile donc même *boot* peut être chiffré : celà protège **/boot** des altérations apportés par quelqu'un qui aurait un accès physique à la machine.

- [Comment préparer et démarrer un installateur USB dans les systèmes Libreboot](grub_boot_installer.md)

- [Modifier la configuration GRUB dans les systèmes Libreboot](grub_cbfs.md)

- [Distribution Guix avec chiffrement du disque en entier sur Libreboot](guix_system.md)

- [Installer Parabola ou Arch GNU+Linux-libre, avec chiffrement du disque en entier (incluant /boot)](encrypted_parabola.md)
  
  - Tutoriel suivant [Configurer Parabola (Après l'installation)](configuring_parabola.md)

- [Installer Hyperbola GNU+Linux, avec chiffrement du disque en entier (incluant /boot)](https://wiki.hyperbola.info/en:guide:encrypted_installation)

- [Installer Trisquel GNU+Linux-Libre, avec chiffrement du disque en entier (incluant /boot)](encrypted_trisquel.md)

- [Installer Debian ou Devuan GNU+Linux-Libre, avec chiffrement du disque en entier (incluant /boot)](encrypted_debian.md)

- [Comment renforcer en sécurité votre configuration GRUB](grub_hardening.md)


Fedora ne veut pas démarrer ?
------------------

Ce qui suit peut s'appliquer aussi à CentOS ou Redhat.
Quand vous utilisez la configuration GRUB par défaut de Libreboot, et que libreboot-grub utilise
le grub.cfg (dans /boot/grub2/grub.cfg) de Fedora, Fedora utilise par défaut la commande
`linux16`, alors que ça devrait être `linux`

Faites ceci dans Fedora:

- Ouvrez `/etc/grub.d/10_linux`

- Mettez la variable `sixteenbit` à jour avec une chaîne de charactères vide puis exécutez :

   grub2-mkconfig -o /boot/grub2/grub.cfg


Copyright © 2014, 2015 Leah Rowe <info@minifree.org>

Copyright © 2017 Elijah Smith <esmith1412@posteo.net>

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
