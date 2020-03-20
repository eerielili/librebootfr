---
title: GRUB hardening
...

Ce guide traite des nombreuses façons dont vous pouvez renforcer votre
configuration GRUB pour la sécurité. Ces étapes sont optionnelles, mais
hautement recommandées par le projet Libreboot.

Démarrage sécurisé de GRUB avec GPG
=========================

    Nous utiliserons l'implémentation libre du standard GPG pour le chiffrement et
la signature/vérification des données et ainsi permettant de vérifier la signature d'un kernel Linux
lors du démarrage. 
Plus d'information à propos de GPG peuvent être trouvée sur le [site web du projet GPG](https://www.gnu.org/software/gnupg/).
GRUB a quelques fonctionnalités de GPG nativement, notamment pour la vérification des signatures.

Ce tutoriel présume que vous avez une image Libreboot (rom) que vous souhaitez
modifier, que nous nommerons dès à présent comme "my.rom". Nous allons ici
modifier grubtest.cfg, celà veut dire que la signature et la protection par mot
de passe marchera après basculement sur celui-ci (grubtest.cfg) dans le menu principal
de démarrage, et le bousillage (bricking) dû à une mauvaise configuration sera impossible.
Dès lors que vous êtes satisfait avec le paramétrage, pensez-bien à transférer votre nouvelle
configuration dans votre grub.cfg pour sécuriser votre machine.


Premièrement, extrayez l'ancien grubtest.cfg de my.rom et enlevez le de celle-ci:

    cbfstool my.rom extract -n grubtest.cfg -f my.grubtest.cfg
    cbfstool my.rom remove -n grubtest.cfg

Liens utiles :

