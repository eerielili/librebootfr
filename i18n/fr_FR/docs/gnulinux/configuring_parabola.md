---
title: Configurer Parabola (Après-Installation)
x-toc-enable: true
...

C'est le guide pour mettre en place Parabola GNU+Linux-Libre après avoir
complété les étapes d'installation vues dans [Installer Parabola ou Arch 
GNU+Linux-Libre, avec le chiffrement du disque tout entier 
(incluant /boot)](encrypted_parabola.md).
Il couvrira l'installation et la configuration d'un environnement de bureau
graphique, ainsi que quelques applications qui rendent le système plus ergonomique.

Dans cet exemple, nous choisissons *l'environnement de bureau MATE* comme notre interface
graphique.

*Ce guide était valide à la date 2017-06-02. Si vous voyez quelconque changement
qui devrait être fait pour aujourd'hui, silvouplaît contacter le projet Libreboot
(ou [faites ces changements vous-mêmes](https://libreboot.org/git.html#editing-the-website-and-documentation-wiki-style))!*

Alors que Parabola peut sembler décourageant au premier abord (surtout pour les
nouveaux utilisateurs GNU+Linux), avec un simple guide, il peut fournir la même
ergonomie que n'importe quelle distribution GNU+Linux basée Debian (p.e., Trisquel, 
Debian, Devuan), sans cacher les détails à l'utilisateur.

Paradoxalement, plus vous devenez expérimenté, plus Parabola devient 
*facile à utiliser*, quand vous voulez mettre en place votre système d'une
façon spéciale, comparé à ce que fournit la majorité des distributions.
Avec le temps, vous allez vous rendre compte que les autres distributions
ont tendance à *se mettre en travers de votre chemin*.

Beaucoup d'étapes dans ce guide se référeront à l'ArchWiki.
Arch est la distribution en amont que Parabola utilise. 
La majorité de ce guide vous dira aussi de lire des articles de wiki,
d'autres pages, des manuels et ainsi de suite.
En général, il essaye de trier sur le volet les informations les
plus utiles, mais cependant, vous êtes encouragé à apprendre autant
que possible.

**NOTE: Ça peut vous prendre quelques jours pour complétement installer
le système de la façon dont vous le voulez, dépendant de combien vous avez 
besoin de lire. La patience est la clé, surtout pour les nouveaux utilisateurs.**

L'ArchWiki utilisera parfois un mauvais language, comme désigner le système par 'Linux',
 utilisant le terme **open-source**/**closed-source**, et recommandera parfois
 l'utilisation de logiciels propriétaires.
 Vous avez besoin de faire attention à celà quand vous lisez quoi que ce soit
 sur l'ArchWiki.

 Quelques étapes nécessitent un accés à Internet. Pour avoir un accés de
 base, initial, à Internet afin de mettre en place le système (on se penchera
 sur le réseau plus tard), connectez juste votre système à un routeur/box via
 un cable ethernet puis exécutez la commande suivante:

    # systemctl start dhcpcd.service

Vous pouvez l'arrêter plus tard si besoin, en utilisant l'option `stop`
de systemd:

    # systemctl stop dhcpcd.service

Pour la majorité des personnes ça devrait être suffisant, mais si vous
pas le DHCP activé sur votre réseau, alors vous devriez mettre en place
votre connection au réseau en premier:
[Mettre en place la connection au réseau dans Parabola](#network).

## Configurer pacman
`pacman` (*pac*kage *man*ager) est le nom du système de gestion des paquets
dans Arch, que Parabola (en tant qu'effort parallèle et déblobbé) utilise
aussi.
Comme avec `apt-get` sur Trisquel, Debian ou Devuan, il peut être utilisé
pour ajouter, supprimer, et mettre à jour les logiciels sur votre
ordinateur.

Pour plus d'information par rapport à `pacman`, lisez les articles suivant sur
l'ArchWiki:

*    [Configurer pacman](https://wiki.parabola.nu/Installation_Guide_%28Fran%C3%A7ais%29#Configurer_Pacman)
*    [Utiliser pacman](https://wiki.archlinux.fr/Pacman)
*    [Dépôts logiciels supplémentaires](https://wiki.parabola.nu/Repositories_%28Fran%C3%A7ais%29)

## Mettre à jour Parabola
Parabola est mis à jour en utilisant `pacman`. Quand vous mettez à jour
Parabola, soyez sûr de rafraîchir la liste des paquets *avant* d'installer quelque mise à jour que ce soit:

    # pacman -Syy

**NOTE: d'après le Wiki,** `-Syy` **est mieux que** `-Sy` **, parce qu'elle
rafraîchit la liste des paquets même si celle-ci apparaît déjà à jour, ce qui
peut être utile lors du basculement sur un autre dépôt mirroir.**

Ensuite, mettons véritablement à jour le système:

    # pacman -Syu

**NOTE: avant d'installer des paquets avec** `pacman -S`**, mettez toujours 
premièrement votre système à jour, en utilisant les deux commandes du dessus.**

Gardez un oeil sur la sortie ou lisez la dans le fichier **/var/log/pacman.log**.
Parfois, `pacman` montrera des messages à propos d'étapes de maintenance
que vous aurez besoin de faire avec certains fichiers (typiquement ceux de
configurations) après la mise à jour.
Aussi, vous devriez jetez un coup d'oeil à la fois la 
[page d'accueil de Parabola](https://www.parabola.nu/) et [celle d'Arch](htt
ps://www.archlinux.org/), pour voir s'il y a mention de quelconque problèmes.
Si un nouveau kernel est installé, vous devriez aussi vous mettre à jour
pour être capable de l'utiliser (le kernel qui est en cours d'exécution fera
aussi bien l'affaire).

C'est une bonne habitude de mettre à jour Parabola d'une à deux fois par semaine.
En tant que distribution en publication continue, ce n'est jamais une bonne idée de
trop laisser vieillir l'installation.
C'est simplement dû à la façon de fonctionner du projet; les vieux paquets sont 
supprimés rapidement des dépôts une fois qu'ils sont mis à jour.
Un système qui n'a pas été mis à jour depuis pas mal de temps, signifiera potentiel-
-lement plus de lecture à travers les précédents billets du site web, et plus de
travaux de maintenance.

Le forum d'Arch peut être utile si d'autres personnes ont le même problème que
vous.
Le canal IRC Parabola ([**\#parabola**](https://webchat.freenode.net/) sur freenode) peut 
aussi vous aider.

Dû à ceci et la nature volatile de Parbola/Arch, vous devriez seulement
mettre à jour quand vous avez au moins quelques heures de libre devant vous, au
cas des problèmes émergent et demandent résolution. Vous ne devriez jamais
mettre à jour, par exemple, si vous avez besoin de votre système pour un évenement
important, comme une présentation, ou d'envoyer un email à une personne importante
avant un date butoir décidée, et ainsi de suite.

Relax! Les paquets sont bien testés quand de nouvelles mises à jour les
amènent dans les dépôts; des dépôts 'testing' existent pour cette exacte
raison (les tester).
Malgrés ce que beaucoup de gens pourrait vous dire, Parabola is plutôt 
stable et sans problème, tant que vous êtes conscient de comment
vérifier un problème, et que vous êtes partant pour prendre un peu de temps
pour réparer ces problèmes, dans les rares cas où ils arrivent (c'est pour celà
qu'Arch/Parabola fournissent une documentation fournie).

## Maintenir Parabola
Parabola est une distribution très simple, dans le sens où vous avez
les pleins pouvoirs, et tout est transparent pour vous.
Une conséquence de ceci est que vous avez besoin de savoir ce que
vous faites, et ce que vous avez fait avant.
En général, garder des notes (comme je l'ai fait avec cette page) peut
être très utile en tant que référence dans le future (p.e., si vous vous
réinstaller, ou installer la distribution sur un autre ordinateur).

Vous devriez aussi lire l'article d'ArchWiki sur la [Maintenance Système](http
s://wiki.archlinux.fr/Maintenance_Syst%C3%A8me) avant de continuer.

Installez `smartmontools`; ça peut être utilisé pour jeter un coup d'oeil aux
données S.M.A.R.T. Les disques durs utilisent des micrologiciels non libres
à l'intéreur d'eux; c'est transparent pour vous mais les données S.M.A.R.T
 viennent de là, donc ne dépendez pas trop de ça, puis ensuite lisez 
[l'article](https://wiki.archlinux.org/index.php/S.M.A.R.T.) de l'ArchWiki 
sur celles-ci pour apprendre comment les exploiter.

    # pacman -S smartmontools

### Nettoyer le cache des paquets
*Cette section fourni une brève vue d'ensemble de comment gérer le répertoire
stockant le cache de tout les paquets téléchargés. Pour plus d'informations,
jetez un coup d'oeil au guide de l'ArchWiki sur [Nettoyer le cache des paquets](https://wiki.archlinux.org/index.php/Pacman#Cleaning_the_package_cache).*

Voici comment utiliser pacman pour nettoyer tous les vieux paquets qui sont en cache:

    # pacman -Sc

Le Wiki avertit que ça devrait être utilisé avec prudence. Par exemple, puisque
les paquets plus anciens sont supprimés du dépôt, si vous rencontrez des problèmes
et voulez revenir à un paquet plus ancien, alors c'est utile d'avoir le cache
disponible. Faites seulement ceci si vous êtes sûr que vous n'en auriez pas besoin.

Le Wiki mentionne aussi cette méthode pour tout enlever du cache, incluant
les paquets à jour installés qui sont dans le cache:

    # pacman -Scc

Ce n'est pas conseillé, puisque ça veut dire qu'il faut retélécharger les paquets  
si vous voulez les réinstaller rapidement. Ça devrait être uniquement utilisé quand
l'espace disque est une priorité.

### Équivalents avec la commande pacman
Si vous venez d'une autre distribution GNU+Linux, vous voulez probablement savoir
les commandes équivalents pour les nombreuses commandes `apt-get` que vous utilisez
souvent. Pour celà, référez-vous à [Pacman/Rosetta](https://wiki.archlinux.org/index.ph
p/Pacman/Rosetta), nommé ainsi car il sert de Pierre de Rosette pour l'ésotérique langage
de pacman.

## your-freedom
`your-freedom` est un paquet spécifique à Parabola et est installé par défaut.
Ce qu'il fait est de se mettre en conflit avec les paquets d'Arch connus comme
des logiciels non libres (propriétaires). Lors de la migration depuis Arch (il
y a un guide sur le wiki Parabola pour migrer/convertir un système Arch existant
en un système Parabola), l'installer échouera aussi, la solution recommandée dès
alors est de supprimer les paquets conflictuels puis de continuer d'installer
`your-freedom`*.

*NdT:votre-liberté

## Ajouter un utilisateur
Ceci est basé sur le guide de l'ArchWiki sur les [Utilisateurs et Groupes](https://w
iki.archlinux.org/index.php/Users_and_Groups).

Il est important (pour des raisons de sécurité) de créer et utiliser un compte non-root
(admin) pour l'utilisation de tout les jours. Le compte **root** par défaut est destiné
seulement à des travaux d'administration critiques, puisqu'il a un accés complet au système
d'exploitation.

Lisez en entier le document partagé en lien ci-dessus, puis continuez.

Ajoutez votre utilisateur avec la commande `useradd`:

    # useradd -m -G wheel -s /bin/bash *votre_nom_dutilisateur*

Définissez un mot de passe, en utilisant `passwd`:

    # passwd *votre_nom_d'utilisateur*

Comme avec l'installation de Parabola, l'utilisation de la [*méthode du lancer de 
dés*](http://world.std.com/~reinhold/diceware.html) est recommandée pour générer des
phrases de passe sécurisée.

### Configurer sudo
Maintenant que nous avons un compte utilisateur normal, nous allons vouloir 
configurer `sudo`, comme ça l'utilisateur peut lancer des commandes en tant
que **root** (p.e., installer des logiciels); ça sera nécessaire de flasher
la ROM plus tard. Référez vous à la documentation de [sudo](https://wiki.ar
chlinux.org/index.php/Sudo) sur l'ArchiWiki.

La première étape est d'installer le paquet `sudo`:

    # pacman -S sudo

Après installation, nous devons le configurer. Pour faire ça, nous devons m
odifier **/etc/sudoers**. Ce fichier doit *toujours* être modifié avec la c
ommande `visudo`. `visudo` peut être difficile à utiliser pour les débutant
s, donc nous allons vouloir éditer le fichier avec `nano`, mais on ne peut
pas faire juste comme ça:

    # nano /etc/sudoers

Car, ça nous causera à éditer le fichier directement, ce qui n'est pas la f
açon conçue pour l'éditeur, et pourrait amener à des problèmes avec le syst
ème.
À la place, pour nous permettre temporairement d'utiliser `nano` pour édite
r le fichier, nous aurons besoin de taper celà dans le terminal:

    # EDITOR=nano visudo

Ça ouvrir le fichier **/etc/sudoers** dans `nano` et nous pouvons maintenan
t y faire des changements sans souci.

Pour donner à l'utilisateur que nous avons créé plus tôt la possibilité d'u
tiliser `sudo`, nous avons besoin de naviguer vers la fin du fichier, et d'
y ajouter cette ligne:

    votre_nom_dutilisateur ALL=(ALL) ALL
Évidemment, tapez le nom de l'utilisateur que vous avez créé à la place de
**votre_nom_dutilisateur**.
Sauvegardez le fichier et quittez `nano`; votre utilisateur a maintenant l
a possibilité d'utiliser `sudo`.

## systemd
`systemd` est le nom du programme pour gérer les services dans Parabola; c
'est une bonne idée de le connaître un peu. Lisez l'article d'ArchWiki sur
[systemd](https://wiki.archlinux.fr/systemd), ainsi que leur article [Usag
e basique de systemctl](https://wiki.archlinux.org/index.php/systemd#Basic
_systemctl_usage), pour bénéficier d'une compréhension complète.
*C'est très important ! Assurez-vous de les lire.*

Un example de service pourrait être un VPN (vous permettant de vous connec
ter à un réseau extérieur, une application dans la barre des tâches indiqu
ant la météo dans votre ville, un gérant de son (pour vous assurez que vou
s pouvons entendre du son à travers des hauts-parleurs ou casque), ou DHCP
 (qui vous permet de récupérer une adresse IP et de se connecter à l'inter
net).
Ce sont juste quelques exemples; ils sont innombrables.

`systemd` est un système d'initialisation controversé; un [billet de foru
m](https://bbs.archlinux.org/viewtopic.php?pid=1149530#p1149530) a une ex
plication pour la décision de son utilisation par l'équipe de développeme
nt d'Arch.

La **manpage** devrait aussi aider:

    # man systemd

La section sur les **types d'unités** (unit types) est très utile.

D'après le wiki, le journal de `systemd` garde des journaux avec un taill
e totale de 10% de la partition racine. Sur une racine de 60Go, ça voudra
it dire 6Go. Ce n'est pas très pratique et peut avoir des impacts sur les
perfomances plus tard quand le journal devient trop gros.
En se basant sur les instructions du wiki, nous réduirons la taille total
e du journal à 50Mo (c'est que le wiki recommande).

Ouvrez **/etc/systemd/journald.conf** et trouvez cette ligne:

    #SystemMaxUse=

Changez la par:

    SystemMaxUse=50M

Relancez `journald`

    # systemctl restart systemd-journald

Le wiki recommande que si le journal devient trop large, vous pouvez auss
i simplement tout supprimer (`rm -Rf`) à l'intérieur de **/var/log/journa
ld**, mais recommande d'en faire une sauvegarde avant.
Ça ne devrait pas être nécessaire puisque qu'on a déjà mis la limite de t
aille juste au-dessus, et `systemd` commencera automatiquement à supprime
r les engeristrements plus vieux, une fois que le journal à atteint la li
mite (d'après les développeurs de systemd).

Finalement, le wiki mentionne les **fichiers temporaires**, et l'utilitai
re pour les gérer.

    # man systemd-tmpfiles

Pour supprimer les fichiers temporaires, vous pouvez utiliser l'option `c
lean`:

    # systemd-tmpfiles --clean

D'après la **manpage**, ceci *"nettoie tous les fichiers et répertoires a
vec un paramètre d'âge"*. D'après l'ArchWiki, ça lit les informations dan
s **/etc/tmpfiles.d** et **/usr/lib/tmpfiles.d**, pour savoir quelles act
ions il faut opérer.

Dès alors, c'est une bonne idée de lire à propos de ce qui est stocké dan
s ces lieux, afin d'avoir une meilleure compréhension.

J'ai regardé dans **/etc/tmpfiles.d/** et trouvé que c'était vide dans mo
n système. Cependant, **/usr/lib/tmpfiles.d** contenait quelques fichiers
Le premier était **etc.conf**, avec des données et une référence à cette
**manpage**:

    # man tmpfiles.d

Lisez cette **manpage** puis ensuite continuez à tout les fichiers.

Les développeurs de `systemd` m'ont dit qu'en pratique ce n'est pas néces
saire du tout de toucher manuellement à `l'utilitaire systemd-tmpfiles`.

## Dépôts intéressants
Dans leur article sur les [kernels](https://wiki.parabolagnulinux.org/Rep
ositories#kernels), le wiki Parabola mentionne un dépôt nommé `\[kernels\]`
pour les kernels customisés qui ne sont pas dans le **base** installé par
défaut. Ça pourrait peut-être valoir le coup de voir ce qui est disponibl
e là-dedans, dépendant de votre cas d'utilisation.

Je l'ai activé sur mon système pour voir ce qui il y avait dedans. Éditez
**/etc/pacman.conf** et en-dessous la section **extra**, ajoutez:

    [kernels]
    Include = /etc/pacman.d/mirrorlist*

Maintenant, synchronisez-vous avec le dépôt fraîchement ajouté:

    # pacman -Syy

Dernièrement, listez tous les paquets dans ce dépôt:
    
    # pacman -Sl kernels

Au final, j'ai décidé de rien installer depuis celui-ci, mais néamoins je
l'ai laissé activé.

## Mettre en place une connection réseau dans Parabola
Lisez le guide d'ArchiWiki sur [Configurer le réseau](https://wiki.archli
nux.org/index.php/Configuring_Network).

### Définir le nom d'hôte
Ça devrait être le même que celui que vous avez défini dans **/etc/hostn
ame** lors de l'installation de Parabola. Vous devriez aussi le faire av
ec `system`. Si vous choisissez le nom *parabola*, faites-le de cette ma
nière:

    # hostnamectl set-hostname parabola

Celà écrit le nom d'hôte spécifié dans **/etc/hostname**.
Plus d'informations peuvent être trouvées dans ces **manp
ages**:

    # man hostname
    # info hostname
    # man hostnamectl

Vérifiez **/etc/hosts**, afin de vous assurez que le nom 
d'hôte inséré pendant l'installation est encore sur chac
une des lignes:

    127.0.0.1 localhost.localdomain localhost parabola
    ::1       localhost.localdomain localhost parabola

Vous noterez que j'ai défini les deux mêmes lignes; la seconde est pour
l'IPv6. Puisque de plus en plus de FAIs fournissent celà en ce moment,
c'est bien de l'activer juste au cas où.

L'utilitaire `hostname` fait parti du paquet `inetutils`, et est dans l
e dépôt **core**, installé par défaut (en tant que partie du paquet **b
ase**).

### Status du réseau
D'après l'ArchWiki, [udev](https://wiki.archlinux.fr/Udev) devrait dét
ecter le chipset Ethernet et automatiquement charger le pilote dès le
démarrage.
Vous pouvez vérifier celà dans la section **Contrôleur Ethernet**, lor
s de l'exécution de la commande `lspci`:

    # lspci -v

Regardez les sections restantes **Pilotes du kernel en cours d'utilis
ation** et **Modules du kernel**.
Dans mon cas, c'était comme il suit:

    Kernel driver in use: e1000e
    Kernel modules: e1000e

Vérifiez que le pilote a été chargé, en exécutant `dmesg | grep nom_m
odule`.
Dans mon cas, j'ai fait:

    # dmesg | grep e1000e

### Noms de périphériques réseaux
D'après le guide d'ArchWiki sur [Configurer les noms de périphériques
réseaux](https://wiki.archlinux.org/index.php/Configuring_Network#Dev
ice_names), il est important de noter que les anciens noms d'interfac
es dont vous avez l'habitude (p.e, `eth0`,`wlan0`,`wwan0`, etc.) si v
ous venez d'une distribution comme Debian ou Trisquel, ne s'appliquen
t plus.
À la place, `systemd` créé des noms de périphériques commençant par
`en` (pour l'Ethernet), `wl` (pour le Wi-Fi), et `ww` (pour le WWAN),
avec un identificateur fixe qu'il génére automatiquement.
Un exemple de nom de périphérique pour votre chipset Ethernet serait
`enp0s25`, et n'est jamais supposé changer.

Si vous voulez rétablir les vieux noms, ArchWiki recommande d'ajouter
`net.ifnames=0` à vos paramètres de kernel ( dans le contexte de Libr
eboot, ça serait accompli en suivant les instructions dans [Comment r
emplacer le fichier de configuration par défaut de GRUB](grub_cbfs)).

Pour des informations historiques, lisez [Noms d'interfaces réseaux p
révisibles](http://www.freedesktop.org/wiki/Software/systemd/Predicta
bleNetworkInterfaceNames/).

Pour voir quels sont les noms de périphériques pour votre système, ex
écutez la commande suivante:

    # ls /sys/class/net

[Changer les noms de périphérique](https://wiki.archlinux.org/index.p
hp/Configuring_Network#Change_device_name) est possible, mais dans l'
intérêt de notre guide, il n'y a aucune raison de le faire.

### Mise en place du réseau
Aside from the steps mentioned above, I choose to ignore most of Networking section on the wiki;
this is because I will be installing the *MATE Desktop Environment*, and thus will
be using the `NetworkManger` client (with its accompanying applet) to manage the network.

If you wish to choose a different program, here are some other
[network manager options](https://wiki.archlinux.org/index.php/List_of_applications/Internet#Network_managers)
that you could use.

## Configuring the Graphical Desktop Environment
Since we are going with the *MATE Desktop Environment*, we will primarily be following
the instructions on the [Arch Linux Package Repository](https://wiki.mate-desktop.org/archlinux_custom_repo) page,
but will also refer to the [General Recommendations](https://wiki.archlinux.org/index.php/General_recommendations#Graphical_user_interface)
on ArchWiki.

### Installing Xorg
The first step is to install [**Xorg**](https://wiki.archlinux.org/index.php/Xorg);
this provides an implementation of the `X Window System`, which is used to provide
a graphical intefrace in GNU+Linux:

    # pacman -S xorg-server

We also need to install the driver for our hardware. Since I am using a Thinkpad X200,
I will use `xf86-video-intel`; it should be the same on the other Thinkpads,
as well as the Macbook 1,1 and 2,1.

    # pacman -S xf86-video-intel

For other systems, you can try:

    # pacman -Ss xf86-video- | less

When this is combined with looking at your `lspci` output, you can determine which
driver is needed. By default, `Xorg` will revert to `xf86-video-vesa`,
which is a generic driver, and doesn't provide true hardware acceleration.

Other drivers (not just video) can be found by looking at the `xorg-drivers` group:

    # pacman -Sg xorg-drivers

### Xorg Keyboard Layout
`xorg` uses a different configuration method for keyboard layouts than Parabola,
so you will notice that the layout you set in **/etc/vconsole.conf** earlier might
not actually be the same in `xorg`.

Check ArchWiki's article on [Xorg's keyboard configuration](https://wiki.archlinux.org/index.php/Keyboard_configuration_in_Xorg), for more information.

To see what layout you currently use, try this on a terminal emulator in `xorg`:

    # setxkbmap -print -verbose 10

I'm simply using the default Qwerty (US) keyboard, so there isn't anything I need
to change here; if you do need to make any changes, ArchWiki recommends two ways
of doing it: manually updating [configuration files](https://wiki.archlinux.org/index.php/Keyboard_configuration_in_Xorg#Using_X_configuration_files) or using the [localectl](https://wiki.archlinux.org/index.php/Keyboard_configuration_in_Xorg#Using_localectl) command.

### Installing MATE
Now we have to install the desktop environment itself. According to the Arch Linux Package Repository,
if we want all of the MATE Desktop, we need to install two packages:

    # pacman -Syy mate mate-extra

The last step is to install a Display Manager; for MATE, we will be using `lightdm`
(it's the recommended Display Manager for the MATE Desktop); for this, we'll folow the instructions [on the MATE wiki](https://wiki.mate-desktop.org/archlinux_custom_repo#display_manager_recommended),
with one small change: the `lightdm-gtk3-greeter` package doesn't exist in Parabola's repositories.
So, instead we will install the `lightdm-gtk-greeter` package; it performs the same function.

We'll also need the `accountsservice` package, which gives us the login window itself: 

    # pacman -Syy lightdm-gtk3-greeter accountsservice

After installing all the required packages, we need to make it so that the MATE Desktop Environment
will start automatically, whenever we boot our computer; to do this, we have to enable the display manager, `lightdm`,
as well as the service that will prompt us with a login window, `accounts-daemon`:

    # systemctl enable lightdm
    # systemctl enable accounts-daemon

Now you have installed the *MATE Desktop Environment*,If you wanted
to install another desktop environment, check out some [other options](https://wiki.archlinux.org/index.php/Desktop_environment) on ArchWiki.

### Configuring Network Manager in MATE
Now that we have installed the Mate Desktop environment, and booted into it,
we need to set up the network configuration in our graphical environment.

The MATE Desktop wiki recommends that we use Network Manager; an
article about Network Manager can be found
[on ArchWiki](https://wiki.archlinux.org/index.php/NetworkManager).

We need to install the NetworkManager package:

    # pacman -S networkmanager

We will also need the Network Manager applet, which will allow us to manage our 
networks from the system tray:

    # pacman -S network-manager-applet

Finally, we need to start the service (if we want to use it now), or enable it,
(so that it will activate automatically, at startup).

    # systemctl enable NetworkManager.service

If you need VPN support, you will also want to install the `networkmanager-openvpn` package.

**NOTE: You do not want multiple networking services running at the same time;
they will conflict, so, if using Network Manager, you want to stop/disable any
others from running. Examples of other services that will probably intefere
with Network Manager are** `dhcpcd` **and** `wifi-menu`**.**

You can see all currently-running services with this command:

    #  systemctl --type=service

And you can stop them using this command:

    # systemctl stop service_name.service

If you want to disable those services, meaning that you no longer want them to start
when the computer boots up, you will need to use `systemctl's` `disable` option,
instead of `stop`.

Now you have a fully-functional graphical environment for your Parabola installation,
including networking. All you have to do is reboot, and you will be prompted to log in,
with a familiar graphical login prompt. You can also now, more easily [modify the GRUB configuration](grub_cbfs.md),
install new applications, and/or make whatever other changes you want to your system.

Copyright © 2014, 2015 Leah Rowe <info@minifree.org>

Copyright © 2017 Elijah Smith <esmith1412@posteo.net>

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License Version 1.3 or any later
version published by the Free Software Foundation
with no Invariant Sections, no Front Cover Texts, and no Back Cover Texts.
A copy of this license is found in [../fdl-1.3.md](../fdl-1.3.md)

