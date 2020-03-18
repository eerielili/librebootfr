---
title: Téléchargements 
...

** La dernière version stable est la [20160907](https://notabug.org/libreboot/libreboot/src/r20160907), sortie le 2016-09-07.**

Des informations à propos de changements spécifiques dans chaque version peut être trouvé dans [docs/release.md](doc/releases.md).

Si vous êtes plus interéssé dans le développement de libreboot, allez à la [page du développement libreboot](../git.md), qui contient aussi des liens vers les repos Git.


Clé de signature GPG
---------------

Les versions sont signés avec GPG.

    gpg --recv-keys 0x969A979505E8C5B2

Empreinte numérique complète de la clé : CDC9 CAE3 2CB4 B7FC 84FD  C804 969A 9795 05E8 C5B2e avec

La clé GPG peut être aussi téléchargé avec cet [export de la clé publique](lbkey.asc).

    $ sha512sum -c sha512sum.txt
    $ gpg --verify sha512sum.txt.sig


Avez-vous un mirroir ?
---------------------

Laissez nous savoir ! Nous les ajouterons ici.

Si vous souhaitez créer un nouveau mirroir des versions de Libreboot, vous pouvez utiliser *rsync*. Voir : [liste des mirroirs rsync](#rsync).



mirroirs HTTPS {#https}
-------------

Ces mirroirs sont recommandé car ils utilisent le chiffrement TLS (https://)


<https://www.mirrorservice.org/sites/libreboot.org/release/> (University
of Kent, Angleterre)

<https://mirror.math.princeton.edu/pub/libreboot/> (Princeton
university, États-Unis)

<https://mirror.splentity.com/libreboot/> (Splentity Software, États-Unis)

<https://mirror.sugol.org/libreboot/> (sugol.org)
(formerly nephelai.zanity.net/mirror/libreboot)

<https://elgrande74.net/libreboot/> (elgrande74.net, France)

<https://mirror.koddos.net/libreboot/> (koddos.net, Pays-bas)

<https://mirror.swordarmor.fr/libreboot/> (swordarmor.fr, France)

<https://mirror-hk.koddos.net/libreboot/> (koddos.net, Hong Kong)

<https://mirror.cyberbits.eu/libreboot/> (cyberbits.eu, France)

Mirroirs RSYNC {#rsync}
-------------

Utile pour mirroiter l'ensemble des archives des versions de Libreboot.
Vous pouvez mettre une commande rsync dans crontab et prendre les fichiers dans un dossier de votre serveur web.

*Il est hautement recommandé que vous utilisiez le mirroir de libreboot.org*, si vous souhaitez héberger un mirroir officiel.
Sinon, si vous voulez simplement créer votre propre mirroir local, vous devriez utiliser un des autres mirroir, qui se synchronize de libreboot.org.


<rsync://rsync.libreboot.org/mirrormirror/> (Libreboot project official mirror)

<rsync://rsync.mirrorservice.org/libreboot.org/release/> (University of Kent,
Angleterre)

<rsync://mirror.math.princeton.edu/pub/libreboot/> (Princeton university, États-Unis)

<rsync://ftp.linux.ro/libreboot/> (linux.ro, Roumanie)

<rsync://mirror.koddos.net/libreboot/> (koddos.net, Pays-bas)

<rsync://mirror-hk.koddos.net/libreboot/> (koddos.net, Hong Kong)

Vous faites marcher un mirroir ? Contactez le projet libreboot, et le lien sera ajouté à cette page !


Mirroir HTTP {#http}
------------

ATTENTION : ces mirroirs ne sont pas HTTPS ce qui signifie qu'ils ne sont pas chiffrés. Votre traffic pourrait être sujet à des interférences dû à des adversaires.
Assurez-vous de bien vérifier les signatures GPG, assumant que vous avez la bonne clé.
Bien sûr, vous devriez toujours faire ça même lorsque vous utilisiez HTTPS.

<http://mirrors.mit.edu/libreboot/> (MIT university, États-Unis)

<http://mirror.linux.ro/libreboot/> (linux.ro, Roumanie)

<http://mirror.helium.in-berlin.de/libreboot/> (in-berlin.de, Allemagne)

<http://mirror.cyberbits.eu/libreboot/> (cyberbits.eu, France)


Mirroirs FTP {#ftp}
-----------

ATTENTION : FTP est aussi non chiffré, comme HTTP. Les même risques sont présents.

<ftp://ftp.mirrorservice.org/sites/libreboot.org/release/> (University
of Kent, Angleterre)

<ftp://ftp.linux.ro/libreboot/> (linux.ro, Roumanie)


Reliage statique
------------------

Libreboot inclut des éxecutables liés statiquement (l'éxecutable contient les dépendances et bibliothèques nécessaire à son éxecution , résultant en une taille plus grande mais un éxecutable plus stable et portable). 
Si vous avez besoin des sources pour ces dépendances liées statiquement dans l'éxecutable, alors vous pouvez contactez le projet libreboot via les informations disponibles sur la page d'accueil; le code source sera fourni.
Vous pouvez télécharger ce code source depuis [le dossier "ccsource"](ccsource/) sur libreboot.org.
