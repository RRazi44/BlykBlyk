---
lang: fr
---

# Extensions

## Extension 1 : Aléatoire ou stratégie en 1 coup : qui est le meilleur ?
Dans cette extension, le but est de faire jouer un joueur **ordinateur** contre un autre joueur **ordinateur** en utilisant différentes "IA" (stratégies) afin de les comparer.

**Tester** une stratégie `S1` contre une stratégie `S2` signifie faire **automatiquement** un **grand nombre** de parties (10000 ou plus) de l'ordinateur utilisant la stratégie `S1` contre l'ordinateur utilisant la stratégie `S2`,
en affichant **uniquement** après chaque partie le **nombre de parties déjà effectuées** et le nombre réel égal au **quotient** du nombre
de **parties gagnées par l'ordinateur utilisant la stratégie S1** divisé par le **nombre de parties déjà effectuées**.

Pour égaliser les chances des 2 joueurs (pour ne pas fausser les résultats attendus sur les stratégies `S1` et `S2`), le leader du premier tour
de la partie sera alternativement le joueur utilisant la stratégie `S1` et celui utilisant la stratégie `S2` au fur et à mesure des parties.

Premier test à réaliser pour tester votre testeur : le test de `S1` contre `S1 `donne-t-il bien un quotient proche de **0.5** (Livrable 2 de base contre lui-même par exemple)?

Dans cette extension, on souhaite pouvoir tester :

* La stratégie du **comportement aléatoire** (livrable 1) contre la stratégie de la version de **base du livrable 2** (meilleur jet, meilleure espérance de classement en 1 coup)

Pour cela, on ajoute un entier `numStrategie` (pour chaque ordinateur) ayant la valeur `0` pour un comportement **aléatoire** et `1`  
pour la version de **base du livrable 2**.

Pour tester votre testeur, vous pouvez commencer par donner la valeur 1 aux 2 joueurs ordinateurs. Si vous n'obtenez pas un
quotient très proche de 0,5, même pour un petit nombre de jetons, vérifiez que le leader du premier tour de la partie change à chaque partie.

Désormais vous pouvez changer en utilisant les deux stratégies différentes (`numStrategie=0`) et (`numStrategie=1`).

Observer comment évolue ce quotient :

* Est-il supérieur (ou inférieur) à 0.5 pour un nombre suffisamment grand de parties déjà effectuées ?
* A-t-il tendance à se stabiliser autour d'une certaine valeur ? Si oui, laquelle ?
* Quel est l'impact du nombre initial de jetons par joueur sur les résultats ?


Le code qui se trouve dans le fichier **SimulateurExtension1.java** est séparé en 3 parties (voir commentaires du code):

