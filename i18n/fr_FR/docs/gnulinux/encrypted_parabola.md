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

### Paramétrer les modules de Kernel
Maintenant nous avons besoin d'être sûr que le kernel a tous les modules qui lui
faut pour démarrer le système d'exploitation. Pour faire ça, nous avons besoin d'éditer
un fichier appelé **mkinitcpio.conf**.
Plus d'information à propos de ce fichier peut être trouvé [dans le guide du débutant dans
Parabola](https://wiki.parabola.nu/Mkinitcpio), mais pour ce guide, vous avez simplement
besoin d'exécuter la commande suivante.

    # nano /etc/mkinitcpio.conf

Il y a plusieurs modifications que nous avons besoin de faire au fichier:

1.  Changer la valeur de la ligne décommentée `MODULES` par `i915`.
    
    * Celà force le pilote à se charger plus tôt, comme ça la police d'écriture de
      la console que vous avez sélectionnée plus tôt n'est pas effacé après s'être
      authentifié.
    * Si vous utilisez un **Macbook 2,1** vous allez avoir besoin d'ajouter `hid-generic`,
    `hid`, et `hid-apple` à l'intérieur des guillements, afin d'avoir un clavier qui marche
     quand on vous demandera de rentrer le mot de passe LUKS.
     Soyez sûr de séparer chaque module par un espace.

2.  Changez la valeur de la ligne décommentée `HOOKS` parle suivant:

    ~~~
    base udev autodetect modconf block keyboard keymap consolefont encrypt lvm2 filesystems fsck shutdown
    ~~~

    Voici ce que chaque module fait:

    * `keymap` ajoute à *initramfs* la disposition clavier spécifiée dans **/etc/vconsole.conf**
    * `consolefont` ajoute à *initramfs* la police d'écriture spécifiée dans **/etc/vconsole.conf**
    * `encrypt` ajoute le support de LUKS à l'initramfs - nécessaire pour dévérouiller vos disques au démarrage.
    * `lvm2` ajoute le support de LVM à l'initramfs - nécessaire pour monter les partitions LVM au démarrage .
    * `shutdown` est nécessaire, selon le wiki de Parabola, pour démonter les périphériques (tels que LUKS/LVM) pendant l'extinction.

Après avoir modifié le fichier et l'avoir enregistré, nous avons besoin de mettre à jour le(s)
kernel(s) avec ces nouveaux paramètres. Avant de faire ça, nous voulons installer un kernel Support
à Long Terme (Long-Term Support ou LTS) en tant que sauvegarde, dans le cas où nous recontrons des
problèmes avec le kernel Linux-Libre par défaut (qui est mis à jour continuellement).

Nous installerons aussi le paquet `grub` dont aurons besoin plus tard
afin de faire des modifitions au fichier de configuration GRUB:

    # pacman -S linux-libre-lts grub

Ensuite, nous mettons à jour les kernels en utilisant la commande
`mkinitcpio`:

    # mkinitcpio -p linux-libre
    # mkinitcpio -p linux-libre-lts

### Paramétrer le nom d'hôte
Maintenant nous avons besoin de configurer le nom d'hôte pour le système; de
ce fait notre appareil peut être identifié par le réseau. Référer vous à la [section
sur le nom d'hôte](https://wiki.parabola.nu/Beginners%27_guide#Hostname) du guide du débutant
dans Parabola. Vous pouvez choisir le nom d'hôte comme vous le voulez; par example, si vous
voulez choisir **parabola** en tant que nom d'hôte, vous aurez a exécuter la commande `echo`
comme ceci:

    # echo parabola > /etc/hostname

Puis ensuite vous aurez à modifier le fichier **/etc/hosts** en y ajoutant votre nom d'hôte:

    # nano /etc/hosts

    #<ip-address> <hostname.domain.org>   <hostname>
    127.0.0.1     localhost.localdomain   localhost   parabola
    ::1           localhost.localdomain   localhost   parabola

### Configurer le réseau
Maintenant que nous avons un nom d'hôte, nous avons besoin de configurer les paramètres
pour le reste du réseau.
Les instructions pour configurer une connexion filaire sont [dans le
guide du Parabola](https://wiki.parabola.nu/Beginners%27_guide#Wired), et les instructions pour
les connexions sans fil [y sont aussi](https://wiki.parabola.nu/Beginners%27_guide#Wireless_2).

### Définir le mot de passe root
Le compte **root** a le contrôle sur tous les fichiers de l'ordinateur; par sécurité,
nous voudrons le protéger avec un mot de passe. Les exigences de mot de passe données
au-dessus pour la phrase de passe LUKS, s'applique ici aussi.
Vous définirez le mot de passe avec la commande `passwd`:

    # passwd

### Fignolages supplémentaires pour la sécurité
Il y a quelques changements que nous pouvons faire à l'installation pour la
rendre nettement plus sécurisée; ils sont basés sur la section [Sécurité](https://wiki.arc
hlinux.org/index.php/Security) du wiki d'Arch.

### Renforcement de clé
Nous voudrons ouvrir le fichier de configuration des mots de passes,
et améliorer la défense de notre mot de passe **root**:

    # nano /etc/pam.d/passwd

Ajoutez `rounds=65536` à la fin de la ligne 'password' décommentée; pour faire simple,
ça forcera un attaquant à prendre plus de temps après chaque tentative, mitigeant la
menace des attaques de force brute.

#### Restreindre l'accés des dossiers importants
Vous pouvez empêcher n'importe quel utilisateur autre que root à accéder aux dossiers
les plus important du système, en utilisant la commande `chmod`; pour en apprendre plus sur
celle ci, exécutez `man chmod`:

    # chmod 700 /boot /etc/{iptables,arptables}

#### Rejeter l'utilisateur après trois mauvaises tentatives de connexion
Nous pouvons aussi configurer le système de façon à verrouiller le compte
utilisateur après trois mauvaises tentatives de connexion.

Pour faire ça, nous aurons besoin d'éditer le fichier **/etc/pam.d/system-login**,
et de décommenter cette ligne:

    auth required pam\_tally.so onerr=succeed file=/var/log/faillog*\

Vous pouvez aussi juste la supprimer. Au-dessus, mettez la ligne suivante:

    auth required pam\_tally.so deny=2 unlock\_time=600 onerr=succeed file=/var/log/faillog

Cette configuration mettra dehors l'utilisateur pendant dix minutes.
Vous pouvez dévérouillez un compte utilisateur manuellement, en tant que 
root, avec cette commande:

    # pam_tally --user *lenomdutilisateur* --reset

#### Générer grub.cfg
Éditez le fichier de configuration `/etc/default/grub`, en vous souvenant d'utiliser les UUID quand
vous pointez vers une partition MBR/GPT.
Utilisez `blkid` pour obtenir une liste des périphériques avec
leurs UUIDs respectifs.
Pour les détails, voyez [le wiki de Parabola.](https://wiki.parabola.nu/Dm-crypt/Encrypting_an_entire_s
ystem#Configuring_the_boot_loader_5).

Ensuite, générez grub.cfg avec:

    # grub-mkconfig -o /boot/grub/grub.cfg

Si vous avez une partition `/boot` séparée, n'oubliez pas d'ajouter le lien symbolique (symbolien) pointant
vers le répertoire en cours

    # cd /boot; ln -s . boot

## Démonter toutes les partitions et redémarrer
Félicitations ! Vous avez fini l'installation de Parabola GNU+Linux-Libre.
Il est maintenant temps de redémarrer le système, mais premièrement,
il y a quelques étapes préliminaires:

Quittez le `chroot`, en utilisant la commande `exit`:

    # exit

Démonter toutes les partitions dans **/mnt**, et "débranchez" le volume de swap:

    # umount -R /mnt
    # swapoff -a

Désactiver les volumes logiques **rootvol** et **swapvol** :

    # lvchange -an /dev/matrix/rootvol
    # lvchange -an /dev/matrix/swapvol

Verouillez la partition chiffrée (fermez-là):

    # cryptsetup luksClose lvm

Éteignez la machine:

    # shutdown -h now

Après que la machine soit éteinte, enlevez le média d'installation et démarrez la.

## Démarrer l'installation manuellement depuis GRUB
Quand vous oubliez de configurer ou vous configurer mal grub sur votre disque, vous devez
démarrer manuellement le système en entrant une série de commandes dans le terminal GRUB.


Après que l'ordinateur démarre, pressez `C` pour faire apparaître la ligne
de commande GRUB.
Vous pouvez démarrer soit le kernel normal ou le LTS qu'on a installé; voici
les commandes pour le kernel normal:

    grub> cryptomount -a
    grub> set root='lvm/matrix-rootvol'
    grub> linux /boot/vmlinuz-linux-libre root=/dev/matrix/rootvol cryptdevice=/dev/sda1:root
    grub> initrd /boot/initramfs-linux-libre.img
    grub> boot

Si vous essayez de démarrer le kernel LTS, ajoutez simplement **-lts** à la
fin de chaque commande contenant le kernel (p.e., **/boot/vmlinuz-linux-libre**
deviendrait **/boot/vmlinuz-linux-libre-lts**).

**NOTE: sur des machines avec SATA natif, un lecteur de disque optique (comme les dvd) peut causer
l'échec/le gel de la commande ** `cryptomount -a` **, ainsi que l'erreur** `AHCI transfer timed out` **
La solution de contournement a été d'enlever le lecteur.**

## Tutorial de suivi: Configurer Parabola
La prochaine étape du processus de préparation est de modifier le fichier
de configuration que GRUB utilise, comme ça nous n'avons pas à taper les commandes
ci-dessus à chaque fois que nous voulons démarrer notre système.

Pour rendre ce processus plus facile, nous avons besoin d'installer une 
interface graphique, ainsi que d'autre paquets qui rendront le système plus 
ergonomique.
Ces additions réduiront nettement les probabilités de "briquer/bousiller" notre
ordinateur.

Le document "[Configurer Parabola (après installation)](configuring_parabola.md)" fournit une
préparation à titre d'exemple; mais vous ne sentez pas obligé de le suivre à la lettre (vous pouvez
si vous le voulez); Parabola est centré sur l'utilisateur est très customisable, ce qui veut dire
que vous avez le contrôle total sur le système, et un nombre sans limite d'options pour le mettre en place.
Pour plus d'informations, lisez [La façon Arch](https://wiki.archlinux.fr/Arch_Linux#Philosophie) (Parabola la suit aussi).


Copyright © 2014, 2015, 2016 Leah Rowe <info@minifree.org>

Copyright © 2015 Jeroen Quint <jezza@diplomail.ch>

Copyright © 2017 Elijah Smith <esmith1412@posteo.net>

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
