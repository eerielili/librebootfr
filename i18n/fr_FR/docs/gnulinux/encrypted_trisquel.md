---
title: Installer Trisquel GNU+Linux avec le chiffrement de tout le disque (incluant /boot)
x-toc enable: true
...

Ce guide est écrit pour la distribution GNU+Linux Trisquel version 7.0 (Belenos) mais ça devrait aussi marcher pour Trisquel version 6.0 (Toutatis).

## Gigabyte GA-G41M-ES2L
Pour démarrer le netinstalleur Trisquel, assurez-vous de spécifier
fb=false dans les paramètres du kernel linux dans grub.
Ça démarrera l'installeur en mode texte au lieu d'utiliser un tampon
d'image.

## Démarrer le média d'installation
Démarrez votre système d'exploitation avec le média d'installation. 
Si vous ne savez pas comment le faire, référez-vous à 
['Comment préparer et démarrer un installeur USB dans les 
systèmes Libreboot'](grub_boot_installer.md).

## Sélectionner un language
La première partie de l'installation est de sélectionner le language
du système; on choisira `Français`.

## Sélectionner votre emplacement
Vous aurez besoin de choisir votre emplacement; on choisira `France`.

## Configurer le clavier
Vous aurez besoin de sélectionner la bonne disposition pour votre 
clavier; si vous voulez que l'installeur le fasse automatiquement
choissisez `Oui`, et il vous demandera si oui ou non certaines touches.
sont présentes sur votre clavier. Dites simplement `Oui` ou `Non` quand
il le faut.

Sinon, choisissez `Non` afin que l'installeur ne choisisse pas 
automatiquement la disposition de votre clavier, il faudra alors simplement
la sélectionner depuis une liste.

## Configurer le réseau

## Choisir l'interface réseau

Vous aurez besoin de sélectionner l'interface réseau devant être utilisé 
pour l'installation. Si vous avez une connexion ethernet (p.e. filaire), 
choisissez `eth0`; sinon choisissez `wlan0` (pour le sans fil).

Si vous choisissez `wlan0`, entrez la phrase/mot de passe de votre réseau 
sans fil WPA/WPA2 (Votre réseau sans fil devrait avoir un mot de passe, et
aucun routeur moderne ne devrait utiliser le 
[protocole WEP](https://en.wikipedia.org/wiki/Wired_Equivalent_Privacy)).

### Choisir votre nom d'hôte
Vous aurez besoin de choisir un nom d'hôte pour le système, identifiant
votre ordinateur sur le réseau; ça peut être n'importe quoi, mais ça doit
consister seulement de nombres, lettres majuscules et miniscules, et de tirets `-`.

### Choisir un mirroir de l'Archive Trisquel
Choisissez le serveur depuis lequel vous téléchargerez les paquets Trisquel
requis pour l'installation. Les choix sont séparés par pays; choisissez simplement
le votre.

Après avoir choisi le pays, vous serez amenés vers une liste de divers serveurs 
individuels. Si il y a plus d'une option, choisissez celui qui est la plus proche
de vous, sinon choisissez n'importe quel autre disponible.

La dernière étape de la configuration du réseau est d'entrer un proxy HTTP (si vous
avez besoin d'un pour accéder le réseau ). Si vous en avez un, insérez le, sinon pressez
`Tabulation` puis choisissez `Continuer` avec les flèches directionnelles.

## Charger des composants additionnels
Maintenant l'installateur a besoin de télécharger quelques paquets de plus pour continuer
l'installation. Selon votre bande passante, ça peut prendre jusqu'à quelques minutes pour
se terminer.

## Configurer les utilisateurs et mots de passe.
Entrez ici le nom complet de l'utilisateur : vous pouvez utiliser votre vrai nom ou 
juste un pseudonyme, puis sélectionnez `Continuer`.

On vous demandera alors un *nom d'utilisateur*. Prenez ce que vous voulez et 
rentrez-le, puis sélectionnez `Continuer`.

Choisissez une phrase de passe (mieux qu'un mot de passe). La méthode du 
[lancer de dés](http://world.std.com/~reinhold/diceware.html) (NdT:diceware en anglais) est hautement recom-
-mandée pour en dégoter un.

Je recommande la combinaison de la méthode *diceware*  avec quelque chose de personnel.
Un example de ça serait de choisir quatre mots de la liste du lancer de dés, et vous
y mettez un cinquième "mot" (p.e. une combinaison de caractères qui vous est propre, co-
-mme un nom plus un nombre/caractère spécial); cet alliage augmente dramatiquement la
sécurité d'une phrase de passe aux *lancer de dés* (p.e. même si quelqu'un avait la liste
complète des mots de la liste du lancer de dés, il ne pourrait pas deviner votre phrase
de passe en utilisant le brute force/attaque dictionnaire).

**NOTE: Ça serait difficile à faire pour une personne, même si vous utilisez *seulement*  
les mots de la liste**

Par exemple, supposons que le nom de votre chat est **Max** et qu'il a trois ans, on pourrait
faire quelque chose comme ça :

    mot_dés_1 mot_dés_2 mot_dés_3 mot_dés_4 Max=3ans

Ça a un large degré d'aléatoire (dû à l'utilisation de la méthode du *lancer de dés*), 
et contient aussi une pièce unique d'information personnelle dont quelqu'un 
aurait besoin pour devenir la phrase de passe; c'est une combinaison avec
beaucoup de potentiel.

Après avoir rentré un mot/phrase de passe deux fois, sélectionnez `Continuer`.

Ça vous demandera maintenant si vous voulez chiffrer votre répertoire personnel.
Rappel, ce ne doit *PAS* être confondu avec le chiffrement de votre disque en entier
(le but de ce guide); ça sera juste les fichiers reposant dans `~`, et ça utilise un
protocole de chiffrement différent  (`ecryptfs`).
Si ici vous voulez chiffrer votre répertoire personnel, choisissez `Oui`; cependant,
puisque nous allons chiffrer l'entière installation, ça sera non seulement redondant mais
ça ajoutera une pénalité de perfomance remarquable, pour un gain de sécurité minime dans
la majorité des cas d'utilisations.
C'est donc optionnel, et *NON* recommandé. Choisissez `Non`.

## Configurer la date et l'heure
L'installeur essayera de détecter automatiquement votre fuseau horaire; si il le choisit
correctement, sélectionnez `Oui`, sinon `Non` et il vous demandera de choisir le bon.

## Partitionner les disques
Maintenant il est temps de partitionner le disque; plusieurs options vous seront montrées;
choisissez le partitionnement `Manuel`.

1. Utilisez les touches directionnelles pour sélectionner le disque (cherchez une taille
correspondante et un nom de constructeur dans la description), et pressez `Entrée`. Ça
vous demandera si vous voulez créer une nouvelle table de partition vide sur le péri-
-phérique; choisissez `Oui`.

2. Votre disque sera maintenant montré avec une seule partition, labellisé `#1`; sélectionnez-
la (il y sera écrit `Espace libre` en dessous) et pressez `Entrée`.

3. Choisissez `Créer une nouvelle partition`. Par défaut, la taille de la partition sera
le disque tout entier; laissez-la tel quel et sélectionnez `Continuer`.

4. Quand ça demande le type de la partition, partez pour `Primaire`; vous serez amenés
vers un écran contenant une liste d'information à propos de votre nouvelle partition;
assurez-vous de remplir chaque champ comme il suit (en utilisant les flèches haut et bas
pour naviguer, `Entrée` pour modifier une option):

    * Utiliser en tant que : `volume physique pour le chiffrement` 
    * Méthode de chiffrement : `Device-mapper (dm-crypt)`
    * Chiffrement: `aes`
    * Taille de la clé: `256`
    * Algorithme IV: `xts-plain64`
    * Clé de chiffrement: `phrase de passe`
    * Supprimer les données: `Oui`

        Pour le champ `Supprimer les données` choisissez seulemyent `Non`, si c'est soit un nouveau
disque qui ne contienne aucune de vos données non chiffrées ou qu'il était chiffré tout entier
auparavant.

5. Choisissez `Finir avec la configuration de cette partition`. Ça vous aménera au menu principal
du partitionnage.

6. Choisissez `Configurer les volumes chiffrés`; l'installeur demandera si vous voulez écrire les
changements sur le disque et configurer les volumes chiffrés; choisissez `Oui`.

7. Sélectionnez `Créer des volumes chiffrés`

8. Sélectionnez votre partition avec les flèches directionnelles (presser `Espace` fera apparaître
une étoile `*` entre les crochets; c'est comme ça que vous savez qu'elle a été sélectionnée). Pressez
`Tabulation`, et choisissez `Continuer`.

9. Sélectionnez `Finir`. Vous serez demandé si vous voulez vraiment effacer le disque; choisissez `Oui`
(la suppression est longue alors soyez patient. Si votre système précédent était chiffré, éxecutez la
pour environ une minute puis choisissez `Annuler`; ça prendra soin que l'en-tête LUKS soit
complétement rasé).

10. Maintenant vous avez besoin d'entrer une phrase de passe pour chiffrer le disque en entier.
Assurez-vous qu'elle est différente du mot de passe utilisateur créé plus tôt, mais utilise encore
la méthode du [lancer de dés](http://world.std.com/~reinhold/diceware.html) pour la créer. Vous
aurez à rentrer la phrase/mot de passe deux fois; après coup, vous serez redirigé vers le
menu de partitionnage principal.

11. Vous verrez maintenant votre périphérique chiffré au sommet de la liste. Ça commencera par 
quelque chose comme : `Volume chiffré (sdXY_crypt)`. Choisissez la partition labellé `#1`.

12. Changez la valeur d'`Utiliser en tant que` sur `Volume physique pour LVM`. Choisissez ensuite
`Finir avec la configuration de cette partition`; vous retournerez au menu principal du partitionnage.

13. Choisissez `Configurer le Gérant de Volume Logique (LVM)`. On vous demandera si vous voulez
`Garder la disposition du partitionnement en cours et configurer LVM`, choisissez `Oui`.

14. Choisissez `Créer un groupe de volumes`. Vous allez devoir entrer le nom du groupe; utilisez
**grubcrypt**. Sélectionnez la partition chiffré pour en tant que périphérique cible (en pressant 
`Espace`, qui fera apparaître une étoile `*` entre crochets; c'est comme ça que vous savez qu'elle
a été choisie). Pressez `Tabulation`, et choisissez `Continuer`.

15. Choisissez `Créer un volume logique`. Sélectionnez le groupe de volumes que vous avez créé 
précédemment (ici, **grubcrypt**), et nommez-le **trisquel**; la taille de la partition est 
celle du disque tout entier moins 2048Mo (pour faire de la place pour l'espace d'échange, ou swap).
Pressez `Entrée`.

16. Choisissez de nouveau `Créer un volume logique`, en sélectionnant le même groupe de volume
que l'étape précédente. Nommez-le **swap** et laissez la taille par défaut (elle devrait être de
2048Mo). Pressez `Entrée` puis choisissez `Finir`.

NdT: La taille de l'espace d'échange, ou *swap* en anglais, doit être généralement d'une 
à deux fois la taille de votre mémoire vive réelle. Les 2048Mo de l'étape 15 et 16 peuvent
être donc adaptés en accordance.

17. Maintenant vous êtes de retour à l'écran de partitionnage principal. Vous allez simplement
définir les points de montages et systèmes de fichiers pour chacune des partitions que vous 
venez juste de créer.
Sous `LVM VG grubscrypt, LV trisquel`, sélectionnez la première partition: `#1`. Changez les
valeurs dans cette section par les suivante puis choisissez `Finir avec la configuration de
cette partition`:

    * utiliser en tant que: `ext4`
    * point de montage: `/`

18. Sous `LVM VG grubcrypt, LV swap`, sélectionner la première partition: `#1`. Changez la
valeur d'`utiliser en tant que` par `swap`. Choisissez `Finir avec la configuration de
cette partition`.

19. Finalement, quand vous êtes de retour à l'écran de partitionnage principal, choisissez
`Finir le partitionnage et écrire les changements sur le disque`. Ça vous demandera de
vérifier si vous voulez vraiment le faire; choisissez `Oui`.

## Installer le système de base

The hardest part of the installation is done; the installer will now download and install the packages necessary for your system to boot/run. The rest of the process will be mostly automated, but there will be a few things that you have to do yourself.

### Choose a Kernel
It will ask you which kernel you want to use; choose `linux-generic`.

**NOTE: After installation, if you want the most up-to-date version of the Linux kernel (Trisquel's kernel is sometimes outdated, even in the testing distro), you might consider using [this repository](https://jxself.org/linux-libre/) instead. These kernels are also deblobbed, like Trisquel's (meaning there are no binary blobs present).**

### Update Policy
You have to select a policy for installing security updates; I recommend that you choose `Install security updates automatically`, but you can choose not to, if you prefer.

### Choose a Desktop Environment
When prompted to choose a desktop environment, use the arrow keys to navigate the choices, and press `Spacebar` to choose an option; here are some guidelines:

* If you want *GNOME*, choose **Trisquel Desktop Environment**
* If you want *LDXE*, choose **Trisquel-mini Desktop Environment**
* If you want *KDE*, choose **Triskel Desktop Environment**

You might also want to choose some of the other package groups (or none of them, if you want a basic shell); it's up to you. Once you've chosen the option you want, press `Tab`, and then choose `Continue`.

## Install the GRUB boot loader to the master boot record
The installer will ask you if you want to install the GRUB bootloader to the master boot record; choose `No`. You do not need to install GRUB at all, since in Libreboot, you are using the GRUB payload on the ROM to boot your system.

The next window will prompt you to enter a `Device for boot loader installation`. Leave the line blank; press `Tab`, and choose `Continue`.

## System Clock
The installer will ask if your system clock is set to UTC; choose `Yes`.

## Finishing the Installation
The installer will now give you a message that the installation is complete. Choose `Continue`, remove the installation media, and the system will automatically reboot.

## Booting your system
At this point, you will have finished the installation. At your GRUB boot screen, press `C` to get to the command line, and enter the following commands at the `grub>` prompt:

    grub> cryptomount -a
    grub> set root='lvm/grubcrypt-trisquel'
    grub> linux /vmlinuz root=/dev/mapper/grubcrypt-trisquel \
    >cryptdevice=/dev/mapper/grubcrypt-trisquel:root
    grub> initrd /initrd.img
    grub> boot

Without specifying a device, **cryptomount's** `-a` parameter tries to unlock *all* detected LUKS volumes (i.e., any LUKS-encrypted device that is connected to the system). You can also specify `-u` (for a UUID). Once logged into the operating system, you can find the UUID by using the `blkid` command:

    $ sudo blkid

## ecryptfs
If you didn't encrypt your home directory, then you can safely ignore this section; if you did choose to encrypt it, then after you log in, you'll need to run this command:

    $ sudo ecryptfs-unwrap-passphrase

This will be needed in the future, if you ever need to recover your home directory from another system. Write it down, or (preferably) store it using a password manager (I recommend `keepass`,`keepasX`, or `keepassXC`).

## Modify grub.cfg (CBFS)
The last step of the proccess is to modify your **grub.cfg** file (in the firmware), and flash the new configuration, [using this tutorial](grub_cbfs.md); this is so that you don't have to manually type in the commands above, every single time you want to boot your computer. You can also make your GRUB configuration much more secure, by following [this guide](grub_hardening.md).

## Troubleshooting
During boot, some Thinkpads have a faulty DVD drive, which can cause the `cryptomount -a` command to fail, as well as the error `AHCI transfer timed out` (when the Thinkpad X200 is connected to an UltraBase). For both issues, the workaround was to remove the DVD drive (if using the UltraBase, then the whole device must be removed).

Copyright © 2014, 2015 Leah Rowe <info@minifree.org>

Copyright © 2017 Elijah Smith <esmith1412@posteo.net>

Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Documentation License Version 1.3 or any later version published by the Free Software Foundation with no Invariant Sections, no Front Cover Texts, and no Back Cover Texts. A copy of this license is found in [../fdl-1.3.md](../fdl-1.3.md)