* Des fonctions à **adapter légèrement** : Il faut supprimer tout ce qui est relatif à l'interaction avec l'utilisateur (saisie, affichage, pause...),
* Des fonctions à **remanier** : Il faut supprimer tout ce qui est relatif à l'interaction avec l'utilisateur et prendre en compte que les deux joueurs sont des ordinateurs (pas d'humains). Chaque ordinateur possède sa propre stratégie (cf paragraphe précédent).
* Des **nouvelles** fonctions à implémenter.

Ce qu'il faut adapter dans le programme :

* dans la configuration de la partie, saisie de l'entier numStrategie et lancement des 10000 parties en changeant l'ordre des joueurs au
  premier tour à chaque partie,
* la fonction partie prend en compte l'ordre des joueurs pour le premier tour donné en paramètre et retourne un entier permettant de compter le nombre de parties gagnées par le joueur numéro 1,
* adaptation des paramètres des fonctions concernant l'IA,
* adaptation des fonctions concernant le choix des joueurs en cours de partie.

## Extension 2 : Stratégie en 1 coup : quelle est la meilleure valeur à analyser?

Le but de cette extension est de permettre d'optimiser **l'espérance mathématique en 1 coup** de n'importe quelle ""valeur"" donnée
par un tableau `tabValeur` de 56 réels contenant les ""valeurs"" des 56 figures dans le **même ordre que dans le tableau figures**. Autrement dit, le tableau `tabValeur` contient pour tout indice `i de 0 à 55` la valeur donnée à la figure `figures[i]`.

Par **valeur d'une figure** on entend une caractéristique chiffrée permettant d'évaluer l'importance d'une figure et de définir un ordre (pas nécessairement total) sur l'ensemble des figures.

Dans la version de base où l'on recherche la **meilleure espérance de classement**, le tableau `tabValeur` est celui du **classement** des figures
contenant pour tout indice `i de 0 à 55` le **classement** de `figures[i]`, c'est-à-dire **i**.
On peut aussi choisir comme **valeur** d'une figure son **nombre de points**. Dans ce cas le tableau `tabValeur` est le tableau `pointsFigures`.

L'**espérance** de la valeur donnée par le tableau `tabValeur` en **1 coup** de `jet` sur ``resCour``, notée **E1**(`tabValeur`, `jet` sur `resCour`) est définie par :

**E1**(`tabValeur`, `jet` sur `resCour`)

= 1/[6^(nb dés relancés dans `jet`)] * **Somme** pour tout triplet `t` obtenu par `jet` sur `resCour`  **tabValeur**[`indice` dans figures de `figure(t)`]

Remarque : Si `tabValeur` représente le **classement**  alors **E1**(`tabValeur`, `jet` sur `resCour`) = **EC1**( `jet` sur `resCour`)

Exemple :

Si `tabValeur = pointsFigures`, `resCour = [3, 2, 2]` et `jet = [false, true, false]` alors :

**E1**(`pointsFigures`, `[false, true,, false]` sur `[3, 2, 2]`)

= (1/6) * **Somme** pour `x_2` de 1 à 6 **pointsFigures**[indice dans figures  de `figure(3, x_2, 2)`]

= (1/6) * (**Somme** des **points** des figures  `figure(3, 1, 2)`, `figure(3, 2, 2)`, `figure(3, 3, 2)`, `figure(3, 4, 2)`, `figure(3, 5, 2)`, et `figure(3, 6, 2)`)

= (1/6) * (**Somme** des **points** des figures  `[3, 2, 1]`, `[3, 2, 2]`, `[3, 3, 2]`, `[4, 3, 2]`, `[5, 3, 2]` et `[6, 3, 2]`)


Compléter les fonctions  **esperanceEn1Coup** et **genererTableauxJetsEtEsperancesEn1Coup**, qui généralisent les fonctions
correspondantes de la version de base sur **l'espérance de classement** à l'aide du paramètre supplémentaire **tabValeur**.

Dans la version de base, pour déterminer s'il doit relancer les dés ou pas, l'ordinateur leader compare le **classement** de son
résultat courant (qui est en quelque sorte la meilleure espérance en 0 coup du classement sur son résultat courant) et la **meilleure
espérance en 1 coup du classement** sur ce résultat courant.

De même, si la **valeur** d'une figure est donnée par un tableau `tabValeur`,
l'ordinateur leader compare les **meilleures espérances** en **0 coup** et en **1 coup** de la valeur donnée par `tabValeur` sur son résultat courant.

On utilise comme tableaux d'IA, non plus un tableau de 56 entiers et un tableau de 56 réels, mais un tableau à **3 dimensions d'entiers** `jetPourMeilleureEsperance` et un tableau à **3 dimensions de réels** `meilleureEsperance`.

Ces tableaux sont organisés ainsi :

* Indice 0 : contient une matrice 2 * 56 appartenant au joueur 1.
* Indice 1 : contient une matrice 2 * 56 appartenant au joueur 2.

Chaque joueur possède donc une matrice ayant deux lignes et 56 colonnes. Dans cette matrice :

* L'indice de **ligne** est le **nombre de coups** et l'indice de **colonne** est  l'**indice dans figures** du résultat courant du joueur.

Ainsi, le joueur 1 (resp. 2) est représenté par une matrice `meilleureEsperance[0]` (resp. `meilleureEsperance[1]`) et par une matrice `jetPourMeilleureEsperance[0]` (resp. `jetPourMeilleureEsperance[1]`)

* La matrice `meilleureEsperance[0]` (resp. `meilleureEsperance[1]`) contient sur la ligne d'**indice 0** (pour chaque i de 0 à 55) la **meilleure espérance en 0 coup** de la valeur donnée par `tabValeur` sur `figures[i]`, qui est tabValeur[i], pour le joueur 1 (resp. le joueur 2) (valeur du résultat courant figures[i] du joueur courant s'il ne relance pas les dés). La ligne d'**indice 1** contient (pour chaque i de 0 à 55) la **meilleure espérance en 1 coup** de la valeur donnée par `tabValeur` sur `figures[i]`.

