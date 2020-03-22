---
title: Information sur l'envoi de patchs et leur revue. 
x-toc-enable: true
...

Téléchargez Libreboot depuis le git comme ceci :

    $ git clone https://notabug.org/libreboot/libreboot.git

Vous pouvez soumettre vos patchs via des
[demandes de fusions](#comment-soumettre-vos-patchs-via-demandes-de-fusions).

Les informations concernant la gouvernance du projet libreboot est documentée dans nos [pratiques générales de management](management.md).

Le développement de Libreboot s'effectue en utilisant le logiciel de contrôle de version Git.
Référez-vous à la [documentation officielle de Git](https://git-scm.com/doc) si vous ne savez pas comment l'utiliser.


Éditer le site web et la documentation, façon wiki
-------------------------------------------------

Le site web et sa documentation est dans le répertoire `www` dans le [répo Git](#comment-télécharger-libreboot-depuis-le-répertoire-git), dans un Markdown à la sauce Pandoc. Le site web est généré en HTML via Pandoc grâce aux scripts suivants, résidant dans le même répertoire :

- index.sh: génère le flux d'actualités (dans la section 'nouvelles du site web').
- publish.sh: convertit un fichier .md en un fichier .html
- Makefile: avec des appels aux scripts index.sh et publish.sh, compile l'entiereté du site web Libreboot.

Utilisez n'importe quel éditeur standart de texte (p.e vim, emacs, nano, gedit) pour modifier les fichiers, répertorier les changements (commit) and [envoyer des patchs](#comment-soumettre-vos-patchs-via-demande-de-fusions)


Optionnellement, vous pouvez installer un serveur web (e.g lighttpd, nginx) localement et configurer la racine du répertoire *www* dans votre répertoire Git.


Avec cette configuration, vous pouvez alors génére une version locale du site web et la voir en rentrant 'localhost' dans la barre d'adresse de votre navigateur.

Vie privée des contributeurs (vous n'avez pas à révéler votre nom ou votre identité!)
------------------------------------------------------------------------------------

Les contributions que vous faites sont publiquement enregistrées 
dans un répertoire Git que tout le monde peut accéder.
Celà inclut le nom et l'adresse mail du contributeur.

Dans Git, vous n'êtes pas obligé d'utiliser votre vrai nom et adresse mail.
Vous pouvez utiliser "Contributeur Libreboot", et votre adresse mail pourrait être contributor@libreboot.org.
Vous êtes autorisé à faire celà si vous souhaitez maintenir votre vie privée. Nous croyons à la vie privée. 
Si vous souhaitez rester anonyme, nous honorerons ceci.

Bien sûr, vous pouvez utilisez n'importe quel nom et/ou adresse qu'il vous plaît.
Pour une confidentialité améliorée, nous recommandons que vous utilisiez un [VPN de confiance](https://torrentfreak.com/which-vpn-services-keep-you-anonymous-in-2019/) et que vous faites circuler tout votre trafic Internet à travers Tor (en plus du VPN).
Le [site Tor](https://torproject.org/) a des renseignements sur la façon de faire passer votre trafic Internet à travers Tor.

Légalement parlant, tout copyright est automatique selon la Convention de Berne sur la loi internationale du copyright. 
Ça n'a pas d'importance pour quel nom, même si vous déclarez un copyright ( mais nous demandons que certains copyright soit utilisés, vous pourrez en lire plus sur cette page ).

Si vous utilisez un email et un nom différent pour vos commits et patchs, alors vous devrez être plutôt anonyme.
Utilisez [git log](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History) et [git show](https://git-scm.com/docs/git-show) pour confirmer ceci avant que vous poussiez vos changements dans un repértoire Git.


Indications générales pour soumettre des patchs
----------------------------------------------

Nous demandons que tout les patches soient soumis sous une license libre:
<https://www.gnu.org/licenses/license-list.html>.

- La GNU General Public License v3 est hautement recommandée.
- Pour la documentation, nous demandons la GNU Free Documentation License version 1.3 ou supérieure.

*Toujours* déclarer une license sur votre travail ! Ne pas déclarer une license veut dire que par défaut les lois de copyright restrictives s'applique, ce qui rend votre travail non-libre.

GNU+Linux est généralement recommandé comme le système d'exploitation de choix pour le développement de Libreboot.

Indications générales pour la revue de code
------------------------------------------

N'importe quel membre du public peut 
[envoyer un patch](#comment-soumettre-vos-patchs-via-demande-de-fusions).

Les membres avec un accès en écriture ne doivent *jamais* écrire sur la branche master; faîtes un demande de fusion, et attendez que quelqu'un d'autre fusionne.
Ne fusionnez jamais votre propre travail !

Votre patch sera revu pour s'assurer de sa qualité, et fusionné si accepté.

Comment télécharger Libreboot depuis le répertoire Git
-----------------------------------------------------

Dans votre terminal :

    $ git clone https://notabug.org/libreboot/libreboot.git

Un nouveau répertoire nommé `libreboot` sera créé, contenant lireboot.


Comment soumettre vos patchs (via demandes de révisions)
-------------------------------------------------------

Faîtes un compte sur <https://notabug.org> et naviguez (pendant que vous êtes connecté) sur
<https://notabug.org/libreboot/libreboot>.
Cliquez *Fork* et dans votre compte, vous aurez votre propre réertoire de Libreboot.
Clonez votre répertoire, faîtes tout les changements que vous voulez et écrivez les dans votre répertoire sur NotABug.

Maintenant naviguez sur <https://notabug.org/libreboot/libreboot/pulls> et cliquez sur *Nouvelle demande de fusion*.
Vous pouvez soumettre vos patchs là-bas.
Alternativement, vous pouvez vous connecter sur le canal IRC de Libreboot et notifier quels patchs vous voulez voir revus si vous avez un répertoire Git avec vos patchs.

Une fois que vous avez envoyé une demande de fusion, les mainteneurs de Libreboot seront notifiés via email. Si vous ne recevez
pas rapidement une réponse du projet, vous pouvez aussi notifier le projet via le canal #libreboot sur Freenode.
