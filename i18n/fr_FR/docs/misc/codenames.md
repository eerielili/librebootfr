---
title: Nom de code des produits
...

Introduction
============

Ce document liste les noms de code de produit pour certains matériels.
Notez silvouplaît que juste parce qu'un appareil est listé ici, ne veux PAS
dire qu'il est supporté par Libreboot. Pour les appareils supportés,
référez-vous aux [documents d'installation](../docs/install/).

### Une note sur les cartes graphiques

Certains ordinateurs portables sont fournis avec et sans une carte graphique
discréte (dGPU). Savoir si oui ou non une carte mère en inclut une peut être
déterminé par (ordonné de façon descendante en fonction 
de la robustesse):

-   souvent grâce à un désassemblage complet et chercherla puce en question
-   regarder s'il y a des marquages PCB blancs près des emplacements RAM/ sous
    le clavier et comparé avec certains noms de code connus (si l'autocollant
    FRU ID n'est pas disponible) listés ci-dessous.
-   parfois en regardant les grilles d'un dissipateur de chaleur: sur les
    ordinateurs portables avec une carte graphique discrète, celles-ci
    apparaîtront orange et sur ceux qui ont la carte graphique intégré, argenté.

Liste des modèles et noms de code
============================

### Noms de code 

-   Asus Chromebook C201PA: speedy\_rk3288, veyron-speedy

-   ThinkPad X60: KS Note
-   ThinkPad X60s (slim): KS Note-2 / KS-2
-   ThinkPad X60 Tablet: Dali (Same PCB as KS Note-2, different EC firmware)

-   ThinkPad X200: Mocha-1
-   ThinkPad X200s (slim): Pecan-1
-   ThinkPad X200 Tablet: Caramel-1

-   ThinkPad R400/T400: Malibu-3
    -   avec carte graph. discrète (dGPU), carte révision 0: "MLB3D-0
    -   avec seulement carte graph. intégrées (iGPU), carte révision 0: "MLB3I-0"

-   ThinkPad T500/W500: Coronado-5
    - avec dGPU (radeon): "COR5D-0" (le dernier chiffre est la révision de la
      carte)
    - avec seulement l'iGPU: "COR5I-0"

-   ThinkPad T400s (slim): Shinai-MV
-   ThinkPad R500: Waikiki-3

-   T6x (famille entière): Davinci. 
Ils n'ont pas de label dans la sérigraphie donc vous avez besoin d'utiliser le
label FRU de la carte mère, qui est placé sous les barrettes de RAM.
-   T60:
    -   avec dGPU (radeon): Magi-0 (le dernier chiffre est la révision de la
        carte graph.)
    -   avec iGPU: Lisa-0

-   R60(e): RP-1, RP-2 - Rockwell / Picasso

-   Avec des ThinkPads sur les plateformes Intel plus récentes que Montevina
    (excepté le T410), les noms de codes deviennent plus consistants. Toutes
    les cartes ont le suivant suffixé dépendant quel type de graphique elles
    ont:
    -   avec dGPU: SWG (*SWitchable Graphics*, graphiques échangeables)
    -   avec seulement iGPU: UMA (*Unified Memory Access*, Accés unifié à la
        mémoire)

*Notez que les plateformes Intel plus récentes que Montevina ne sont pas
encore supportées par Libreboot!. En ce moment, seulement les plateformes
Calistoga et Montevina sont supportées.*
-   Ceux-ci sont les noms de codes connus des modèles:
    -   ThinkPad T410: NOZOMI-1 # EXT/INT
    -   ThinkPad T410s: SHINAI-2 # SWG/UMA
    -   ThinkPad T420: NOZOMI-3 # SWG/UMA
    -   ThinkPad T420s: SHINAI-3 # SWG/UMA
    -   ThinkPad T430: NOZOMI-4 # SWG/UMA
    -   ThinkPad T430s: SHINAI-4 # SWG/UMA
    -   ThinkPad T520: KENDO-1
    -   ThinkPad W520: KENDO-1 WS
    -   ThinkPad T520: KENDO-3
    -   ThinkPad W520: KENDO-3 WS
    -   ThinkPad T530: KENDO-4
    -   ThinkPad W530: KENDO-4 WS


### Divers
-   [Calistoga](https://ark.intel.com/products/codename/5950/Calistoga):
nom de famille du jeu de puces 945GM/945PM 
-   Napa: plateforme basée sur Calistoga
-   [Cantiga](https://ark.intel.com/products/codename/26552/Cantiga):
nom de famille du jeu de puces GM45/GS45/PM45.
    C'est le jeu de puces utilisé dans le T400,X200 et similaire.
-   Montevina: plateforme basée sur Cantiga.
-   PMH: le Power Management Hub est une puce préfabriquée pour gérer
    la séquence ouverte/fermée de l'alimentation. Il est additionnelement
    demandé pour étendre l'entrée/sortie du contrôleur embarqué (EC).
    Sa version plus récente était nommée "Thinker-1", et éventuellement a été
    fusionnée avec le PMIC (Rinkan) en tant que "ThinkEngine" (ne le confondez
    pas avec la puce EC qui a aussi le logo ThinkEngine sur les cartes mères
-   Kozak, Tsurumai, Rinkan: ce sont les versions successives de la gestion de
    l'alimentation IC pour les Notebook. Le marquage de la puce Tsurumai est
    "TB62501F" et sa fiche technique décrit entiérement son fonctionnement.

Voir aussi
========

-   Bien d'autres noms de code d'Intel peuvent être trouvés sur 
    [Wikipedia](https://en.wikipedia.org/wiki/List_of_Intel_codenames).
-   Pour les ThinkPads, voyez [Documentation/thinkpad/codenames.csv @ Coreboot]
(https://review.coreboot.org/cgit/coreboot.git/tree/Documentation/thinkpad/codenames.csv)

Copyright © 2018 Fedja Beader <fedja@protonmail.ch>

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
