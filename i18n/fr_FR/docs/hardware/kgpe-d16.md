---
title: ASUS KGPE-D16 carte mère serveur / station de travail 
...

C'est une carte mère de serveur utilisant du matériel d'AMD (CPUs Fam10h
*et Fam15h* disponible). Ça peut être aussi utilisé pour construire
une station de travail hautes performances.
Marchant avec Libreboot. L'adaptation de coreboot a été réalisée par
Timothy Pearson de chez Raptor Engineering Inc. et, travaillant avec eux
(et en étant un sponsor), fusionné dans libreboot.

*L'initialisation de la mémoire est encore problématique pour certaines
barettes. Nous recommandons d'éviter les barettes Kingston*
*Pour des configurations fonctionnelles jetez un coup d'oeil à 
<https://www.coreboot.org/Board:asus/kgpe-d16>.*

Les instructions de flashage peuvent être trouvées dans
 [../install/\#flashrom](../install/#flashrom) - noter que le 
flashage externe est requis (p.e. BBB), si le micrologiciel 
propriétaire (d'ASUS) est installé en ce moment.
Si libreboot est déjà installé, par défaut il est possible
de re-flasher en utilisant des logiciels s'exécutant dans GNU+Linux
sur le KGPE-D16, sans utiliser de matériels externes.

Compatilibité du CPU
=================
*Utilisez l'Opteron 6200 series (marche sans mises à jour du microcode,
incluant l'accélération matérielle de la virtualisation).*
Les 6300 series ont besoin de mises à jour du microcode, donc évitez-les.
Les 6100 series sont trop vieux, et pour la plupart non testés.

Status de compatibilité de la carte mère {#boardstatus}
============================

Voyez <https://raptorengineeringinc.com/coreboot/kgpe-d16-status.php>.

Dimensions / facteur de forme {#formfactor}
===========

Ces cartes mères utilisent le factor de forme SSI EEB 3.61; soyez sûr
que votre boîtier est adapté. Ce facteur de forme est similaire à E-ATX
en ce que la taille est identique, mais la position des vis sont différentes.

Module IPMI iKVM {#ipmi}
=======================

Ne l'utilisez pas. Ça se sert d'un micrologiciel propriétaire et 
rajoute une porte dérobée (puce d'administration à distance hors fréquence, similaire
à l'[Intel Management Engine](../../www/faq.md#intelme)).
Heuresement, le micrologiciel n'est pas signé (probablement pour remplacer) et
séparé physiquement de la carte mère puisqu'il est sûr le module, que vous n'avez
pas à installer.

Puces de flash {#flashchips}
===========

Les puces de flash de 2Mo sont inclues par défaut sur ces cartes mères. C'est
sur un P-DIP à 8 compartiments (puce SPI). La puce de flash peut être amélioré
à une taille plus grande: 4Mo, 8Mo ou 16Mo. Avec au moins 8Mo, vous pourrez
aisément rentrer une image linux linux+initramfs (BusyBox+Système Linux) dans
CBFS et démarrer ça, puis le charger en mémoire.

Libreboot a des configurations pour des puces de flashes de taille 2, 4, 8 et
16Mo (la taille par défaut est 2Mo).

*NE CHANGEZ PAS À CHAUD la puce avec vos mains nues. Utilisez un extracteur
de puce P-DIP à 8 emplacements. Voyez
<http://www.coreboot.org/Developer_Manual/Tools#Chip_removal_tools>*

Initialisation des graphiques natif {#graphics}
==============================

Seulement le mode texte est connu pour marcher, mais linux (le kernel) peut
initialiser l'affichage du tampon d'image (si il a le paramètrage de mode -
Kernel Mode Setting, ou KMS - activé).

Problèmes actuels {#issues}
==============

-   Les modules de mémoires LRDIMM sont incompatibles pour le moment.
-   L'interface SAS (appelée aussi SCSI) requiert une ROM optionnelle
    propriétaire (et SeaBIOS) pour y démarrer depuis (c'est théoriquement
    possible à remplacer, mais vous pouvez mettre un kernel dans CBFS ou sur
    SATA et utiliser ça pour démarrer GNU, qui peut résider sur une disque dur
    SAS. Le kernel linux peut utiliser ces disques durs (via le module PIKE)
    sans une ROM optionnelle).
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

Détails du matériel {#specifications}
-----------------------

L'information ici est reprised depuis le site web d'ASUS.

### Processeur / bus système

-   2 réceptacles processeurs (G34 compatible)
-   HyperTransport™ Technology 3.0
-   Processeurs supportés:
    -   AMD Opteron 6100 series (Fam10h. Pas de support d'IOMMU. *Non*
        recommandée - vieille. Voyez la fiche d'errata ici:
        <http://support.amd.com/TechDocs/41322_10h_Rev_Gd.pdf>)
    -   AMD Opteron 6200 series (Fam15h, avec support complet d'IOMMU dans
        libreboot - *hautement recommandée - rapide, et marchent bien sans
        mises à jour du microcode.*
    -   AMD Opteron 6300 series (Fam15h, avec support complet d'IOMMU dans
        libreboot. *ÉVITER ÇA COMME LA PESTE - la virtualisation est cassée
        sans les mises à jour du microcode.*
    -   À NOTER: les processeurs série 6300 ont du microcode bugué intégré de
        base, et donc libreboot recommande d'éviter ces mises à jour. Les
        processeurs série 6200 ont un microcode plus stable. Voyez cette fiche
        technique:
        <http://support.amd.com/TechDocs/48063_15h_Mod_00h-0Fh_Rev_Guide.pdf>
        (regardez l'Errata 734 - c'est ce qui tue le 6300 series)
-   6.4 GT/s per link (triple link)

### Logique principale

-   AMD SR5690
-   AMD SP5100

### Compatibilité de la mémoire (avec libreboot)

-   *Emplacements totaux:* 16 (4 canal par CPU, 8 DIMM par CPU), ECC
-   *Capacité:* Maximum va jusqu'à 256Go RDIMM (le maximum testé est 128Go
    pour le moment).
-   *Types de mémoire compatible:*
    -   DDR3 1600/1333/1066/800 UDIMM\*
    -   DDR3 1600/1333/1066/800 RDIMM\*
-   *Taille compatible par barette mémoire:*
    -   16GB, 8GB, 4GB, 3GB, 2GB, 1GB RDIMM
    -   8GB, 4GB, 2GB, 1GB UDIMM

### Emplacements d'extensions 

-   *Total des emplacaments:* 
-   *Emplacement 1:* PCI 32bit/33MHz
-   *Emplacement 2:* PCI-E x16 (lien Gen2 X8)
-   *Emplacement 3:* PCI-E x16 (lien Gen2 X16 ), bascule automatiquement sur
    le lien x8
     si l'emplacement 2 est occupé.
-   *Emplacement 4:* PCI-E x8 (lien Gen2 X4 )
-   *Emplacement 5:* PCI-E x16 (lien Gen2 X16 )
-   *Emplacement 6:* PCI-E x16 (lien Gen2 X16 ), s'éteint automatiquement si
    le l'emplacement 5 est occupé, pour les cartes 1U FH/FL , MIO supporté
-   *Emplacement addtionnel 1!* Emplacement PIKE (pour les disques SAS. Voyez
    les notes ci-dessus)
-   Follow SSI Location\#

### Format {#form-factor}

-   SSI EEB 3.61 (12"x13")

### Fonctionnalitées ASUS 

-   Fan Speed Control
-   Rack Ready (Rack and Pedestal dual use)

### Stockage 

-   *controlleur SATA :*
    -   AMD SP5100
    -   6 x SATA2 300Mo/s
-   *SAS/SATA Controlleur:*
    -   ASUS PIKE2008 3Go/s avec carte SAS 8 ports incluse.

### Réseau 

-   2 x Intel® 82574L + 1 x Mgmt LAN

### Graphiques

-   Aspeed AST2050 with 8MB VRAM

### Entrée/Sortie inclus carte mère 

-   1 x connecteur de bloc d'alimentation (connecteur d'alimentation 12V SSI
    24-pin + un connecteur d'alimentation 12V SSI 8-pin ).
    )
-   1 x connecteur d'administration, réceptacle intégré pour carte
    d'aministration
-   3 x [pin en-tête](https://en.wikipedia.org/wiki/Pin_header) USB, jusqu'à 6
    appareils
-   1 x Port USB interne Type A
-   8 x en-tête de ventialeurn, 4 pins (support de deux ventilateur
    3pin/4pin)
-   2 x SMBus
-   1 x En-tête port série
-   1 x En-tête TPM
-   1 x port Clavier/Souris PS/2

### Port d'entrée/sorties à l'arrière

-   1 x Port série externe
-   2 x Port USB externe
-   1 x Port VGA
-   2 x RJ-45
-   1 x Clavier/Souris PS/2

### Environement

-   *Températures opérationnelles:* 10C \~ 35C
-   *Températures de dysfonctionnement:* -40C \~ 70C
-   *Taux d'humidité dysfonctionnels:* 20% \~ 90% ( Non condensée)

### Surveillance et contrôle

-   Températures des processeurs
-   Vitesse des ventilateurs (RPM)

### Note:

-   \* Les DDR3 1600 ne peuvent être seulement supportés qu'avec un processeur
    AMD Opteron 6300/6200 series.

Copyright © 2015 Leah Rowe <info@minifree.org>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