-   [manuel GRUB](https://www.gnu.org/software/grub/manual/html_node/Security.html#Security)
-   [pages 'info' GRUB](http://git.savannah.gnu.org/cgit/grub.git/tree/docs/grub.texi)
-   [Espace de stockage connectés en SATA considerés dangereux.](../../faq.md#hddssd-firmware)
-   [Tutoriel sécurité GRUB sur Coreboot](https://www.coreboot.org/GRUB2#Security)

Mot de passe GRUB
=============

La sécurité de ce paramétrage dépend d'un bon mot de passe GRUB car
la vérification de signature GPG peut être désactiver à travers la console
interactive:
    set check_signatures=no

C'est une bonne chose dans le fait où celà permet d'occasionnellement 
démarrer des liveCDs non signés et autres. Vous devriez penser à fournir
les signatures sur une clef USB, mais actuellement le code de
vérification de signature recherche </chemin/vers/fichier>.sig quand il veut
vérifier </chemin/vers/fichier> et alors donc il n'est pas possible de fournir
des signatures dans un endroit différent.

Notez que ça n'est pas votre mot de passe LUKS, mais c'est un mot de passe
que vous avez à rentrer afin d'utiliser des fonctionalités restreintes (tel
que la console). Celà protège votre système d'un attaquant qui démarrerai tout
simplement une clef USB live et reflasherai votre micrologiciel. *Celui ci devrait
être différent de la phrase de passe LUKS et utilisateur*.

L'utilisation de la *méthode du lancer de dés* est recommandée pour générer
des phrases de passe (et non mot de passes) sécurisées. La méthode du lancer
de dés consiste en l'utilisation de ceux-ci pour générer des nombres au hasard qui
seront ensuite utilisés en tant qu'index pour piocher un mot au hasard à partir
d'un large dictionnaire. Vous pouvez utilisez n'importe quel language (p.e Anglais
Allemand, Français, etc).
Vous aurez plus de précisions sur un moteur de recherche (ou 
[ici](https://fr.wikipedia.org/wiki/Diceware). La méthode du lancer de dés est un moyen
de générer des phrases de passes qui sont très dures (presque impossible avec assez de mots)
à cracker tout en étant plus facile à se souvenir.
D'un autre côté, la majorité des types de mots de passes sont plus durs à
retenir et plus faciles à cracker. Les phrases de passe produites grâce 
au lancer de dés sont plus dure à cracker dû à l'entropie bien plus grande (il y a beaucoup
de mots disponibles à l'utilisation à l'opposé de la cinquantaine de symboles
communéments usés dans les *mots* de passe.

-->
Le mot de passe GRUB peut être entré de deux façons:

-   texte pur
-   protégé avec [PBKDF2](https://fr.wikipedia.org/wiki/Pbkdf2)

Nous utiliserons (évidemment) ce dernier. Générer la clé derivée
PBKDF2 se fait grâce à l'utilisation de l'utilitaire `grub-mkpasswd-pbkdf2`.
Vous pouvez l'avoir en installant GRUB version 2.
Générez une clé en lui donnant un mot de passe en tapant:
    grub-mkpasswd-pbkdf2

Ça vous sortira une chaîne de charactère semblable à la suivante:

    grub.pbkdf2.sha512.10000.CHIFFREHEXADECIMAL.PLUSDECHIFFRESHEXA

Maintenant ouvrez my.grubtest.cfg et rentrez le suivant avant les menus
(il vaut mieux que ce soit au dessus des fonctions et autres directives).
Bien sûr, utilisez la chaîne de charactère PBDKF que vous avez précédemment
générée:

    set superusers="root"
    password_pbkdf2 root grub.pbkdf2.sha512.10000.711F186347156BC105CD83A2ED7AF1EB971AA2B1EB2640172F34B0DEFFC97E654AF48E5F0C3B7622502B76458DA494270CC0EA6504411D676E6752FD1651E749.8DD11178EB8D1F633308FD8FCC64D0B243F949B9B99CCEADE2ECA11657A757D22025986B0FA116F1D5191E0A22677674C994EDBFADE62240E9D161688266A711

Encore une fois, remplacez la chaîne de charactère ci-dessus, le "hash", par
celui que vous avez eu pour le mot de passe que vous avez entré dans `grub-mkpasswd-pbkdf2`, et
non celui que vous voyez juste au-dessus !

Comme l'activation de la protection par mot de passe comme ci-dessus veut
dire que vous devez le rentrer à chaque démarrage, nous allons confectionner
un menu qui marche sans y avoir à le faire.
Rappellez-vous que nous aurons la signature GPG active, donc un attaqueur potentiel
ne sera pas capable de démarrer arbitrairement un système d'exploitation. Nous faisons ça
en ajoutant l'option `--unrestricted` dans la définition d'une menuentry:

    menuentry 'Load Operating System (incl. fully encrypted disks)  [o]' --hotkey='o' --unrestricted {
    ...

Une autre bonne chose à faire, si nous choisissons de charger les configurations
GRUB signées sur le disque, et d'enlever (ou commenter) `unset superusers` dans la
fonction try\_user\_config:

    function try_user_config {
       set root="${1}"
       for dir in boot grub grub2 boot/grub boot/grub2; do
          for name in '' autoboot_ libreboot_ coreboot_; do
             if [ -f /"${dir}"/"${name}"grub.cfg ]; then
                #unset superusers
                configfile /"${dir}"/"${name}"grub.cfg
             fi
          done
       done
    }


Pourquoi? Nous avons permis ci-dessus un démarrage normal sans rentrer de
mot de passe. Quand on "unset superusers" et que l'on charge ensuite un fichier
de configuration GRUB signé, on peut aisément utiliser la ligne de commande puisque
la protection par mot de passe sera complètement désactivée.
La désactivation de la vérification de signature et le démarrage de ce que veux
l'attaquant est dès lors à la portée de quelques commandes GRUB.

En tout ce qui concerne la configuration du mot de passe nous sommes OK, donc
nous pouvons passer maintenant à la signature.

Clés GPG
========

En premier lieu, générez une paire de clés GPG utilisés pour la signature. 
L'option RSA (signature seulement) est ok.

Attention: GRUB ne lis pas les clés GPG armurées ASCII. Lors de la tentative
de faire confiance à une clé, l'erreur suivante sortira : error: bad signature.

   mkdir --mode 0700 keys
   gpg --homedir keys --gen-key
   gpg --homedir keys --export-secret-keys --armor > boot.secret.key # sauvegarde
   gpg --homedir keys --export > boot.key

Maintenant que nous avons une clef, nous pouvons signer quelques fichiers avec.
Nous avons à signer :

-   un kernel
-   un initramfs (si on a en un)
-   un grub.cfg sur le disque (si l'on souhaite lui passer le contrôle)
-   grubtest.cfg (de telle façon que quelqu'un puisse revenir sur le 
grubtest.cfg une fois que la vérification de signature est mise en place.
Vous pouvez toujours revenir au grub.cfg en pressant Échap, mais alors grubtest.cfg
n'est pas signé et ne chargera pas.

Supposons que nous avons une pair de `my.kernel` et `my.initramfs` et `libreboot_grub.cfg`
sur le disque. Nous les signons avec les commandes suivantes:

    gpg --homedir keys --detach-sign my.initramfs
    gpg --homedir keys --detach-sign my.kernel
    gpg --homedir keys --detach-sign libreboot_grub.cfg
    gpg --homedir keys --detach-sign my.grubtest.cfg

Bien sûr d'autres modifications de my.grubtest.cfg vont être requises.
Nous avons à faire confiance à la clef et à activer l'enforçage de la 
vérification de signature (mettez ce qui suit avant les menus):

    trust (cbfsdisk)/boot.key
    set check_signatures=enforce

Ce qui reste maintenant est d'inclure les modifications dans l'image
(la ROM):

    cbfstool my.rom add -n boot.key -f boot.key -t raw
    cbfstool my.rom add -n grubtest.cfg -f my.grubtest.cfg -t raw
    cbfstool my.rom add -n grubtest.cfg.sig -f my.grubtest.cfg.sig -t raw

... et de la flasher.


Copyright © 2017 Fedja Beader <fedja@protonmail.ch>\

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
