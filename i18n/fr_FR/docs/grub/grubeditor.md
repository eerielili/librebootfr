---
title: ÉDITEUR DE GRUB
x-toc-enable: true
...

Cette section concerne l'édition des fichiers de configurations GRUB à
l'intérieur des images ROM libreboot grâce à un éditeur.

Éditeur de GRUB
================

Les images ROM de Libreboot supportent maintenant une fluide édition de
_grub.cfg_ et _grubtest.cfg_ grâce au script grubeditor.sh! Au lieu d'exécuter
manuellement cbfstool pour manipuler ces fichiers de configuration, ce script
prendra la tâche en charge afin que vous puissiez véritablement vous
concentrer sur la modification des fichiers de configuration GRUB selon vos
besoins.

Au moment de cet écrit, grubeditor.sh supporte l'extraction et l'édition de
soit le fichier grub.cfg ou grubtest.cfg dans n'importe quelle image ROM
Libreboot compatible cbfstool contenant ces fichiers, même celles qui ont été
précédemment modifiées. Il peut aussi échanger ces fichiers de configuration
dans une image ROM existante, pratique si vous avez un grubtest.cfg qui marche
et vous voulez le mettre par défaut, ou si vous avez 'cassé' le grub.cfg
principal, et que vous savez que votre grubtest.cfg marche encore.
Dernièrement, ça peut aussi faire la diff de ces deux fichiers de
configurations pour montrer leurs différences.


Nécessités
=========

grubeditor.sh nécessite un environnement d'architecture x86, x86_64 ou armv7l,
puisque ce sont ceux pour lesquels les exécutables de cbfstools sont fournis.
Additionnellement, grubeditor.sh nécessite un environnement bash avec la
fonctionnalité getopt étendue qui peut exécuter la commande **diff** et écrire
dans /tmp. Enfin, le script s'attend à être placé dans le répertoire
racine des paquets utilitaires de Libreboot afin de bien exécuter
cbfstool.

Il y a des chances que vous remplissez déjà ces préréquis si vous êtes dans un
environnement Linux sur l'une des architectures listées, et que vous avez
téléchargé les utilitaires libreboot depuis une source officielle.
Sinon, ça ne devrait pas être trop difficile d'utiliser un LiveCD Linux de
votre choix qui fournissent l'essentiel.

Optionnellement, vous devriez vous assurer que votre variable EDITOR est
définie. Sinon, grubeditor.sh utilisera vi par défaut, qui peut ne pas exister
sur votre système. Vous pouvez supplanter ce choix par défaut ou le contenu de
votre variable EDITOR en utilisant l'option **-e** ou **--editor**.

Aide à l'utilisation
====================

grubeditor.sh a quelques options, la seule étant requise est une image ROM
libreboot valide utilisant une charge utile GRUB2 et contient les fichiers
_grub.cfg_ et _grubtest.cfg_. Les options additionnelles devrait être mise
avant l'image ROM sur la ligne de commande.

grubeditor.sh supporte la combinaison de plusieurs courtes options avec un
seul tiret tel que **-ris**, mais vous pouvez toujours les séparer comme ceci:
**-r -i -s**. Les longues options doivent être toujours écrites en tant
qu'arguments séparés/seuls.

Vous pouvez utiliser l'option **-h** ou **--help** pour voir un bref résumé
des options disponibles. Considérez le guide ici présent comme une version
plus étendue de cette option/écran.

Enfin, vous pouvez vérifier quelle version de grubeditor.sh vous être en train
d'utiliser avec l'option **-v** ou **--version**.

Éditer les fichiers de configuration
------------------------------------

Invoqué sans aucun arguments excepté pour l'image ROM, grubeditor.sh tentera
d'extraire le fichier _grubtest.cfg_ de l'image ROM fournie et lancera son
édition dans l'éditeur de votre choix. Si vous faites des changements,
grubeditor.sh les intégrera dans une nouvelle image ROM avec le même nom dans
le même répertoire, excepté que le nouveau fichier ROM terminera par
".modified". Vous pouvez ensuite flasher cette image ROM dans la puce BIOS de
votre machine.

Si vous préférez éditer le véritable fichier de configuration _grub.cfg_,
utilisez l'option **-r** ou **--realcfg**. Tout marchera pareil sauf que votre
éditeur ouvrira _grub.cfg_ à la place.

Si vous préférez écraser votre image ROM existante au lieu d'en créer une
nouvelle terminant par ".modified", utilisez l'option **-i** ou **--inplace**.
Naturellement, vous pouvez combiner cette option avec l'option
**-r/--realcfg** décrite ci-dessus.

Échanger grub.cfg et grubtest.cfg
---------------------------------

grubeditor.sh supporte l'échangee des fichiers de configuration _grub.cfg_ et
_grubtest.cfg_ grâce à l'option **-s** ou **--swap**. Ça créera une nouvelle
image ROM au côté de l'image ROM existante avec l'extension ".modified",
contenant ces fichiers mais échangés.
Naturellement, vous pouvez demandez l'écrasement de la ROM existante en
ajoutant l'option **-i** ou **--inplace**.

Notez que le script modifiera automatiquement l'entrée de menu "Charger la
configuration de test (grubtest.cfg)" dans les deux fichiers de configurations
pendant cette opération.
Si ça ne faisait pas, ces entrées finirait par s'auto-référencer après le
renommage, cassant la fonctionnalité attendue d'échange entre les deux
fichiers.

Pour des résultats optimal, ne modifiez pas silvouplaît cette section sans
étudier le code source de _grubeditor.sh_ et soyez sûr que vos changements
n'impactent pas la capacité du script à faire cette modification.


Faire la différence entre grub.cfg et grubtest.cfg
--------------------------------------------------

grubeditor.sh supporte la comparaison des fichiers _grub.cfg_ et
_grubtest.cfg_ pour trouver des différences, à l'aide de l'option **-d** ou
**--diffcfg**. Ça utilise la commande diff par défaut, mais si vous voulez
utiliser un autre progralle (p.ex vimdiff), vous pouvez le spécifier avec
l'option **-D** ou **--differ**. Notez que ce mode est seulement fait pour
montrer les différences dans les fichiers et ne supporte pas la mise à jour
des configurations en elles-même, donc n'importe quels changements faits dans
un différenciateur interactif seront ignorés.

Extraire un fichier de configuration
------------------------------------

Vous pouvez simplement extraire un fichier de configuration en utilisant
l'option **-x** ou **--extract**. Cette option s'adapte à l'option
**-r**/**--realcfg** pour choisir entre grubtest.cfg et grub.cfg.

Développement en attente
========================

À FAIRE:
-   permettre l'injection de fichiers de configurations pour complémenter
    l'extracteur.
-   détecter des cas d'exceptions potentiellement dévastateurs, aussi rare
    qu'ils pourraient être
-   je ne peux pas spécifier des arguments apostrophés sur la ligne de
    commandes pour les options **-e** ou **-D**, pourquoi?
-   supporter l'édition des deux fichiers de configurations à la fois si on
    est en train d'utiliser un différenciateur intéractif
-   fonctionner avec d'autres types de fichiers en dehors des fichiers de
    configuration GRUB.

Conclusion
==========

J'espère que grubeditor.sh facilitera significativement la modification des
fichiers de configuration dans vos fichiers ROM Libreboot.

Trouveriez-vous quelconque bugs ou avez des demandes de fonctionnalité,
n'hésitez pas silvouplaît à me faire un email ou me déranger sur IRC.

Copyright © 2014 Leah Rowe <info@minifree.org>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
