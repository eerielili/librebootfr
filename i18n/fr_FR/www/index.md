---
title: Libreboot
...

[![Libreboot logo](logo/logo.svg "Canteloupe, la mascotte de Libreboot"){#logo}](faq.md#qui-a-fait-le-logo)

[FAQ](faq.md)                                                               --
[Téléchargement](download.md)                                                     --
[Installer](docs/install/)                                                    --
[Documentation](docs/)                                                      --
[Nouveautés](news/)                                                               --
[Chat](https://webchat.freenode.net/?channels=libreboot)                    --
[Bogues](https://notabug.org/libreboot/libreboot/issues)                      --
[Envoyez des patchs](git.md)                                                      --
[Achetez préinstallé](suppliers.md)

Libreboot est un micrologiciel de démarrage [respectueux de la liberté](https://www.gnu.org/philosophy/free-sw.html), initialisant le matériel et démarrant un système d'exploitation.
Cela replace le micrologiciel propriétaire BIOS/UEFI trouvé dans les ordinateurs.
Libreboot est compatible avec [certains ordinateurs rendus compatibles](docs/hardware/) sur ARM et x86.

*Le micrologiciel de démarrage* est le logiciel bas-niveau dans un ordinateur étant exécuté le moment qu'il est allumé.
Il apporte tous les composants (CPU,[controlleur de mémoire](https://fr.wikipedia.org/wiki/Contr%C3%B4leur_m%C3%A9moire), quelques périphériques et ainsi de suite) dans un état usable pour que les logiciels marchent aisément.
Le micrologiciel de démarrage chargera typiquement un système d'exploitation (GNU+Linux, BSD, etc) qui fournit une interface commune pour les applications logicielles qui font usage du matériel dans l'ordinateur.
En addition de Libreboot, nous recommandons l'utilisation d'un 
système d'exploitation libre comme définit par les [indications
 de GNU sur les distributions systèmes libre](https://gnu.org/distros/free-system-distribution-guidelines.html) (donc, pas de Windows/Mac. Utilisez GNU+Linux!).

Combinés, le micrologiciel de démarrage et le système d'exploitation fournissent une interface unifiée qui rendent les ordinateurs fonctionnels que ce soit pour une utilisation quotidienne ou le développement logiciel.

Le principal fournisseur de Libreboot en amont est [coreboot](https://www.coreboot.org), d'où nous [enlevons les blobs binaires](docs/#about-the-libreboot-project).
Nous envoyons nos patchs personnalisés à des projets comme coreboot, depthcharge, GRUB et flashrom quand c'est possible.
Ensemble, notre système de build et documentation est fournit avec le but de rendre le micrologiciel libre accessible à tous.
*En d'autres mots, Libreboot est une distribution de coreboot !*.
Pour faire simple, Libreboot intègre tout les composants logiciels requis dans un seul paquetage unifié applicable dans la majorité des cas d'utilisation.

Nous fournissons de l'aide utilisateur via le [canal IRC #libreboot](https://webchat.freenode.net/?channels=libreboot) sur Freenode.
La discussion à propos du développement se passe aussi sur IRC.Les instructions pour envoyer des patchs sont sur la page [git](git.md).

Pourquoi utiliser Libreboot ?
----------------------------

Saviez-vous que vous avez des droits ? Le droit à l'intimité/vie privée, liberté d'expression et le droit de lire.
Dans la sphère de l'informatique, cela veut dire que n'importe qui peut utiliser des [logiciels libre](https://www.gnu.org/philosophy/free-sw.html).
Pour parler simplement, le logiciel libre est un logiciel qui est sous le contrôle de l'utilisateur, et plus important, du collectif qu'est la *communauté*.
Les logiciels non-libre (p.e Windows, MacOS ou un BIOS/UEFI propriétaire), est sous le contrôle exclusif des propriétaires, et non pas des utilisateurs !
Avec des logiciels respectant les libertés, les utilisateurs peuvent à tout moment étudier le code source, et même devenir développeurs eux-mêmes !
Envie d'aider le mouvement ?
Plusieurs façons de contribuer peuvent inclure l'écriture de documentations, fournir du support utilisateur, ou tester d'autres contributions.
Besoin de plus d'idées sur comment vous pouvez aider le projet ? Regardez [la page git](git.md).
Si vous aimeriez qu'une fonctionnalité soit développée mais n'êtes pas enclin techniquement, vous pouvez employer n'importe qui pour faire le travail à votre place.
Avec le logiciel libre, vous êtes essentiellement le propriétaire de votre propre copie.

Beaucoup de personnes utilisent un micrologiciel de démarrage [non-libre](https://www.gnu.org/philosophy/proprietary.html), même s'ils utilisent GNU+Linux.
Les micrologiciels BIOS/UEFI non-libre  [contiennent](faq.md#intel) souvent des [portes dérobées](faq.md#amd), peuvent être lent et avoir des bugs sévères.
Le support et le développement peut être abandonné à tout moment.
Par contraste, libreboot est un logiciel complétement libre, oùtout le monde peut contribuer ou inspecter son code.
Libreboot n'est pas simplement un logiciel libre; c'est aussi un logiciel [copyleft](http://www.gnu.org/philosophy/copyleft.fr.html), versionné sous ( pour la majorité ) un mix de la [GNU General Public License version 2 et 3](http://www.gnu.org/licenses/gpl.html).
Cela veut dire que le logiciel sera toujours libre pour tout le monde.
Si quelqu'un viendrait à prendre Libreboot et essayerait de le rendre propriétaire, il serait en train d'**enfreindre la loi**.
En d'autres mots, le copyleft assure un bien public *commun* où tout le savoir et la puissance sont partagés sans discrimination.

Libreboot est plus rapide, plus sécurisé et plus fiable que la plupart des micrologiciels non-libre.
Libreboot fournit beaucoup de fonctionnalités avancées, comme le /boot/ chiffré, vérification de signature GPG avant le démarrage du kernel de Linux et plus !

