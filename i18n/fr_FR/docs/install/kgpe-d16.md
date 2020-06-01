---
title: Intructions de flashage externe pour la carte KGPE-D16
x-toc-enable: true
...

Instructions initiales de flashage pour la carte KGPE-D16.

Ce guide est pour ceux voulant libreboot sur leur carte mère ASUS KGPE-D16,
pendant qu'ils ont toujours le BIOS propriétaire d'ASUS présent.
Ce guide peut être aussi suivi (adapté) si vous bousillez votre carte, pour
savoir comment la remettre sur pied.

*L'initialisation de la mémoire est encore problématique, pour certains
modules/barettes. Nous recommandons d'éviter les modules Kingston.*

Pour plus d'informations générales à propos de cette carte, référez-vous à
[../hardware/kgpe-d16.md](../hardware/kgpe-d16.md).

AFAIRE: montrer des photos ici, et d'autres infos.

Programmeur externe
===================

Référez-vous à [bbb\_setup.md](bbb_setup.md) pour un guide sur comment
configurer un programmeur SPI externe.

La puce de flash est dans un réceptable PDIP 8 (puce flash SPI) sur la carte
mère, que vous devez sortir et re-flasher avec libreboot, en utilisant le
programmeur. *NE PAS* retirer la puce avec vos mains nues. Utilisez un outil
extracteur de puces.

Copyright © 2015 Leah Rowe <info@minifree.org>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
