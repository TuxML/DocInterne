# Guide des *issues*

## Écrire une bonne *issue*

*TBD*

## Les étiquettes (*labels*)

Afin de permettre d'identifier rapidement et sans difficultés les différents types de problèmes un système simple d'étiquette est implémenté dans *GitHub*.

En règle général ce n'est pas à celui qui a ouvert l'*issue* qui met les étiquettes. Il peut s'agir de quelqu'un désigné "de Triage" ou les développeurs. Ici dans les repertoires du projet **TuxML** nous utilisons les étiquettes suivantes :
- **Etiquettes de niveau** :
  - *critical* : Un bug critique aux effets de bord importants et non limités au programme TuxML en tant que tel (eg. fait planter la BdD lors de l'envoi...). A réservé à des cas exeptionnels.
  - *urgent* : Un bug important empèchant le projet d'avancer et dont la résolution est urgente et/ou importante pour la suite de la réalisation du programme. Des bugs plus "mineurs" peuvent être désignés "urgents" dans des cas particuliers (par exemple si le bug ne déclenche pas de log d'erreur).
  - *moderate* : Un bug affectant le programme et nuisant à son bon fonctionnement.
  - *minor* : Bug ou problème mineur. Peut désigner des problèmes d'affichage ou des comportement inattendus du programme ne résultant pas en un arrêt inattendu.
- **Etiquettes descriptives** :
  - *duplicate* : Indique que l'*issue* désigne un problème déjà référencé dans une autre *issue*. Accompagne généralement une référence vers l'*issue* mère et une cloture.
  - *enhancement* : Etiquette indiquant qu'une *Pull Request* amène de nouvelles fonctionnalités au programme. **Peut être aposée par l'ouvreur de la PR.**
  - *feature request* : Etiquette indiquant qu'une *issue* n'est pas une description de bugs mais une demande de nouvelles fonctionnalités. Une *issue* marquée ainsi peut ne pas suivre le *template* fourni. **Peut être aposée par l'ouvreur de l'*issue*.**
  - *good first issue* : Etiquette indiquant que l'ouvreur a bien respecté pour la première fois le *template* et a fournit une bonne *issue*. utilisé pour permettre aux nouveaux de filtrer les bons exemples et s'en inspirer.
  - *help wanted* : Etiquette drapeau levée par le/les développeur(s) assigné(s) à une tâche pour indiquer qu'ils ont besoin de l'aide du reste du groupe.
  - *need more info* : Etiquette posée pour indiquer qu'une *issue* n'a pas été correctement remplie ou que l'équipe de triage ou de développement à besoin d'informations supplémentaires. Est forcément accompagnée d'une question ou d'un commentaire.
