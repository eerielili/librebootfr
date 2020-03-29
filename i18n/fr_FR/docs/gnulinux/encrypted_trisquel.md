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
La partie la plus dure de l'installation est faite; l'installeur téléchargera et installera 
maintenant les paquets nécessaires pour le démarrage et l'éxecution du système.
Le reste du processus sera en majorité automatisé, mais il y aura quelques choses que
vous allez devoir faire vous même.

### Choisir un kernel
Ça vous demandera quel kernel vous voulez utilisez; choisissez `linux-generic`.

**NOTE: Après installation, si vous voulez obtenir la version la plus à jour
 du kernel Linux (le kernel de Trisquel est parfois obsolète; même dans la
 distribution de test), vous devriez peut-être considérer l'utilisation
 de [ce dépôt](https://jxself.org/linux-libre/). Ces kernels sont aussi
 débloblés, comme ceux de Trisquel (voulant dire qu'il n'y a pas de
 blobs binaires présents.**

### Règles de mises à jour
Vous devez sélectionner une politique par rapport à l'installation des
mises à jour de sécurité; je recommande que vous choisissez `Installer les
mises à jour de sécurité automatiquement`, mais vous pouvez ne pas le choisir
si vous préférez.

### Choisir un environnement de bureau
Quand amené à choisir un environnement de bureau, utilisez les flèches directionnelles
pour naviguer parmi les choix, et pressez `Espace` pour choisir une option; voici quelques
indications:

* Si vous voulez *GNOME*, choisissez **Environnement de bureau Trisquel**
* Si vous voulez *LXDE*, choisissez **Environnement de bureau Trisquel-mini**
* Si vous voulez *KDE*, choisissez **Environnement de bureau Triskel**

Vous allez peut-être vouloir choisir quelques-un des autres groupes de paquets (ou
aucun, si vous voulez une interface en ligne de commande basique); c'est à vous de
choisir. Une fois le choix voulu fait, pressez `Tabulation` puis choisissez `Continuer`.

## Installer le chargeur d'amorçage GRUB sur le MBR (master boot record)
L'installer vous demandera si vous voulez installer GRUB sur le MBR; choisissez `Non`.
Vous n'avez pas du tout besoin d'installer GRUB, puisque dans Libreboot, vous utilisez
la charge utile contenue dans la ROM pour démarrer votre système.

La fenêtre suivante vous demandera d'entrer un `Appareil pour l'installation 
du chargeur d'amorçage`. Laissez la ligne vide; pressez `Tabulation`, et choi-
-sissez `Continuer`.

## Horloge système
L'installeur demande si votre horloge système est définie sur UTC; choi-
-sissez `Oui`.

## Finir l'installation
L'installeur donnera maitenant un message comme quoi l'installation est
complète. Choisissez `Continuer`, enlevez le média d'installation (clef USB)
et le système redémarrera automatiquement.

## Démarrer votre système
À ce point vous aurez fini l'installation. Quand vous
arrivez sur votre charge utile GRUB, pressez C pour avoir
la ligne de commande et entrez :

    grub> cryptomount -a
    grub> set root='lvm/grubcrypt-trisquel'
    grub> linux /vmlinuz root=/dev/mapper/grubcrypt-trisquel \
    >cryptdevice=/dev/mapper/grubcrypt-trisquel:root
    grub> initrd /initrd.img
    grub> boot

Sans spécifier un appareil, le paramètre `-a` de **cryptomount** es-
saye de dévérouiller *tous* les volumes LUKS détectés (p.e., n'importe
quel appareil chiffré LUKS qui est relié au système).
Vous pouvez aussi spécifier le paramètre `-u` (pour un UUID).
Une fois authentifié dans le système d'exploitation, vous pouvez
trouver l'UUID en utilisant la commande `blkid`:

    $ sudo blkid

## ecryptfs
Si vous n'avez pas chiffré votre répertoire personnel, alors vous 
pouvez ignorer sûrement cette section; sinon, juste après que vous
êtes authentifié, vous aurez besoin d'éxecuter cette commande:

    $ sudo ecryptfs-unwrap-passphrase

Ça sera nécessaire dans le futur, si jamais vous avez besoin de
récupérer votre répertoire depuis un autre système. 
Écrivez-la (sortie de la commande), ou (préférablement) stockez-la 
en utilisant un gestionnaire de mots de passe (je recommande `keepass`
`keepasX`, ou `keepassXC`).

## Modifier grub.cfg (CBFS)
La dernière étape du processus est de modifier votre fichier **grub.cfg**
(dans le micrologiciel), et flasher la nouvelle configuration en [utilisant
ce tutoriel](grub_cbfs.md); de ce fait vous n'avez plus à insérer manuellement
les commandes ci-dessus à chaque fois que vous voulez démarrer votre
ordinateur. Vous pouvez aussi rendre votre configuration GRUB bien plus
sécurisée, en suivant [ce guide](grub_hardening.md).

## Dépannage
Pendant le démarrage, quelques ThinkPads ont un lecteur DVD défectueux, ce qui
peut causer l'échec de la commande `cryptomount -a`, ainsi que l'erreur `AHCI transfer
timed out` (quand le ThinkPad X200 est connecté à la station d'accueil UltraBase). Pour les deux problèmes
la solution était d'enlever le lecteur DVD (si utilisation de l'UltraBase, alors la station d'accueil
toute entière doit être enlevé).

Copyright © 2014, 2015 Leah Rowe <info@minifree.org>

Copyright © 2017 Elijah Smith <esmith1412@posteo.net>

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
