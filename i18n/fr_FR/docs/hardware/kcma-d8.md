---
title: Carte mère de PC bureau/station de travail ASUS KCMA-D8 
...

C'est une carte mère de bureau support du matériel AMD (processeurs Fam10h *et
Fam15h* disponibles). Elle peut être aussi utilisé pour construire une station
de travail hautement performantes.
Fonctionnant sous Libreboot.
L'adaption de coreboot a été faite par Timothy Pearson de l'entreprise Raptor
Engineering, et , nous travaillant avec eux, fusionné dans Libreboot.

L'initialisation de la mémoire est encore problématique, pour quelques
barettes. Nous recommandons d'éviter les modules Kingston.*

Les instructions de flashage peuvent être trouvée dans
[../install/\#flashrom](../install/#flashrom) - notez que le flashage externe
est requis (p.e. BBB), si le micrologiciel propriétaire d'ASUS est installé
sur le moment. Si vous avez déjà libreboot, il est par défaut possible de
re-flasher par voie logicielle depuis GNU+Linux sur kcma-d18, sans utiliser de
matériel extérieurs.*

Compatilibilité des processeurs
=================
*Utilisez l'Opteron 4200 series (marchent sans mises à jour du microcode,
incluant la virtualisation matérielle)*.

Le 4300 series a besoin de mises à jour du microcode, donc éviter ces CPUs.
Les 4100 séries sont trop vieux, et majoritairement non testés.

Status de la carte mère (niveau compatibilité) {#boardstatus}
============================

Voyez <https://raptorengineeringinc.com/coreboot/kcma-d8-status.php>.

Facteur de forme / format {#formfactor}
===========

Cette carte mère utilise le format ATX. Bien que la [version 2.2 du standard
ATX](https://web.archive.org/web/20120725150314/http://www.formfactors.org/developer/specs/atx2_2.pdf)
spécifié des dimensions de carte mères de 305mm x 244mm, cette carte mesure
305mm x 253mm; assurez-vous que votre boîtier supporte ces centimètre  de plus
en largeur.

Extension IPMI iKVM {#ipmi}
=======================

Ne l'utlilisez pas. Ça utilise du micrologiciel propriétaire et ajoute une
porte dérobée (une puce d'administration hors bandes, similaire à l'[Intel
Management Engine](../../faq.md#intelme).
Heuresement, le micrologiciel est non signé (possiblement pour faciliter le
remplaçage) et physiquement séparé, puisque c'est une extension que vous
n'avez pas à installer.

Puces de flashage {#flashchips}
===========

Les puces de flashage de 2Mo sont inclues par défaut sur ces cartes mères.
C'est sur un P-DIP à 8 emplacements (puce SPI).
La puce flash peut être amélioré vers une plus grande taille :
4Mo, 8Mo ou 16Mo.
Avec au moins 8Mo, il est faisaible que vous faites rentrer une image
compressé linux+initramfs (système Linux+BusyBox) dans CBFS et démarrer sur
ça, se chargeant ensuite dans la mémoire.

Libreboot a des configurations pour des tailles de puces de 2, 4, 8 et 16Mo (
la puce flash par défaut fait 2Mo).

*NE RETIREZ PAS la puce avec vos mains nues. Utilisez un extracteur de puce
P-DIP 8. Ceux-ci s'achètent en ligne: Jetez un coup d'oeil à
<http://www.coreboot.org/Developer_Manual/Tools#Chip_removal_tools>*

Initialisation natives des graphiques {#graphics}
==============================

Seulement le mode texte est connu pour marcher, mais le kernel(linux) peut
initialiser le tampon d'affichage (si KMS est activé

Only text-mode is known to work, but linux(kernel) can initialize the
framebuffer display (if it has KMS - kernel mode setting).

Problèmes en ce moment {#issues}
==============

-   Les modules mémoires LRDIMM sont pour le moment incompatibles.
-   Le SAS (SCSI, via le module PIKE 2008) nécessite une ROM (et SeaBIOS)
    optionnelle et propriétaire pour démarrer dessus (théoriquement possible
    de remplacer, mais vous pouvez mettre un kernel dans CBFS ou sur SATA et
    utilisez ça pour démarrer sur GNU, qui peut marcher sur un disque SAS. Le
    kernel linux peut utiliser ces disques SAS (via le module PIKE sans une
    ROM optionnelle).


-   Le module IPMI iKVM (extension de carte optionnelle) utilise un
    micrologiciel propriétaire. Puisque c'est pour une administration
    hors-bande, c'est théoriquement une porte dérobée similaire similaire à
    l'Intel Management Engine. Heuresement, contrairement à la ME, ce
    micrologiciel n'est pas signé voulant dire qu'un remplacement par du libre
    est théoriquement possible.
    Pour l'instant, le project libreboot recommande de ne pas installer le
    module. [Ce projet](https://github.com/facebook/openbmc) pourrait être
    intéressant en tant que base sur laquelle dériver pour ceux voulant
    travailler sur un remplacement libre.
    En pratique, l'administration hors-bande n'est pas très utile de toute
    façon (ce n'est pas un handicap de ne pas l'avoir).
-   Graphiques: seulement le mode texte marche. Jetez un coup à la section
    [\#graphique](#graphics)

Caractéristiques techniques {#specifications}
-----------------------

[Jetez un coup d'oeil sur le site web d'ASUS](https://www.asus.com/Commercial-Servers-Workstations/KCMAD8/specifications/).

Copyright © 2016 Leah Rowe <info@minifree.org>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
