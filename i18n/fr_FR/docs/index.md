---
title: Documentation
...

Les informations de versions de Libreboot peuvent être trouvée dans [release.md](release.md).
Vérifiez toujours [libreboot.org](/) pour les mises à jours. 
Les nouvelles versions de Libreboot sont annoncées dans la [section actualités](../news/) du site web.

[Réponses aux questions fréquemment posées à propos de Libreboot](../faq.md).


Installer Libreboot
===================

-   [Sur quelles machines puis-je utiliser Libreboot ?](hardware/)
-   [Comment installer Libreboot](install/)


Installer des systèmes d'exploitations
============================

-   [Comment installer GNU+Linux sur une machine Libreboot](gnulinux/)
-   [Comment installer BSD sur une machine Libreboot](bsd/)



Informations pour les développeurs
=================================

-   [Comment compiler le code source de Libreboot](git/)
-   [Charge utile (payload) Depthcharge](depthcharge/)
-   [Charge utile GRUB](grub/)



Autres informations
================

-   [Divers](misc/)
-   [Liste de nom de codes matériels](misc/codenames.md)


À propos du projet Libreboot
===========================

Libreboot est un remplacement [libre](https://www.gnu.org/philosophy/free-sw.html) 
et au code source ouvert pour le BIOS ou l'UEFI, initialisant le matériel et démarrant votre système d'exploitation.
Nous sommes un membre du projet [Peers Community](https://peers.community), une organisation qui supporte le logiciel libre.

Libreboot est une distribution (distro) de [coreboot](http://coreboot.org/) sans logiciel propriétaire, avec l'intention d'être un remplacement [libre](https://www.gnu.org/philosophy/free-sw.html) du BIOS pour votre ordinateur. Le projet se dirige vers les utilisateurs, tentant de rendre coreboot utilisable facilement autant que possible.

Libreboot a de nombreux avantages pratiques par rapport aux micrologiciels de démarrage propriétaire,	
tel que des vitesses de démarrage plus rapide et une meilleure sécurité.
Vous pouvez [installer GNU+Linux avec le /boot/ chiffré](gnulinux/), [vérifier les signatures GPG sur votre kernel](gnulinux/grub_hardening.md), mettre un kernel dans la puce de flash et plus encore.


Le projet libreboot a 3 buts principaux :
-------------------------------------------

-   *Recommander et distribuer seulement du logiciel libre*.
Coreboot distribue certaines pièces de logiciels propriétaires nécessaires dans quelques systèmes. 
Des exemples peuvent inclure des choses comme les mises à jour du microcode du processeur (CPU), blobs d'initialisation mémoire et ainsi de suite.
Le projet Coreboot recommande parfois d'ajouter plus de blobs qu'ils ne distribuent pas, comme le Video BIOS ou le *Moteur de management* d'Intel (IME).
Cependant, beaucoup d'individus talentueux et dévoués dans Coreboot travaillent dur pour remplacer ces blobs quand cela est possible.
-   *Supporter le plus de matériel possible* Libreboot supporte moins de matériels que coreboot, parce que la majorité des systèmes de coreboot ont besoin de certains logiciels propriétaires pour marcher proprement.
Libreboot est une tentative de supporter autant de matériel  que possible, sans aucun logiciel propriétaire.
-   *Rendre Coreboot facile à utiliser*. Coreboot est notable pour sa difficulté d'installation, dû à un manque global de documentation et support centrés sur l'utilisateur.
La majorité de gens abandonneront tout simplement avant de tenter d'installer coreboot.

Libreboot tente de combler ces lacunes en fournissant un système de construction automatisant une bonne partie de la création d'image et de customisation de coreboot.
Deuxiémement, le projet produit de la documentation orientée vers les utilisateurs non-techniques.
Troisiémement, le projet tente de fournir un excellent support utilisateur via les listes de diffusions et IRC.

Libreboot contient déjà une charge utile (GRUB), flashrom et autres parties nécessaires.
Tout est complétement intégré, d'une façon que la majorité des étapes compliqués qui sont autrement requises, sont déjà faites en avance pour l'utilisateur.

Vous pouvez télécharger les images ROM pour votre système libreboot et les installer sans avoir à construire quoi que ce soit depuis la source.
Si, cependant, vous êtes intéressé dans le façonnage de votre propre image, le système de construction rend ceci relativement aisé.


Libreboot est une distribution de coreboot, pas un embranchement (fork) de coreboot.
---------------------------------------------------------

Libreboot est un embrachement de coreboot. De temps à temps, le projet se rebase sur la dernière version de coreboot, avec le nombre de patchs utilisés minimisés.

Tout nouveau développement de coreboot devrait se faire dans coreboot (en amont), pas libreboot ! Libreboot est à propos du déblobage et empaquetage de coreboot d'une façon amicale à l'utilisateur, où la majorité du travail est déjà fait pour l'utilisateur.

Par exemple, si vous voulez ajouter une nouvelle carte mère à libreboot, vous devriez l'ajouter d'abord en premier à coreboot.
Libreboot recevra automatiquement votre code plus tard, quand il se mettra à jour lui-même.


L'arborescence déblobée de coreboot utilisé dans libreboot est référée en tant que *coreboot-libre*, pour la distinguer comme un composant de *libreboot*.


Libreboot est une version 'stable' de coreboot
---------------------------------------------

-   Coreboot utilise le modèle de la [publication continue](https://fr.wikipedia.org/wiki/Rolling_release), ce qui veut dire que ce n'est pas garantie d'être stable, ou même de marcher un jour donné.
Coreboot a un processus de revue de code strict, mais étant un si large projet avec tant de contributeurs, les régressions sont toujours possibles.
-   Libreboot "gèle" sur une version particulière de coreboot, s'assure que tout marche correctement, fait des corrections par dessus et répète cela pour chaque dernière mise à jour de coreboot.
En faisant comme celà, ça fournit une garantie plus forte à l'utilisateur que le micrologiciel sera fiable, et ne cassera pas leur système.


Comment sais-je la version que je suis en train d'exécuter? {#version}
========================================

Si vous êtes au moins 127 commits après la version 20150518 (message de commit *build/roms/helper: add version information to CBFS*) (ou vous avez quelconque version stable de libreboot *en amont* après 20150518), alors vous pouvez presser C dans la console GRUB, et utilisez cette commande pour trouver quelle version de libreboot vous avez:

    cat (cbfsdisk)/lbversion

Celà marchera aussi sur les images non versionnées (la chaîne de caractère de la version est générée automatiquement, en utilisant `git describe --tags HEAD`), contruitent à partir du répertoire git. Un fichier nommé `version` sera inclu dans les archives que vous avez téléchargées.

Si ça existe, vous pouvez extraire ce fichier `lbversion` grâce à l'utilitaire `cbfstool` que libreboot inclut, depuis une image ROM que vous avez soit déchargée ou pas encore flashée.
Dans votre distribution, exécutez cbfstool sur votre image ROM (`libreboot.rom`, dans cet exemple):

    $ ./cbfstool libreboot.rom extract -n lbversion -f lbversion

Vous aurez maintenant un fichier, nommé `lbversion`, que vous pourriez lire dans n'importe quel programme que vous utilisiez pour la lecture/écriture de fichiers texte.

Pour git, c'est facile. Vérifiez juste le log git.

Pour les versions égales ou en dessous 20150518, ou d'instantanés générés à partir du répertoire git en dessous de 127 commits après 20150518, et vous pouvez trouver un fichier nommé *commitid* à l'intérieur des archives.
Si vous utilisez des images de ROM pré-construites venant du projet libreboot, vous pouvez presser C dans GRUB pour accéder au terminal, et ensuite exécutez cette commande:

     lscoreboot

Vous pouvez trouver une date ici, détaillant quand l'image de la ROM a été construite.
Pour les images pré-construites distribuées par le projet libreboot, c'est une vague approximation de la version que vous avez, parce que les numéros de versions sont datés, et les archives de version sont construites le même jour que la version; vous pouvez corréler cela avec les informations de version dans [release.md](release.md).

Pour la 20160818, notez que le fichier lbversion manquait du CBFS sur les images GRUB.
Vous pouvez quand même deviner quelle version de libreboot que vous avez en comparant les sommes de contrôle des décharges d'image (images dumps) (avec le descripteur rempli de 00s, et la même est faite aux ROMS de l'archive de la version si vous êtes sur un ordinateur portable GM45).

Il peut y avoir aussi un fichier ChangeLog dans votre archive de version, comme ça vous pouvez regarder dedans pour deviner quelle version vous avez.

Vous pouvez aussi vérifier la documentation venant avec vos archives, et dans *docs/release.html* il y aura les informations à propos de la version de libreboot que vous utilisez.

Généralement parlant, il est conseillé d'utiliser la dernière version de libreboot.

Copyright © 2014, 2015, 2016 Leah Rowe <info@minifree.org>\

Permission est donnée de copier, distribuer et/ou modifier ce document sous les termes de la Licence de documentation libre GNU version 1.3 ou quelconque autre versions publiées plus tard par la Free Software Foundation sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.

Une copie de cette license peut être trouvé dans [fdl-1.3.md](fdl-1.3.md).
