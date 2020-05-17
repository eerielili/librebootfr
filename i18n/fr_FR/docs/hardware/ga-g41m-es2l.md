---
title: Carte mère d'ordi de bureau Gigabyte GA-G41M-ES2L 
...

C'est une carte mère d'ordi de bureau utilisant du matériel Intel (datant
d'environ 2009, contrôleur d'entrée/sorite ICH7, similaire au niveau des
performances au X200 sous Libreboot.
Ça peut donner un ordi de bureau vraiment pas mal, marchant sur libreboot.

Les ports IDE sur la carte mère ne sont pas testés, mais il serait peut-être
possible d'utiliser un disque dur SATA en utilisant un adaptateur IDE à SATA.
Les ports SATA marchent.

Il sera nécessaire de définir une adresse MAC customisée depuis GNU+Linux afin
que la carte réseau marche.
Sur les systèmes basés Debian tel qu'Ubuntu, Devuan ou Debian, voici comment
se présenterait la ligne à mettre dans le fichier /etc/network/interfaces pour
votre carte réseau:\
hwaddress ether addresse\_mac\_ici

Les instructions de flashage peuvent être trouvées dans
[../install/\#flashrom](../install/#flashrom)

Copyright © 2016 Leah Rowe <info@minifree.org>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
