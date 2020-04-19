---
title: Système Guix avec chiffrement du disque tout entier sur Libreboot
...

Objectif
=========

Fournir un guide étape par étape pour mettre en place
un système Guix (seul), avec le chiffrement de tout 
le disque (comprenant /boot), sur des machines tournant
sous Libreboot.


To provide step-by-step guide for setting up guix system (stand-alone guix)
with full disk encryption (including /boot) on devices powered by libreboot.

Public ciblé
=============
N'importe quels utilisateurs, pour le
urs cas d'utilisation
généraux, n'ont pas besoin de différer de ce guide pour
accomplir la mise en place du système.

Les utilisateurs avancés, pour leurs cas d'utilisation 
différents, devront explorer au-delà et en-dehors de 
guide pour la customisation; néanmoins ce guide fournit
des informations qui sont d'une grande utilité.

Marche à suivre
================

Préparation
-----------

Dans votre système GNU/Linux actuel, ouvrez un terminal
en tant que superutilisateur (root).

Insérez le disque USB et récupérez le nom du 
périphérique USB, généralement /dev/sdX, ou X est une
variable qu'il faut noter :

	`lsblk`

Démontez le disque dur USB dans le cas ou il s'est 
auto-monté :

	`umount /dev/sdX`

Téléchargez le dernier (a.b.c) Paquet Installateur ISO 
du système Guix (xxx) et sa signature GPG; où "a.b.c"
est la variable pour le n° de vérsion et "sss" est la
variable pour l'architecture système (x86_64, i386, etc):
	
	`̀``
	wget https://ftp.gnu.org/gnu/guix/guix-system-install-a.b.c.sss-linux.iso.xz
	wget https://ftp.gnu.org/gnu/guix/guix-system-install-a.b.c.sss-linux.iso.xz.sig
	```

Importez la clé publique requise:

	`gpg --keyserver pool.sks-keyservers.net --recv-keys 3CE464558A84FDC69DB40CFB090B11993D9AEBB5`

Vérifiez la signature GPG du paquet téléchargé:

	`gpg --verify guix-system-install-a.b.c.sss-linux.iso.xz.sig`

Extrayez l'image ISO du paquet téléchargé:

	`xz -d guix-system-install-a.b.c.sss-linux.iso.xz`

Écrivez l'image ISO extraite sur le disque dur USB:

	`dd if=guix-system-install-a.b.c.sss-linux.iso of=/dev/sdX; sync`

Redémarrez votre machine:

	`reboot`

Pré-installation
----------------

Au redémarrage, dès que vous voyez le logo de Libreboot, pressez
les touches directionnelles pour changer la sélection de menu.

Choisissez "Chercher pour une configuration GRUB2 sur des médias
externes [s]" et attendez que le système Guix sur la clé USB se
charge.

Changez votre disposition de clavier, où "lo" est le code à deux
lettres de la disposition du clavier (exemple:fr ou us):

	`loadkeys us`

Débloquez les interfaces réseaux (si il y en a):

	`rfkill unblock all`

Récupérez les noms de celles-ci:

	`ifconfig -a`

Activez votre interface réseau requise nwif (filaire ou sans fil),
où "nwif" est la variable correspondant au nom de l'interface.
Pour les connexions filaires, ça devrait suffire:

	`ifconfig nwif up`

Pour les connexions sans fil, créez un fichier de configuration avec
un éditeur de texte, où "fname" est la variable pour un nom de fichier
quelconque (mettez des guillemets si celui-ci contient des espaces):

	`nano fname.conf`

Choisissez, écrivez et sauvegardez UN des extraits suivants, où 'nm'
est le nom du réseau auquel vous voulez vous connecter, 'pw' est le
mot/phrase de passe correspondant(e), et 'un' est l'identifiant.


Pour la majorité des réseaux privés:
```
network={
  ssid="nm"
  key_mgmt=WPA-PSK
  psk="pw"
}
```

ou

Pour la majorité des réseaux publics:
```
network={
  ssid="nm"
  key_mgmt=NONE
}
```

ou

Pour la majorité des réseaux d'entreprises/organisations:
```
network={
  ssid="nm"
  scan_ssid=1
  key_mgmt=WPA-EAP
  identity="un"
  password="pw"
  eap=PEAP
  phase1="peaplabel=0"
  phase2="auth=MSCHAPV2"
}
```

Connectez-vous au réseau configuré, où  "fname" est le nom de fichier
and "nwif" est le nom de l'interface réseau.

	`wpa_supplicant -c fname.conf -i nwif -B`

Assignez une adresse IP à votre interface réseau, où "nwif" est 
le nom de l'interface réseau.

	`dhclient -v nwif`

Obtenez le nom du périphérique /dev/sdX dans lequel vous voudriez
déployer et installer le système Guix, où "X" est la variable dont
il faut prendre note.

	`lsblk`

Nettoyez le périphérique en question. **Attendez que la commande se
finisse**:

	`dd if=/dev/urandom of=/dev/sdX; sync`

Chargez le module device-mapper dans le kernel en cours:

	`modprobe dm_mod`

Partitionnez le périphérique en question. Faites juste,
GPT --> New --> Write --> Quit; les paramètres par défauts seront
définis:

	`cfdisk /dev/sdX`

Chiffrez la partition:

	`cryptsetup -v --cipher serpent-xts-plain64 --key-size 512 --hash whirlpool --iter-time 500 --use-random --verify-passphrase luksFormat /dev/sdX1`

Obtenez et **notez bien** l' "UUID LUKS".

	`cryptsetup luksUUID /dev/sdX1`

Ouvrez la partition chiffrée en questioon, où "partname" est
le nom de partition désiré:

	`cryptsetup luksOpen /dev/sdX1 partname`

