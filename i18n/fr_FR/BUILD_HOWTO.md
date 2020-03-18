# Système de build de Libreboot


## Point d'entrée

Le point d'entrée du système de build de Libreboot est le script s'appelant "libreboot" dans le dossier racine .

Éxecuter ce script sans arguments montrera comment l'utiliser de façon générale.


## Configuration

Le système de build peut être configuré avec un fichier s'appelant "libreboot.conf" placé dans le dossier racine.

L'usage typique de ce fichier est l'initialisation des variables d'environnements dont une liste est disponible dans la description de l'usage du point d'entrée.

Puisque le système de build se sert de ce fichier après que les bibliothèques sont chargées, il peut être utilisé pour remplacer n'importe quelle de leurs fonctions ou variables.


## Projets et outils cibles

Le système de build marche autour de concepts de projets et d'outils, qui défini les composants spécifiques qui peuvent être utilisé pour produire :
* des fichiers sources
* des systèmes
* des images
* des outils

De nombreuses actions sont disponibles pour chaque projet et outil cibles, beaucoup d'entre elles figurant dans l'aide du script principal. ? . Ces actions sont exécutés de façon récursives quand aucune cible est spécifiée. 

Chaque projet et outil cible à son propre dossier (soit dans [projets](https://git.miquellionel.ovh/lili/libreboot_fr/src/branch/master/projects) ou [tools](https://git.miquellionel.ovh/lili/libreboot_fr/src/branch/master/tools) ) contenant un script nommé après la cible et si possible un script d'aide ( nom du script+"-helper".
Le script d'aide est automatiquement inclus dans le système de build.
Le nom des fonctions dans le script d'aide sont généralement précédées du nom de la cible, les trait d'union étant rémplacé par "_".

Chaque action d'un projet et outil cible sont les functions définis dans le script spécifique de  la cible, avec des noms correspondant aux actions de la cible.


## Méta-cibles

Les méta-cibles sont des outils et projets cible qui applique l'action demandée sur des cibles individuelles, permettant d'éxecuter une action sur plusiers cibles à la fois. Par example, une méta-cible nommée après le système de build avec un suffixe "-all" appelerait d'autre cibles, prefixé avec "-images", "-tools", etc qui performeront l'action demandée sur toutes les cibles sous-jacentes.


## Actions de projets

De nombres actions génériques permettent la préparation de projets, à travers une série d'étapes :
* télécharger, extraire où mettre à jour la source du projet.
* construire le projet dans un dossier de construction
* installer le projet dans un dossier d'installation
* produire une version du projet dans un dossier de versions
* nettoyer les dossiers d'installation, de construction et de version 

Les actions peuvent être vérifiées par une fonction correspondante spécifique au projet, nommé après la fonction à vérifier avec le suffixe "_check" pour déterminer s'il est nécessaire ou non de les lancer de nouveau pour suivre les étapes.
Une variable d'environnement peut forcer les actions à être éxecutée, en spécifiant une liste de projets espacée :
PROJECTS_FORCE


## Configuration du projet et patchs

La configuration pour chaque projet est stockée dans leur propre dossier.
Les cibles pour chaque projet sont définis par un fichier "targets" dans le dossier "configs". Les cibles sont lues de façon récursive, suivant les noms des sous dossiers pour les cibles du projet. 

Chaque dossier de configuration d'un projet peut être utilisé pour garder des informations et configurations propre la à la cible.

Un fichier "install" dans chaque sous dossier indique quel fichier prendre depuis le fichier de build et où installer dans le dossier d'installation.


## Sources des projets

Chaque projet peut soit télécharger des sources spécifiques ou  soit utiliser les sources d'autres projets, apportant possiblement ses propres patchs et révisions.

Les sources sont téléchargées avec l'action "download" ou peuvent être extraite depuis des sources versionnées ( après avoir placé les sources dans un dossier "source" ) avec l'action "extract".

Les projets peuvent aussi garder leurs sources dans leur propre dossier de projet, dans un dossier nommé "sources".


## Installation des projets

Les projets sont installés depuis des fichiers nommés "install" situés dans le dossier du projet, copiant les fichiers construits sélectionnés dans le dossier d'installation portant le nom du projet, avec la liste des cibles séparées par des trait d'union.
Ces fichiers d'installation sont lus récursivement, suivant les noms des sous-dossiers pour trouver les cibles du projet.

Les fichiers supplémentaires à installer sont spécifiés dans le dossier "install" et décrits dans un fichier du même nom, suivant les noms des sous-dossiers pour trouver les cibles du projet.


## Version des projets

Les projets sont versionnés dans un dossier de version, nommé après le projet avec la liste des cibles séparé par un trait d'union.
Chaque fichiers d'installation d'un projet sont empaquetés dans une tarball reposant dans le dossier de version correspondant.
Une somme de contrôle et une signature GPG détâchée (si la variable d'environnement RELEASE_KEY est initialisée) sont aussi générés.

Les tarballs sont reproduisables grâce à une liste de fichiers donnée par le fichier ".tarfiles". Elle peuvent aussi contenir le fichier ".epoch" et les fichiers git ".revision" et ".version".


## Actions des outils

Les outils sont utilisés pour maintenir le système de build et effectuer des tâches routinières automatisables. Ils peuvent avoir des actions spécifiques et n'implémenter aucune des actions génériques.

Les actions peuvent être vérifiées par un fonction correspondant à un outil spécifique, nommée après la fonction à vérifier et avec le suffixe "_check", pour déterminer si oui ou non il est nécesssaire de les éxecuter à nouveau pour suivre les étapes.
Une variable d'environnement peut forcer certaines actions à être éxecutée en spécifiant une liste espacée de tâches : TASKS_FORCE.


## Source des outils

Les outils peuvent garder leur sources dans un dossier "sources" dans leur propre dossier. Ces sources peuvent être mise à jour avec l'action update.