* La matrice `jetPourMeilleureEsperance[0]` (resp. `jetPourMeilleureEsperance[1]`) contient une ligne d'**indice 0** qui n'est **pas initialisée** pour la bonne et simple raison qu'il ne peut pas y avoir de jet en 0 coup ! La ligne d'**indice 1** contient (pour chaque i de 0 à 55) un indice (dans le tableau `jetsPossibles`) du jet pour lequel `E1` (espérance **en 1 coup** de la valeur donnée par `tabValeur` sur le résultat `figures[i]` est la meilleure pour le joueur 1 (resp. 2)).

Complétez les fonctions générant ces matrices pour le classement et le nombre de points. La fonction **genererTableauxIA** génère 2 tableaux, l'un pour les meilleurs jets et l'autre pour les meilleures espérances, contenant les matrices d'IA des 2 joueurs.

Comme expliqué précédemment, chaque tableau est de dimension 3 : 2 * 2 * 56 où le premier indice est celui du joueur.

Par défaut, les matrices d'IA du **joueur 1** (valeur 0 du premier indice de chacun des 2 tableaux) sont celles du **classement** en 1 coup et celles du **joueur 2** (valeur 1 du premier indice de chacun des 2 tableaux) sont celles du nombre de **points** en 1 coup.

Tout comme dans l'extension 1, le code qui se trouve dans le fichier **SimulateurExtension2.java** est séparé en 3 parties (voir commentaires du code).

Ce qu'il faut adapter dans le programme :

* dans la configuration de la partie, la seule valeur à saisir est le nombre de jetons, les matrices d'IA des 2 joueurs étant données en paramètres par les 2 tableaux de dimension 3 générés par la fonction genererTableauxIA.
* adaptation des paramètres des fonctions concernant l'IA,
* adaptation des fonctions concernant le choix des joueurs en cours de partie.

Après avoir fait toutes ces adaptations, les choses intéressantes commencent !
Tout d'abord, quelle est la "meilleure" valeur entre classement et points ? Essayer différentes valeurs du nombre de jetons initial.
Avez-vous toujours la même "meilleure" valeur entre classement et points ?

