# TUXML : Compilation du noyau Linux et dépendance

## Automatisation

Le noyau Linux requière un nombre non négligeable de dépendances pour pouvoir être compilé.
Nous cherchons à automatiser le plus possible leur installation. Même en dehors du projet TUXML cela peut-être intéressant de proposer un outil qui automatise cette étape pour faciliter la vie des utilisateurs.

L'installation manuelle des dépendances, lors que l'on veut effectuer des compilations sur différents environnements avec différentes configurations auto-générées serait rédhibitoire.

- Le nombre de dépendances est assez important.Il est difficile de trouver une liste exhaustive. Il est probable que la liste de dépendance évolue dans le temps au fur et à mesure que le noyau évolue.
- Différentes distributions ont des paquets requis au nom différent.
- Certains paquets n'existent que sur certaines distributions.
- Les options ont une influence sur les dépendances requises.

Du point de vue du Machine Learning c'est un problème de classification.

**Objectif** : A partir d'une configuration (environnement de compilation + options du Kernel), prédire automatiquement la liste des dépendances.

## Problèmatiques


La gestion des dépendances soulève plusieurs problèmes :

- On tente de préinstaller un maximum de paquet pour éviter le processus coûteux de résolution automatique. Le problème est alors qu'on risque d'installer des paquets non requis. Par exemple les paquets SSL ne sont requis que si l'on active des options liées à la cryptographie.

- Lorsqu'une dépendance manquante doit être résolue, on itère sur les paquets candidats. On retente de poursuivre la compilation jusqu’à avoir trouvé la bonne dépendance. C'est une méthode assez lente.

- Si on a une liste de N candidats et que le paquet requis est le dernier, on installe les N-1 paquets précédents même si ils ne sont pas requis.
- Lors que l'on a résolu la dépendance après avoir installé le N-ième candidat, il est possible que certains des paquets précédents étaient aussi requis (ou bien faisait partie d'une dépendance du dernier paquet).
Il est évident que le dernier est requis mais il n'est pas trivial de déterminer parmi les précédents ceux dont on a aussi besoin.

## Mode opératoire

Actuellement, pendant la compilation, nous générons un fichier de log qui, pour chaque fichier manquant requis, nous indique les packages installés pour l'obtenir. On indique aussi si on a réussi à résoudre la dépendance ou pas.

Voici un exemple :

Missing files encountered | Missing packages installed |Resolution successfull
------------ | ------------- | -------------
 bversion.h | ['cmake-doc', 'gcc-6-plugin-dev'] | true

Ici on voit que pour le fichier requis *bversion.h*, on a pu résoudre cette dépendance en installant les paquets *cmake-doc* et *gcc-6-plugin-dev*.

Dans cette exemple, *cmake-doc* est installé pour rien et inutile.
Actuellement les fichiers de log sont souvent vides dans notre environnement : dans la
majorité des cas, on a pas de fichier car la liste est assez complète.

Pour prédire quel paquet doivent être installés en fonction des options, une stratégie
possible est de désactiver pré-installation de paquets sauf sur ceux que les paquets sur lequel on sait qu'il est inutile d’investiguer car forcément requis quelque soit les options (gcc, make). On peut ainsi générer un ficher de log complet pour les dépendances.

On peut alors effectuer plein de compilation et obtenir une structure qui fait corrélation entre **configuration<-->options<-->paquets installés**.

Voici la forme simplifiée d'une entrée :

|option1|...| option N | Configuration materielle et logicielle|Missing files encountered | Missing packages installed |Resolution successfull
------------ | ------------- | -------------
|val1|val2| val N| (CPU, disque dur, distribution, version GCC, etc...)  | file1.h | ['pck1', 'pck2'] | true

A partir d'une liste d'entré, on peut utiliser le machine learning pour tenter d'établir des corrélation entre des options et des paquets requis. On peut déterminer que tel ou tel paquet n'est requis que si telle ou telle option est activée. Même sans machine learning il est possible d'extraire de l'information utile : les paquets qui sont installés à chaque compilation sont facilement détectables et peuvent être ajoutés à la liste minimale de dépendances requises.
