---
title: Installer Parabola ou Arch GNU+Linux-Libre, avec le chiffrement du disque tout entier (incluant /boot)
x-toc-enable: true
...

Consultez aussi:
[Installer Parabola ou Arch GNU+Linux-Libre, avec le chiffrement du disque tout entier (incluant /boot)](https://wiki.hyperbola.info/en:guide:encrypted_installation)

Ce guide couvre comment installer Parabola GNU+Linux-libre, avec le chiffrement
du disque tout entier, incluant **/boot** (le répertoire de démarrage). Sur la
majorité des systèmes, **/boot** doit être laissé non chiffré pendant que l'/les
autre(s) partition(s) est/sont chiffrée(s).
C'est ainsi que GRUB (et alors le kernel) peut être chargé et éxecuté,
parce que la majorité des micrologiciels ne peuvent pas ouvrir un 
volume LUKS; cependant, avec libreboot, GRUB est déjà inclus en tant que
[charge utile](http://www.coreboot.org/Payloads#GRUB_2), donc même **/boot**
peut être chiffré; ça protège **/boot** d'altérations par quelqu'un ayant
un accés physique à la machine.

**NOTE: Ce guide est *seulement* pour la charge utile GRUB.
Si vous utilisez la charge utile depthcharge, ignorez entièrement
cette section.**

Ce guide s'inspire massivement du wiki de Parabola, et le référencera constamment.
Pour les novices de Parabola GNU+Linux-Libre, jetez un coup d'oeil à leur
[section Débutants](https://wiki.parabola.nu/Beginners%27_guide#Beginners) pour une
vue d'ensemble.

## Configuration minimale requise
Vous pouvez trouver la configuration minimale requise
pour éxecuter et faire marcher Parabola GNU+Linux 
[sur le wiki de Parabola](https://wiki.parabola.nu/Beginners%27_guide#Minimum_system_requirements).

## Préparation

### Télécharger la dernière ISO
Pour ce guide j'ai utilisé l'ISO *2016.11.03*; l'image CD la plus
récente est disponible sur la [page des téléchargements](https://wiki.parabola.nu/Get_Parabola#Main_live_ISO://wiki.parabola.nu/Get_Parabola#Release_images_for_x86_64_and_i686_architectures) de Para-
-bola.

NdT: j'ai mis à jour les liens car le wiki a changé depuis.

Si vous êtes un débutant complet avec GNU+Linux, choisissez le
*LXDE Desktop ISO*. C'est plus facile d'installer Parabola avec
cette version parce qu'elle vous permet d'accéder un navigateur
web, comme ça vous pouvez copier-coller les commandes directement
dans le terminal, sans se soucier des fautes de frappe.

**NOTE: Vous ne devriez jamais copier-coller aveuglément n'importe
quelles commandes. Dans ce guide, le copie-collage permet de s'assurer
que vous ne faites pas d'erreurs quand vous entrez les commandes, et donc
que vous ne 'briquer' pas votre installation, et devoir tout recommencer.
C'est important de savoir ce que chaque commande fait avant que l'utilisiez,
soyez donc sûr pour chacune d'entre elles de lire la documentation du wiki
Parabola/Arch sur celles-ci, ainsi que sa page** `man` **.**

Si vous n'êtes pas un débutant, choisissez le *Main Live ISO*

Choisissez seulement le *TalkingParabola ISO*, si vous êtes aveugle
ou malvoyant.

### Choisir le périphérique d'installation
Référez vous au wiki Parabolab pour trouver le bon périphérique d'installation,
que vous utilisiez soit un [disque optique](https://wiki.parabola.nu/Beginners%27_guide#Optical_Disks)
ou une [clef USB](https://wiki.parabola.nu/Beginners%27_guide#USB_flash_drive).

## Démarrer l'environnement d'installation de Parabola
Après avoir téléchargé l'ISO et créé une sorte de périphérique démarrable,
vous aurez besoin de démarrer dans l'image Live. Si vous n'êtes pas sûr de savoir
comment faire, voyez [Comment démarrer un installeur GNU+Linux](grub_boot_installer.md) 
puis passez à l'étape suivante; sinon, passez juste à l'étape suivante.

Une fois que vous avez démarré dans l'environnement, ouvrez soit le **`Terminal LXDE`**
(si vous utilisez l'ISO Bureau LXDE), ou juste rentrez simplement les commandes listées
ci-dessous (si vous utilisez n'importe quel autre ISO).

## Configurer la disposition du clavier
Pour bien commencer l'installation, vous devez d'abord choisir
la bonne [disposition de votre clavier](https://wiki.parabola.nu/Beginners%27_guide#Changing_Keyboard).

## Établir une connexion Internet
Vous aurez aussi besoin [de configurer une connexion réseau](https://wiki.parabola.nu/Beginners%27_guide#Establish_an_internet_connection) 
pour installer des paquets.

## Préparer le périphérique de stockage pour l'installation

Vous allez avoir besoin de préparer le périphérique de stockage que nous utiliserons
pour installer le système d'exploitation.
Vous pouvez utiliser la même méthode pour trouver le [nom du périphérique](https://wiki.parabola.nu/Beginners%27_guide#USB_flash_drive)
qu'on a exécutée plus tôt afin de déterminer le périphérique d'installation pour l'ISO.

### Effacer le périphérique de stockage

Vous voulez vous rendre sur que le périphérique de stockage que vous utilisiez ne 
contient aucune copie de vos données personnelles non chiffrée. Si le disque est
nouveau, alors vous pouvez ignorer le reste de cette section; sinon, il y a deux
manières de s'en occuper:

1. Si le périphérique n'était pas chiffré précédemment, effacez-le de façon sécurisée
avec la commande 'dd'; vous pouvez choisir de le remplir de zéros ou de données aléatoire;
je choisis les données aléatoires (p.e., `urandom`), parce que c'est plus sécurisé. Dépendant 
de la taille du disque, ça pourrait prendre un moment à se finir:
    
    ~~~
    # dd if=/dev/urandom of=/dev/sdX; sync
    ~~~

2. Si le périphérique était chiffré précédemment, tout ce que vous avez besoin de faire
est d'effacer l'en-tête LUKS.
La taille de l'en-tête dépend du modèle spécifique du disque dur; vous pouvez trouver cette 
information en faisant quelques recherches en ligne.
Référez-vous à cet [article](https://www.lisenet.com/2013/luks-add-keys-backup-and-restore-volume-header/)
pour plus d'informations sur les en-têtes LUKS.
Vous pouvez soit remplir l'en-tête avec des zéros, ou soit avec des données aléatoires; encore une fois je
choisis les données aléatoires, en utilisant `urandom`:

    ~~~
    # head -c 3145728 /dev/urandom > /dev/sdX; sync
    ~~~

Par ailleurs, si vous utilisez un SSD, il y a deux choses que vous devez garder en tête:
-    Il y a des problèmes avec TRIM; ce n'est pas activé par défaut à travers LUKS, et
il y a des problèmes de sécurité si vous l'activez. Voyez [cette page](https://wiki.archlinux.org/index.php/Dm-crypt#Specialties)
pour plus d'infos.
-    Soyez bien sûr de lire [cet article](https://wiki.archlinux.org/index.php/Solid_State_Drives), pour des informations sur
la gestion des SSDs dans Arch Linux (les informations s'appliquent aussi à Parabola).

### Formater le périphérique de stockage
Maitenant que toutes vos données personnelles ont été supprimée du disque, il est temps de le formater.
Nous commencerons en créant dessus une seule grande partition, et nous la chiffrerons en utilisant LUKS.

### Créer la partition LUKS
Vous allez avoir besoin du module de kernel `device-mapper` pendant l'installation;
ça nous permettra de mettre en place notre disque chiffré. Pour le charger, utilisez
la commande suivante:

    # modprobe dm_mod

Nous avons besoin ensuite de sélectionner le **nom de périphérique** du disque sur lequel nous
allons installer le système d'exploitation; voyez la méthode ci-dessus, si besoin, pour trouver
les noms de périphérique.

Maintenant que nous avons le nom correct du périphérique, nous avons besoin de créer la partition dessus.
Pour celà, nous utiliserons la commande `cfdisk`:

    # cfdisk /dev/sdX

1. Utilisez les flèches directionnelles pour sélectionner votre partition, et si il y
a déjà une partition sur le disque, sélectionnez **Supprimer**, et ensuite **Nouvelle**.
2. Pour la taille de la partition, laissez là par défaut : c'est le disque en entier.
3. Vous verrez une option pour **Primaire** ou **Logique**; choisissez **Primaire**, et
soyez sûr que le type de cette partition est **Linux (83)**.
4. Sélectionnez **Écrire**; ça vous demandera si vous êtes sûr que vous voulez écraser
le disque.
5. Tapez **yes** et pressez Entrée. Un message apparaîtra en bas vous disant que la table
de partition a été altérée.
6. Sélectionnez **Quitter**, pour vous rendre sur le terminal principal.

Maintenant que vous avez créé la partition, il est temps de créer le volume chiffré dessus
en utilisant la commande `cryptsetup`comme il suit:

    # cryptsetup -v --cipher serpent-xts-plain64 --key-size 512 --hash whirlpool \
    --iter-time 500 --use-random --verify-passphrase --type luks1 luksFormat /dev/sdXY

Ce sont juste les défauts recommandés; si vous voulez utiliser quoi que ce soit
d'autre ou de trouver quelles sont les options, exécutez `man cryptsetup`.

These are just recommended defaults; if you want to juse anything else,
or to find out what options there are, run `man cryptsetup`.

**NOTE: le temps d'itération par défaut est de 2000ms (2 secondes)
si celui-ci n'est pas spécifié lors de l'exécution de la commande
cryptsetup. Vous devriez définir un temps plus bas que ça; sinon
il y aura un délai d'approximativement 20 secondes quand vous allez
démarrer votre système. Nous recommandons 500ms (0.5 secondes), et ceci
est inclus dans la commande ** `cryptsetup` ** préparée au-dessus. Gardez
à l'esprit que le temps d'itération est là dans des buts de sécurité (ça mitige
les attaques de force brute), donc quoi que ce soit en dessous de 0.5 secondes
n'est probablement pas très sécurisé.**

Vous serez maintenant invité à entrer une phrase de passe; soyez sûr qu'elle soit
*sécurisée*. Pour la sécurité d'une phrase de passe, la longueur est plus importante
que la complexité (p.e., **correct-horse-battery-staple** est plus sécurisé que
**bf20$3Jhy3**), mais ça aide d'inclure quelques types de charactères différents
(p.e., lettres maju/minuscules, nombres, charactères spéciaux).
La longueur du mot de passe devrait être aussi longue que vous pouvez vous en 
rappeler, sans avoir à l'écrire, où la stocker physiquement nulle part.

L'utilisation de la méthode du [**lancer de dés**](http://world.std.com/~reinhold/diceware.html)
est recommandée pour la génération de phrases de passe sécurisées (au lieu de mots de passe).

#### Créer le groupe de Volume et les Volumes Logique
L'étape suivante est de créer deux Volumes Logiques à l'intérieur de la
partition LUKS chiffrée: une contiendra votre installation principale, et 
l'autre contiendra votre espace swap.

Nous créerons celà en utilisant le [Logical Volume Manager (LVM)](https://wiki.archlinux.org/index.php/LVM).

Premièrement, nous avons besoin d'ouvrir la partition LUKS, à **/dev/mapper/lvm**:

    # cryptsetup luksOpen /dev/sdXY lvm

Ensuite, nous créons une partition LVM:

    # pvcreate /dev/mapper/lvm

Vérifiez pour être sûr que la partition a été créée:

    # pvdisplay

Après, nous créons le groupe de volumes, à l'intérieur duquel
les volumes logique seront créés. Dans le cas de libreboot, nous
appelerons ce groupe **matrix**. Si vous voulez le faire marcher via
*Charger un système d'exploitation (incluant les disques complètement
chiffrés) [o]* il a besoin d'être appelé **matrix** ( comme c'est codé
dans le dur du grub.cfg de libreboot lors du flash)

    # vgcreate matrix /dev/mapper/lvm

Assurez-vous que le groupe a été créé:

    # vgdisplay

Dernièrement, nous avons besoin de créer les volumes logique eux-mêmes à l'intérieur
du groupe de volumes; un sera notre swap, intelligemment nommée **swapvol**, et l'autre
sera notre partition root, également intelligemment nommée **rootvol**.

1. Nous créerons le **swapvol** en premier (rappelons-le, choisissez le nom qui vous 
convient si vous le voulez).
Aussi, soyez sûr de [choisir une taille de swap appropriée](http://www.linux.com/news/software/applications/8208-all-about-linux-swap-space)
(dans cet example, **2G** fait référence à deux gigaoctets; changez celà comme il
convient):
    
    ~~~
    # lvcreate -L 2G matrix -n swapvol
    ~~~

2. Maintenant, nous créerons une seule grande partition dans l'espace restant, pour **rootvol**:

    ~~~
    # lvcreate -l +100%FREE matrix -n rootvol
    ~~~

Vous pouvez aussi être flexible ici, par example vous pouvez spécifier un volume
**/home**, **/var** or **/usr**. Par exemple, si vous allez faire marcher un serveur
web/mail alors vous allez vouloir **/var** (ou les fichiers journaux sont stockés) dans
sa propre partition, comme ça si ça se remplit de journaux, ça ne plantera pas votre
système.
Pour un système d'ordi portable / pc de bureau (cas typique), juste un root et un swap 
fera l'affaire.

Vérifiez que les volumes logique ont été correctement créés:

    # lvdisplay

#### Rendre les partitions rootvol et swapvol prêtes pour l'installation
Les dernières étapes de la préparation du disque pour l'installation sont
de faire de **swapvol** une partition swap active, et formater **rootvol**.

    # mkswap /dev/mapper/matrix-swapvol

Activez le **swapvol**, le permettant d'être maintenant utilisé
en tant que swap, grâce à la commande `swapon` :

    # swapon /dev/matrix/swapvol

Maintenant nous avons besoin de formater **rootvol** pour préparer
à l'installation; nous allons le faire avec la commande `mkfs` (make
filesystem: construire un système de fichiers).
Nous choisirons le système de fichiers **ext4**, mais vous pouvez utilisez
un différent selon votre cas d'utilisation:

    # mkfs.ext4 /dev/mapper/matrix-rootvol

Dernièrement, on a besoin de monter **rootvol**. Heuresement, GNU+Linux a
un répertoire pour ce but précis, **/mnt**:

    # mount /dev/matrix/rootvol /mnt

#### Séparer les volumes logiques boot et home
Vous pouvez aussi créer deux volumes logiques séparés pour **/boot** et **/home**,
mais une telle configuration serait pour des utilisateurs avancés, et donc pas pris
en charge dans ce guide.
Si un volume logique de démarrage séparé est utilisé, il doit être nommé **boot**
afin que libreboot l'utilise.

La configuration du disque et des partitions est maintenant complète; il est temps de
finalement installer Parabola.

## Sélectionner un mirroir
La première étape de la véritable installation est de choisir le serveur d'où
nous allons avoir besoin de télécharger les paquets; pour celà nous nous référerons 
encore une fois au [Wiki de Parabola](https://wiki.parabola.nu/Beginners%27_guide#Select_a_mirror).
Pour les débutants, nous recommandons d'éditer le fichier en utilisant `nano` (un éditeur de texte
en ligne de commande); vous pouvez en savoir plus sur [leur site web](https://www.nano-editor.org/);
pour les non débutants, éditez le simplement dans votre éditeur de texte préféré.

## Installer le système de base
Vous allez avoir besoin d'installer les applications essentielles nécessaire à l'éxecution de votre
installation de Parabola; référez-vous à [Installer le système de base](https://wiki.parabola.nu/Beginners%27_guide#Install_the_base_system)
sur le wiki de Parabola.

## Générer un fstab
La prochaine étape du procédé est de générer un fichier connu sous le nom de **fstab**;
le but de ce fichier est de permettre au système d'exploitation d'identifier le périphérique
de stockage utilisé par votre installation.
[Sur le guide du débutant dans Parabola](https://wiki.parabola.nu/Beginners%27_guide#Generate_an_fstab)
figure les instructions pour créer ce fichier.

## 'Chrooter' dans le système et le configurer
Maintenant, vous avez besoin de `chroot` dans votre nouvelle installation
pour finaliser le processus de préparation et d'installation. Le **Chrooting**
réfère au changement du répertoire racine (root) d'un système d'exploitation
vers un autre répertoire; dans ce cas là, celà signifie changer votre répertoire
racine sur celui que vous avez créé dans les étapes précédentes, comme ça vous pouvez
modifier des fichiers et installer des logiciels, comme ci vous étiez le système d'exploitation
hôte.

Pour `chroot` dans votre installation, suivez les instructions [dans le guide du débutant
dans Parabola](https://wiki.parabola.nu/Beginners%27_guide#Chroot_and_configure_the_base_system).

### Paramétrer la localisation
La localisation réfère au language que votre système d'exploitation utilisera, ainsi que
d'autres considérations en relation avec la région dans laquelle vous vivez.
Pour mettre ça en place, suivez les instructions dans le [guide du débutant dans
Parabola](https://wiki.parabola.nu/Beginners%27_guide#Locale).

### Paramétrer la police d'écriture de la console (Consolefont) et la disposition clavier
Celà déterminera la disposition clavier de votre nouvelle installation; suivez
les instructions [dans le guide du débutant dans Parabola](https://wiki.parabola.nu/Beginners%27_
guide#Console_font_and_keymap).

### Paramétrer le fuseau horaire
Vous allez avoir besoin de définir votre fuseau horaire actuel dans le système d'exploitation; ça
permettra aux applications ayant besoin d'une heure précise pour fonctionner proprement (p.e., les
navigateurs web).
Pour faire ceci, suivez les instructions [dans le guide du débutant dans Parabola](https://wiki.parabola.nu/Be
ginners%27_guide#Time_zone).

### Paramétrer l'horloge matérielle
Pour être sur que votre ordinateur est à la bonne heure, vous allez avoir à définir le temps
de l'horloge interne de l'ordinateur.
Suivez les instructions [dans le guide du débutant dans Parabola](https://wiki.parabola.nu/Beginner
s%27_guide#Hardware_clock) pour le faire.

### Setting up the Kernel Modules
Now we need to make sure that the kernel has all the modules that it needs
to boot the operating system. To do this, we need to edit a file called **mkinitcpio.conf**.
More information about this file can be found [in the Parabola beginner's guide](https://wiki.parabola.nu/Mkinitcpio),
but for the sake of this guide, you simply need to run the following command.

    # nano /etc/mkinitcpio.conf

There are several modifications that we need to make to the file:

1.  Change the value of the uncommented `MODULES` line to `i915`.

    * This forces the driver to load earlier, so that the console font you selected earlier
      isn’t wiped out after getting to login.
    * If you are using a **Macbook 2,1** you will also need to add `hid-generic`,
      `hid`, and `hid-apple` inside the quotation marks, in order to have
      a working keyboard when asked to enter the LUKS password.
      Make sure to separate each module by one space.

2.  Change the value of the uncommented `HOOKS` line to the following:

    ~~~
    base udev autodetect modconf block keyboard keymap consolefont encrypt lvm2 filesystems fsck shutdown
    ~~~

    here's what each module does:

    * `keymap` adds to *initramfs* the keymap that you specified in **/etc/vconsole.conf**
    * `consolefont` adds to *initramfs* the font that you specified in **/etc/vconsole.conf**
    * `encrypt` adds LUKS support to the initramfs - needed to unlock your disks at boot time
    * `lvm2` adds LVM support to the initramfs - needed to mount the LVM partitions at boot time
    * `shutdown` is needed according to Parabola wiki, for unmounting devices (such as LUKS/LVM) during shutdown

After modifying the file and saving it, we need to update the kernel(s) with the new settings.
Before doing this, we want to install a Long-Term Support (LTS) kernel as a backup, in the event
that we encounter problems with the default Linux-Libre kernel (which is continually updated).

We will also install the `grub` package, which we will need later,
to make our modifications to the GRUB configuration file:

    # pacman -S linux-libre-lts grub

Then, we update both kernels like this, using the `mkinitcpio` command:

    # mkinitcpio -p linux-libre
    # mkinitcpio -p linux-libre-lts

### Setting up the Hostname
Now we need to set up the hostname for the system; this is so that our device
can be identified by the network. Refer to [the hostname section](https://wiki.parabola.nu/Beginners%27_guide#Hostname)
of the Parabola wiki's Beginner's Guide. You can make the hostname anything you like;
for example, if you wanted to choose the hostname **parabola**,
you would run the `echo` command, like this:

    # echo parabola > /etc/hostname

And then you would modify **/etc/hosts** like this, adding the hostname to it:

    # nano /etc/hosts

    #<ip-address> <hostname.domain.org>   <hostname>
    127.0.0.1     localhost.localdomain   localhost   parabola
    ::1           localhost.localdomain   localhost   parabola

### Configure the Network
Now that we have a hostname, we need to configure the settings for the rest of the network.
Instructions for setting up a wired connection are [in the Parabola beginner's guide](https://wiki.parabola.nu/Beginners%27_guide#Wired),
and instructions for setting up a wireless connection are [in the Parabola beginner's guide](https://wiki.parabola.nu/Beginners%27_guide#Wireless_2).

### Set the root Password
The **root** account has control over all the files in the computer; for security,
we want to protect it with a password. The password requirements given above,
for the LUKS passphrase, apply here as well. You will set this password with the `passwd` command:

    # passwd

### Extra Security Tweaks
There are some final changes that we can make to the installation, to make it
significantly more secure; these are based on the [Security](https://wiki.archlinux.org/index.php/Security) section of the Arch wiki.

#### Key Strengthening
We will want to open the configuration file for password settings, and increase
the strength of our **root** password:

    # nano /etc/pam.d/passwd

Add `rounds=65536` at the end of the uncommented 'password' line; in simple terms,
this will force an attacker to take more time with each password guess, mitigating
the threat of brute force attacks.

#### Restrict Access to Important Directories
You can prevent any user, other than the root user, from accessing the most important
directories in the system, using the `chmod` command; to learn more about this command,
run `man chmod`:

    # chmod 700 /boot /etc/{iptables,arptables}

#### Lockout User After Three Failed Login Attempts
We can also setup the system to lock a user's account, after three failed login attempts.

To do this, we will need to edit the file **/etc/pam.d/system-login**,
and comment out this line:

    auth required pam\_tally.so onerr=succeed file=/var/log/faillog*\

You could also just delete it. Above it, put the following line:

    auth required pam\_tally.so deny=2 unlock\_time=600 onerr=succeed file=/var/log/faillog

This configuration will lock the user out for ten minutes.
You can unlock a user's account manually, using the **root** account, with this command:

    # pam_tally --user *theusername* --reset
    
#### Generate grub.cfg
Edit configuration in `/etc/default/grub`, remembering to use UUID when poitning to mbr/gpt partition.
Use `blkid` to get list of devices with their respective UUIDs.
For details see [parabola wiki.](https://wiki.parabola.nu/Dm-crypt/Encrypting_an_entire_system#Configuring_the_boot_loader_5)

Next generate grub.cfg with:

    # grub-mkconfig -o /boot/grub/grub.cfg
    
If you have separate `/boot` partition, don't forget to add `boot` symlink inside that points to current directory

    # cd /boot; ln -s . boot

## Unmount All Partitions and Reboot
Congratulations! You have finished the installation of Parabola GNU+Linux-Libre.
Now it is time to reboot the system, but first, there are several preliminary steps:

Exit from `chroot`, using the `exit` command:

    # exit

Unmount all of the partitions from **/mnt**, and "turn off" the swap volume:

    # umount -R /mnt
    # swapoff -a

Deactivate the **rootvol** and **swapvol** logical volumes:

    # lvchange -an /dev/matrix/rootvol
    # lvchange -an /dev/matrix/swapvol

Lock the encrypted partition (i.e., close it):

    # cryptsetup luksClose lvm

Shutdown the machine:

    # shutdown -h now

After the machine is off, remove the installation media, and turn it on.

## Booting the installation manually from GRUB
When you forget to configure or misconfigure grub on your hdd, you have to manually boot
the system by entering a series of commands into the GRUB command line.


After the computer starts, Press `C` to bring up the GRUB command line.
You can either boot the normal kernel, or the LTS kernel we installed;
here are the commands for the normal kernel:

    grub> cryptomount -a
    grub> set root='lvm/matrix-rootvol'
    grub> linux /boot/vmlinuz-linux-libre root=/dev/matrix/rootvol cryptdevice=/dev/sda1:root
    grub> initrd /boot/initramfs-linux-libre.img
    grub> boot

If you're trying to boot the LTS kernel, simply add **-lts** to the end
of each command that contains the kernel (e.g., **/boot/vmlinuz-linux-libre**
would be **/boot/vmlinuz/linux-libre-lts**).

**NOTE: on machines with native sata, during boot a (faulty) optical disc drive (like dvd) can cause
the** `cryptomount -a` **command to fail/hang, as well as the error** `AHCI transfer timed out`
**The workaround was to remove the DVD drive.**

## Follow-Up Tutorial: Configuring Parabola
The next step of the setup process is to modify the configuration file that
GRUB uses, so that we don't have to manually type in those commands above, each time we want
to boot our system.

To make this process much easier, we need to install a graphical interface,
as well as install some other packages that will make the system more user-friendly.
These additions will also sharply reduce the probability of "bricking" our computer.

[Configuring Parabola (Post-Install)](configuring_parabola.md) provides an example setup, but don't feel
as if you must follow it verbatim (of course, you can, if you want to);
Parabola is user-centric and very customizable, which means that you have maximum control
of the system, and a near-limitless number of options for setting it up. For more information,
read [The Arch Way](https://wiki.archlinux.org/index.php/The_Arch_Way) (Parabola also follows it).

After setting up the graphical interface, refer to [How to Modify GRUB Configuration](grub_cbfs.md),
for instructions on doing just that, as well as flashing the ROM (if necessary).

Copyright © 2014, 2015, 2016 Leah Rowe <info@minifree.org>

Copyright © 2015 Jeroen Quint <jezza@diplomail.ch>

Copyright © 2017 Elijah Smith <esmith1412@posteo.net>

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License Version 1.3 or any later
version published by the Free Software Foundation
with no Invariant Sections, no Front Cover Texts, and no Back Cover Texts.
A copy of this license is found in [../fdl-1.3.md](../fdl-1.3.md)