Construisez un système de fichier sur cette partition, où
"fsname" est le nom du système de fichier voulu:

	`mkfs.btrfs -L fsname /dev/mapper/partname`

Montez le système de fichier en question dans le système en
cours:

	`mount LABEL=fsname /mnt`

Créez un fichier swap (fichier d'échange) et rendez le écrivable/
lisible seulement par le superutilisateur (root).


	```
	dd if=/dev/zero of=/mnt/swapfile bs=1MiB count=2048`
	chmod 600 /mnt/swapfile
	mkswap /mnt/swapfile
	swapon /mnt/swapfile
	```

Installation
------------

Faites en sorte que les paquets d'installation soient écris
sur le système de fichiers monté en question:

	`herd start cow-store /mnt`

Créez le répertoire requis:

	`mkdir /mnt/etc`

Créez, éditez et sauvegardez le fichier de configuration en tapant
l'extrait de code suivant. **SOYEZ ATTENTIF** aux variables dans 
l'extrait de code et remplacez les avec vos valeurs relevées.

	`nano /mnt/etc/config.scm`

Extrait:

```
(use-modules
	(gnu)
	(gnu system nss))
(use-service-modules
	xorg
	desktop)
(use-package-modules
	certs
	gnome)
(operating-system
	(host-name "hostname")
	(timezone "Zone/SubZone")
	(locale "ab_XY.1234")
	(keyboard-layout
		(keyboard-layout
			"xy"
			"altgr-intl"))
	(bootloader
		(bootloader-configuration
			(bootloader
				(bootloader
					(inherit grub-bootloader)
					(installer #~(const #t))))
			(keyboard-layout keyboard-layout)))
	(mapped-devices
		(list
			(mapped-device
				(source
					(uuid "luks-uuid"))
					(target "partname")
					(type luks-device-mapping))))
	(file-systems
		(append
			(list
				(file-system
					(device
						(file-system-label "fsname"))
					(mount-point "/")
					(type "btrfs")
					(dependencies mapped-devices)))
			%base-file-systems))
	(users
		(append
			(list
				(user-account
					(name "username")
					(comment "Full Name")
					(group "users")
					(supplementary-groups '("wheel" "netdev" "audio" "video" "lp" "cdrom" "tape" "kvm"))))
			%base-user-accounts))
	(packages
		(append
			(list
				nss-certs)
			%base-packages))
	(services
		(append
			(list
				(extra-special-file "/usr/bin/env"
					(file-append coreutils "/bin/env"))
				(set-xorg-configuration
					(xorg-configuration
						(keyboard-layout keyboard-layout)))
				(service gnome-desktop-service-type))
			%desktop-services))
	(name-service-switch %mdns-host-lookup-nss))
```
    
Initialisez le nouveau système Guix:

	`guix system init /mnt/etc/config.scm /mnt`

Redémarrez votre machine:

	`reboot`

Post-Installation
------------

Au redémarrage, dès que vous voyez le logo de Libreboot, choisissez
l'option 'Charger le système d'exploitation [o]'

Entrez la clé LUKS pour le GRUB de libreboot, quand demandé.

Vous aurez peut-être à passer outre des avertissements en
pressant de façon répétée la touche 'Entrée/Retour'.

Vous verrez maitenant le menu GRUB de guix depuis lequel vous
pouvez choisir l'option par défaut.

Entrez de nouveau la clé LUKS, pour le kernel, quand demandé.

Sur la page d'authentification GNOME, identifiez-vous en tant que
"root" en laissant le mot de passe vide.

Ouvrez un terminal depuis GNOME Dash.

Définissez un mot de passe pour l'utilisateur "root". Suivez les
demandes:

	`passwd root`

Faîtes de même pour l'utilisateur "username". Suivez les
demandes:

	`passwd username`

Mettez à jour la distribution Guix. Attendez que le processus se
finit:

	`guix pull`

Mettez à jour les chemins de recherche:

	```
	export PATH="$HOME/.config/guix/current/bin:$PATH"
	export INFOPATH="$HOME/.config/guix/current/share/info:$INFOPATH"
	```

Mettez à jour le système guix. Attendez que le processus se finit:

	`guix system reconfigure /etc/config.scm`

Redémarrez la machine:

	`reboot`

Conclusion
==========

Maintenant, tout devrait être profilé. Vous pouvez continuer
à démarrer normalement sans avoir intervention manuelle. Vous
pouvez commencer à vous authentifier en tant qu'utilisateur
normal avec l'"username" en question.

Vous auriez à périodiquement mettre votre système à jour
(quand le temps vous le permet): authentifiez-vous en tant que
supertutilisateur (root) et reproduisez la partie mise à jour
de la section post-installation de ce guide, afin de garder
la disttribution et le système guix à jour.

C'est tout! Vous avez maintenant mis en place un système Guix
avec le chiffrement du disque tout entier, sur votre machine
fonctionnant sous libreboot. Profitez !

Réferences
==========

[1] Manuel Guix (http://guix.gnu.org/manual/fr/).

[2] Documentation Libreboot (https://libreboot.org/docs/).

Acknowledgements
================

[1] Merci au développeur de Guix, Clement Lassieur (clement@lassieur.org),
pour m'avoir aidé avec le code Guile Scheme pour la configuration du chargeur
d'amorçage.

[2] Merci à la fondatrice et développeuse du projet Libreboot,
Leah Rowe (leah@libreboot.org), pour m'avoir aidé à mieux comprendre les fonctionnalités
de libreboot.

License
=======

Copyright (C) 2019  RAGHAV "RG" GURURAJAN (raghavgururajan@disroot.org).

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md) et 
[ici](https://www.gnu.org/licenses/fdl-1.3.en.html").