On ne va pas s'arrêter là ! Modifier le programme pour tester des valeurs données par d'autres tableaux `tabValeur`, en modifiant la fonction
`genererTableauxIA`. L'idée est de tester le "meilleur" tableau `tabValeur` déjà trouvé (entre classement et points) contre un nouveau tableau `tabValeur'`, pour déterminer par expérimentation (en lançant un grand nombre de tests) le "meilleur" tableau `tabValeur'` possible. Des points bonus seront attribués si un tableau `tabValeur'` plus performant que `tabValeur` est trouvé et qu'il est original (c-a-d proposé par peu de binômes).

**REMARQUE IMPORTANTE** : ce sont les valeurs par défaut (**classement** pour le **joueur 1** et **points** pour le **joueur 2**) que vous devrez utiliser dans votre rendu de l'extension 2 du livrable 2, pour que cette fonction puisse passer les **tests automatiques**.

Par conséquent, avant le rendu, vous devrez laisser en commentaire les lignes qui permettent de générer et d'utiliser votre "meilleur" tableau `tabValeur` et laisser le comportement initialement demandé (c'est-à-dire classement contre points). Vous ajouterez **une zone commentée au début du fichier** pour signifier que vous avez trouvé un meilleur tableau `tabValeur` ainsi que les instructions pour le tester (quelles lignes décommenter, etc).

**Quelques pistes :**

En remarquant que le classement a l'inconvénient de ne pas tenir compte du nombre de points, et que le nombre de points
a l'inconvénient de donner la même valeur à des figures différentes, on peut choisir un tableau `tabValeur` évitant ces 2 inconvénients, c'est-à-dire strictement croissant et augmentant le "poids" des figures ayant un grand nombre de points. À vous de chercher et bon courage ;) !

## Extension 3 : Jeu avec IA améliorée

Pour cette dernière extension, le but va être de reprendre le meilleur tableau `tabValeur` trouvé lors de l'extension 2 afin de l'utiliser dans le jeu (contre l'humain). C'est donc une évolution de la base du livrable 2 auquel on vient ajouter l'amélioraiton trouvée dans l'extension 2.

Par conséquent, **cette extension ne peut être réalisée que si l'extension 2 a déjà été complétée**.

Pour cette extension, nous allons reprendre la logique des structures de données utilisées lors de l'extension 2 (pour le joueur ordinateur). Cependant, comme il n'y a plus qu'un seul type d'IA (qui utilise votre meilleur `tabValeur`), ces structures sont légèrement simplifiées. Nous n'allons plus utiliser des tableaux à 3 dimensions, mais simplement des **matrices**.

Ainsi, dans le programme, vous manipulerez les **matrices** `meilleureEsperance` et `jetPourMeilleureEsperance`.

* La matrice `meilleureEsperance` contient sur la ligne d'**indice 0** (pour chaque i de 0 à 55) la **meilleure espérance en 0 coup** de la valeur donnée par le `tabValeur` **optimal** sur `figures[i]`, qui est tabValeur[i]. La ligne d'**indice 1** contient (pour chaque i de 0 à 55) la **meilleure espérance en 1 coup** de la valeur donnée par `tabValeur` sur `figures[i]`.

* La matrice `jetPourMeilleureEsperance` contient une ligne d'**indice 0** qui n'est **pas initialisée** (toujours pas de jet en 0 coup !). La ligne d'**indice 1** contient (pour chaque i de 0 à 55) un indice (dans le tableau `jetsPossibles`) du jet pour lequel `E1` (espérance **en 1 coup** de la valeur donnée par le `tabValeur` **optimal** sur le résultat `figures[i]`) est la meilleure.

La génération du `tabValeur` optimal se fera au travers de la fonction `public static double[] genererTabValeursOptimal(?)` que vous devrez décommenter et compléter avec les paramètres adéquats (ce dont a besoin la fonction pour générer votre `tabValeur` optimal).

Comme il n'y a plus qu'un seul type d'IA, on n'a plus besoin de la procédure `genererTableauxIA` qui initialisait les matrices de chaque joueur ordinateur (dans les tableaux à 3 dimensions). Ici, on se servira directement de `genererTableauxJetsEtEsperances` dans la procédure `initialiserEtJouer` en lui donnant en entrée votre `tabValeur` optimal.

Il y a donc quelques fonctions à implémenter et à adapter, mais la logique reste assez similaire à celle de l'extension 2, sauf qu'il y a maintenant un joueur humain et un seul type d'IA.

À la fin, on doit donc obtenir un jeu qui se déroule de manière semblable à celui implémenté dans la base du livrable 2, sauf que l'IA de l'ordinateur est améliorée en optimisant la meilleure "valeur" possible (grâce au `tabValeur` trouvé dans l'extension 2). Selon les tests effectués dans l'extension 2 et vos recherches, ce `tabValeur` peut être soit le classement, soit les points ou bien un meilleur tableau que vous aurez trouvé ! Il faut utiliser celui qui permet d'obtenir le meilleur ratio de victoires.