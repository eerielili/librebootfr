# Présentation du projet Libreboot

La documentation de Libreboot est dans le dossier [docs/](https://git.miquellionel.ovh/lili/libreboot_fr/src/branch/master/docs).
Les termes de la licence peuvent être trouvé dans [COPYING](https://git.miquellionel.ovh/lili/libreboot_fr/src/branch/master/COPYING).

Libreboot est destiné à être un remplacement libre du BIOS ou UEFI; un micrologiciel libre initialisant le matériel et démarrant le chargeur d'amorçage (bootloader) pour votre système d'exploitation. C'est aussi un BIOS open source, mais l'open source échoue à promouvoir la liberté; définissez libreboot en tant qu'un logiciel libre, merci.

## Pourquoi utiliser Libreboot ?

Beaucoup de personnes utilisent un micrologiciel non libre, même s'ils utilisent GNU/Linux. Les micrologiciels
BIOS/UEFI non libre contiennent souvent des portes dérobées, peuvent être lent et avoir de grave bugs, 
vous laissant démuni et à la merci des développeurs; vous n'avez pas de libertés sur votre usage de l'informatique. 


Par contraste,libreboot est en train de construire un monde où tout le monde peut utiliser, étudier, adapter et partager des logiciels, avec un vrai contrôle et une vraie appropriation de leur technologie.
.

Bref, vous devriez l'utiliser pour le bien de votre liberté !

Libreboot est plus rapide, plus sécurisé et plus stable que la plupart des micrologiciels non-libre, et peut apporter d'autres fonctionnalités avancées (tel que le chiffrement du /boot/, vérification de la signature GPG avant de démarrer votre kernel, possibilité de charger un OS depuis la puce flash (flash chip), et plus encore).

Les fournisseurs principaux de Libreboot en amont sont Coreboot (que nous "déblobons", pour l'initialisation matérielle), depthcharge (chargeur d'amorçage, et charge utile de Libreboot par défaut sur l'architecture ARM), et GRUB (chargeur d'amorçage, et charge utile de Libreboot par défaut sur l'architecture x86). 


Nous intégrons par ailleurs flashrom (pour installer Libreboot), ainsi que quelques un de nos utilitaires, scripts et fichier de configuration. 

Tout celà est intégré dans un seul paquetage cohérent et facile à utiliser. Nous ajoutons nos propres patchs aux nombreuses sources utilisées en amont, et quand celà est faisable nous essayons de fusionner en amont autant que possible.

Le système de contruction de Libreboot et sa documentation sont disponibles ici dans le but de rendre le micrologiciel libre accessible à tous.


Ce LISEZMOI est sous la licence [Creative Commons Zero 1.0](https://creativecommons.org/publicdomain/zero/1.0/)
