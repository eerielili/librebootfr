---
title: Charge utile deptcharge
x-toc-enable: true
...

Cette section concerne la charge utilise depthcharge utilisée dans libreboot.

Modèle de sécurité CrOS
===================

Les appareils CrOS (Système d'exploitation Chromium/Chrome) tel que les 
Chromebooks,  implémentent un modèle de sécurité strict pour s'assurer
que ces appareils ne sont pas compromis, ce modèle est implémenté en tant que
démarrage vérifié de référence (vboot), dont la majorité est exécuté dans
depthcharge. Un vue d'ensemble détaillé du modèle de sécurité CrOS est
disponible sur la page dédiée.

Malgré le modèle de sécurité CrOS, depthcharge ne permettra pas de démarrer
des kernels sans vérifier leur signature et de démarrer sur des médium
externes ou charges utiles classique, sauf si explicitement autorisé.
Voyez la [configuration des paramètres de la vérification au démarrage.](#configuring_verified_boot_parameters).

Écran du mode développeur
=====================

L'écran du mode développeur peut être accédé dans depthcharge quand le mode
est activé. Il peut l'être depuis l'écran du mode récupération/dépannage.

Ça permet de démarrer normalement, de démarrer depuis le stockage interne, de
démarrer à partir d'un média externe (p.ex USB) (quand activé), de démarrer
depuis une charge utile classique (quand activé), de montrer des informations
à propos de l'appareil et de désactiver le mode développeur.

Maintenir l'écran du mode développeur
---------------------------------

Comme indiqué sur l'écran du mode développeur, l'écran peut être maintenu en
pressant *CTRL + H* dans les 3 premières secondes après que l'écran apparait.
Après ce délai, depthcharge démarrera normalement.

Démarrer normalement
----------------

Comme indiqué sur l'écran du mode développeur, un démarrage normal se fera
après 3 secondes (si l'écran du mode développeur n'est pas maintenu).

Le type du média de démarrage par défaut (stockage interne, média externe, charge
utile classique) est montré à l'écran.

Démarrer depuis différents médiums
------------------------------

Quand ils sont autorisés, depthcharge permet le démarrage depuis différents
médium (voyez la
[configuration des paramètres de la vérification au démarrage.](#configuring_verified_boot_parameters)
pour activer pour désactiver ces médiums de démarrage).

Comme indiqué sur l'écran du mode développeur, démarrer à partir de médiums
variés peut être enclenché grâce à des combinaisons de touches:

-   Stockage interne: *Ctrl + D*
-   Média externe: *Ctrl + U* (quand activé)
-   Charge utile classique: *Ctrl + L* (quand activé)

Montrer les informations de l'appareil
--------------------------

Comme indiqué sur l'écran du mode développeur, montrer des informations sur
l'appareil peut être enclenché en pressant *Ctrl + I* ou *Tab*. Des
informations variées sont montrées, comprenant les données non-volatile de
vboot, statut TPM, drapeaux GBB et hashs/somme de contrôles des clés.

Avertissements
--------

L'écran du mode développeur montrera des avertissements quand:

-   Démarrer des kernels sans vérifier leur signature est activé
-   Démarrer depuis un média externe est activé
-   Démarrer les charges utiles classique est activé

Écran du mode de recouvrement/dépannage
====================

L'écran du mode de recouvrement/dépannage peut être accéder dans deptcharge,
en pressant *Échap + Rafraîchir + Bouton Marche/Arrêt* quand la machine est
éteinte.

Ça permet de recouvrir la machine d'un état corrompu/erroné en démarrant sur
un média de récupération de confiance.
Quand accédé alors que la machine est dans un bon état, ça permet aussi
d'activer le mode développeur.

Recouvrir d'un mauvais état
---------------------------

Quand l'appareil échoue la vérification de la signature d'une partie du
logiciel de démarrage ou quand une erreur arrive, il est considéré être dans
un mauvais état et demandera à l'utilisateur de redémarrer sur le mode de
récupération.

Le mode de récupération démarre seulement le logiciel situé dans la mémoire
protégée en écriture, qui est considérée de confiance et sécurisée.

Le mode de récupération permet alors de recouvrir/dépanner l'appareil en
démarrant d'un média externe de confiance, qui est automatiquement détecté
quand le mode de récupération commence. Quand aucun média externe est trouvé
ou quand celui-ci est invalide, des instructions sont montrées à l'écran.

Les média de récupérations de confiance sont des médiums externe (clés USB,
cartes SD, etc) qui stockent un kernel signé avec la clé de
dépannage/recouvrement;

Google fournit des images de tels médiums de récupération pour Chrome OS (qui
ne sont pas recommandées aux utilisateurs car elles contiennent des logiciels
propriétaires).

Elles sont signées avec les clés de récupération de Google, qui sont
préinstallées sur l'appareil lors de la livraison.

Lors du remplacement du flash entier de l'appareil, les clés préinstallées
sont remplacées. Quand la clé de recouvrement privée est disponible (p.ex
quand on utile des clés générées soi-même), elle peut être utilisée pour
signer un kernel à des fins de recouvrement.

Activer le mode développpeur
-----------------------

Comme indiqué sur l'écran du mode de récupération, le mode développeur
peut-être activé en pressant *Ctrl + D*. Les instructions pour confirmer
l'activation du mode développeur sont ensuite montrées sur l'écran.

Configurer les paramètres de vérification du démarrage
====================================

Le comportement de depthcharge dépend de l'implémentation du démarrage vérifié
(vboot, **V**erified **B**oot), qui peut être configuré avec des paramètres
stocké dans le stockage non-volatile du démarrage vérifié.

Ces paramètres peuvent être modifiés avec l'outil `crossystem`, nécessitant
des privilèges suffisants pour accéder au stockage non-volatile du démarrage
vérifié.

`crossystem` dépend de `mosys`, qui est utilisé pour accéder le stockage
non-volatile du démarrage vérifié sur certaines machines. `crossystem` et
`mosys` sont tout deux libre et leur code source est rendu disponible par
Google:
[crossystem](https://chromium.googlesource.com/chromiumos/platform/vboot_reference/).
[mosys](https://chromium.googlesource.com/chromiumos/platform/mosys/).

Ces outils ne sont pas encore distribués avec Libreboot. Cependant, ils sont
préinstallés sur la machine, avec ChromeOS.

Certains de ces paramètres ont le potentiel *d'affaiblir la sécurité de
l'appareil*. En particulier, la désactivation de la vérification de la
signature des kernels, le démarrage sur un média externe ou une charge utile
classique peuvent affaiblir la sécurité de l'appareil.

Les paramètres suivant peuvent être configurés:

Vérification de la signature des kernels:

    # crossystem dev_boot_signed_only=1 # activer
    # crossystem dev_boot_signed_only=0 # désactiver

Démarrage sur média externe:

    # crossystem dev_boot_usb=1 # activer
    # crossystem dev_boot_usb=0 # désactiver

Démarrage sur charge utile classique:

    # crossystem dev_boot_legacy=1 # activer 
    # crossystem dev_boot_legacy=0 # désactiver

Médium de démarrage par défaut:

    # crossystem dev_default_boot=disk # stockage interne
    # crossystem dev_default_boot=usb # média externe 
    # crossystem dev_default_boot=legacy # charge utile classique


Copyright © 2015 Paul Kocialkowski <contact@paulk.fr>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
