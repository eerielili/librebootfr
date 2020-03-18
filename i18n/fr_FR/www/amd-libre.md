[Mise à jour](https://yro.slashdot.org/story/17/07/19/1459244/amd-has-no-plans-to-release-psp-code): malheuresement AMD n'a rien de prévu pour sortir le code source, et au lieu de ça fais confiance à des "hackers" tierce-partie pour auditer la "fonctionnalité"; trop de pressions de la part d'agences gouvernementales ? Qui sait ?

---
title: Nous appelons AMD à sortir le code source et spécifications pour la plateforme Ryzen
...

Récemment dans le projet Libreboot, nous avons été informés à propos des nouveaux microprocesseurs Ryzen publiés et vendus par AMD.
Ils sont en ce moment de prendre des avis de la communauté.
Voici des moyens de contacter AMD pour leur signaler que vous demandez du matériel libre :
-   <https://www.reddit.com/r/Amd/comments/5x4hxu/we_are_amd_creators_of_athlon_radeon_and_other/def5h1b/>
-   <https://twitter.com/amd?lang=fr>
-   <https://www.facebook.com/AMD/>
-   <https://community.amd.com/places?filterID=all%7Eobjecttype%7Espace>
-   <https://www.amd.com/fr/corporate/contact> (a des liens de contacts pour de multiples pays)


-   **La PDG d'AMD, Lisa Su, peut-être contactée directement par email. Dites-lui que vous demandez du 
    matériel libre <lisa.su@amd.com>**

Libreboot a le but de fournir un micrologiciel complétement *libre* sur les processeurs Intel, AMD, ARM, POWER et RISC-V, et le fait déjà sur de plus vieux processeurs.

Comme documenté dans la [section FAQ de Libreboot](../faq.md#amd), AMD est en ce moment non coopératif
dans le mouvement du logiciel libre. Spécifiquement, ils publient des micrologiciels non libre exécutables seulement, en plus de technologies tyrannique comme 
l'[AMD Platform Security Processor](../faq.md#amdpsp).

Nous , projet Libreboot , appelons AMD à publier son code source et commencer la coopération avec notre fournisseur en amont [coreboot](https://coreboot.org/) ( et [librecore](http://librecore.info)) pour ses nouveaux processeurs Ryzen et la microarchitecte Zen. Cela inclut le code source pour tout les micrologiciels d'initialisation ( référé typiquement comme le micrologiciel BIOS ou UEFI par quelques membres de la communauté ), et en particulier, l'*AMD Platform Security Processor*, pour permettre à la communauté du logiciel libre d'utiliser du matériel AMD étant entièrement respectueux des libertés. 
Si ce n'est pas trop demander, nous aimerions aussi le code source et les clés de signement du PSP et du microcode du CPU.

Nous aimerions aussi avoir les guides de design des cartes, documents techniques et 
[empreintes](https://fr.wikipedia.org/wiki/Empreinte_d%27un_composant_%C3%A9lectronique) pour les CPUs/southbriges et ainsi de suite.

Nous avons spécialement besoin que les clés de signement soient publiées pour les composants étant signés (PSP, microcode du CPU, SMU, etc). 
Cela rendra possible d'utiliser n'importe quel code source publié ( actuellement quelques composants ne marcheront pas sauf si le micrologiciel est signé par une certaine signature, habituellement tenu secrète par les fabriquants de matériel).

Nous, dans la communauté, avons besoin de matériaux respectant les libertés ! Nous appelons AMD à travailler avec nous, les projets Libreboot, Coreboot et Librecore
pour un monde où l'informatique n'est plus sous le verrou du fabriquant, mais au main des utilisateurs.

Cela a plusieurs avantages pour AMD. Il y a en ce moment une énorme demande sur le marché pour le matériel libre.
Aujourd'hui, les seules entreprises le fournissant sont celles comme les fournisseurs Libreboot où les systèmes sont vendus avec du logiciel entièrement libre, comprenant le micrologiciel de démarage et le système d'exploitation, sans aucun micrologiciel signé pour qui aucune clés sont disponibles au public.

Le problème ? Ces entreprises (les fournisseurs de systèmes Libreboot) vendent des systèmes bien plus vieux étant rendu libre en grande partie grâce à l'ingénérie inversée.
Aujourd'hui les systèmes vendus par ces entreprises utilisent des designs matériel vieux de plus de 5-10ans, ce qui veut dire que la majorité des gens voulant utiliser tout logiciel libre ne peut pas le faire, dû à des problèmes pratiques.
Il y a quelque personnes qui utiliseront ces systèmes plus vieux, mais sans un énorme sacrifice au niveau de la commodité puisqu'ils finissent par utiliser du matériel vieux et obsolète, et certaines tâches ( spécialement le développement logiciel sérieux/professionnel ) devient impraticable pour beaucoup de personnes. 

AMD à le pouvoir de renverser cette tendance, et il a du potentiel pour faire un grand bénéfice. Les communautés du libre et open source sauteraient pieds par dessus tête pour encourager
un tel geste. En quelque sorte, AMD peut se faire de l'argent en investissant dans le libre.

Il y a même un précedent. AMD avait par le passé publié le code source pour ses nouveaux processeurs et architectes au projet Coreboot, mais ensuite ils ont arrêté.
Nous appelons à la reprise, et d'aller encore plus loin.

Voici quelques exemples de campagnes populaires, quelques-une d'entre-elles ayant réussi :

-   <https://www.crowdsupply.com/sutajio-kosagi/novena>
-   <https://www.crowdsupply.com/eoma68/micro-desktop>
-   <https://www.crowdsupply.com/raptor-computing-systems/talos-secure-workstation>

Dans tout ces cas, les campagnes étaient populaires et ceci malgré le matériel étant soit
 bas de gamme et non adapté à la majorité des personnes, ou trop cher à obtenir.

Puis regardez la popularité du projet Libreboot.

Imaginez juste ce qu'il arriverait si AMD commençait à produire du matériel peu cher et procurable, au point où Libreboot pourrait commencer à supporter de nouveaux systèmes venant d'AMD.
Les possibilités sont infinies ! Les gens sauteraient sur AMD et les ventes d'AMD exploseraient, pendant que nous dans la communauté du matériel libre, pourrait finalement avoir des systèmes d'un constructeur qui prend soin de nos libertés d'utiliser nos ordinateurs sans logiciel propriétaire.

Même le matériel bas de gamme comme le BeagleBone ou le Raspberry Pi ([qui peut-être libéré](https://blog.rosenzweig.io/blobless-linux-on-the-pi.md)) montre que la technologie libre
est profitable, et désiré par la communauté.

Regardez les Google Chromebooks. Ces appareils sortent avec coreboot préinstallé par défaut ! Il y a même quelque Chromebooks ARM que nous supportions dans Libreboot, qui sont encore produits et vendus flambant neuf par des revendeurs (p.e Amazon, Newegg, etc). Ces appareils sont vendus par millions ! Cela montre que non seulement c'est possible, mais aussi profitable pour AMD de commencer à publier des systèmes qui respectent les libertés des utilisateurs.

Ce n'est pas juste les bénéfices commerciaux qui sont rendus possible. Il y a toutes sortes de possibilités pour
la recherche scientifique si les systèmes sont libres au niveau matériel/micrologiciel. Par exemple aujourd'hui, les 
universités n'apprennent pas le développement de micrologiciel BIOS / de démarrage dans les cours d'informatiques parce que les technologies sont en ce moment restreinte par les fabriquants et disponible seulement à quelques privilégiés.

AMD a le pouvoir de faire la bonne chose. Nous, dans Libreboot, appelons AMD à travailler avec nous pour construire un monde où les utilisateurs de la technologie peut utiliser leurs ordinateurs sans dépendre d'aucun logiciel propriétaire. Nous voulons - avons besoin - d'un monde de matériel hautement sécurisé, libre, aux mains des utilisateurs, par des entreprises qui prennent soin du logiciel libre.

Le projet Libreboot est contactable, en usant les détails sur la page d'accueil. 
Nous avons hâte de travailler avec AMD :). 
