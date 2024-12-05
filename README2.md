---
lang: fr
---

# IA basée sur les probabilités

Pour ce second livrable, vous allez travailler sur une IA basée sur les probabilités afin qu'un joueur type ordinateur
puisse faire des coups "intelligents".

## Probabilités

Le cœur de l'IA que vous allez développer se base sur **l'espérance mathématique** d'une certaine "valeur". Celle-ci 
est calculée sur la figure qu'obtiendra le joueur courant après avoir relancé les dés à partir de son résultat courant, 
en fonction des dés qu'il va relancer. Le joueur ordinateur choisit de relancer les dés qui lui donnent la meilleure espérance de cette "valeur" possible.
Dans la version de base, la "valeur" choisie pour évaluer une figure est son **classement**, c'est-à-dire son indice dans le tableau **figures** contenant toutes les figures en ordre croissant d'importance.

On distingue l'espérance de classement en 1 coup et en 2 coups, puisque le joueur peut rejouer 1 ou 2 coups.
Dans la version de base, vous vous limiterez à **l'espérance du classement en 1 coup**, c'est-à-dire que l'ordinateur essaie d'optimiser
son prochain coup, sans tenir compte du fait qu'il s'agit ou non de son dernier coup.

### Calcul de l'espérance du classement en 1 coup

Le classement d'une figure étant son indice dans le tableau figures, **l'espérance mathématique du classement** de la figure obtenue 
**en 1 coup**, notée `EC1`, par le lancer de dés **jet** sur le résultat courant **resCour** du joueur courant est définie par :<br>

**EC1** (`jet` sur `resCour`)

 = 

**Somme** pour chaque figure `fig` [**classement**(`fig`) * **probabilité**(obtenir `fig` par `jet` sur `resCour`)]
  
 = 
 
**Somme** pour chaque figure `fig` [**indice** de `fig `dans figures * **probabilité**(obtenir `fig` par `jet` sur `resCour`)]

Exemple :


Si `resCour = [6, 4, 1]`  et ` jet = [true, false, true]` alors : 


**EC1** (`jet` sur `resCour`)

 = 

**Somme** pour chaque figure `fig` [**indice** de `fig` dans figures * **probabilité**(obtenir `fig` en lançant `d1` et `d3` sur `[6, 4, 1]`)]


La probabilité d'obtenir la figure `fig` en lançant `d1` et `d3` sur `[6, 4, 1]` dépend de `fig`. Par exemple :  

* pour `fig = [6, 1, 1]`, cette probabilité est nulle puisque la valeur `4` de `d2` doit être conservée,
* pour `fig = [4, 1, 1]`, cette probabilité est égale à `1/(6*6) = 1/36` puisqu'elle ne peut être obtenue que par le triplet `(1, 4, 1)`, c'est-à-dire en obtenant `1` pour `d1` et `1` pour `d3`
*  pour `fig = [4, 2, 1]`, cette probabilité est égale à 2/36 puisqu'elle peut être obtenue par les triplets `(1, 4, 2)` et `(2, 4, 1)`,
c'est-à-dire en obtenant `1` pour `d1` et `2` pour `d3` ou en obtenant `2` pour `d1` et 1 pour `d3`.

Le calcul de l'espérance de classement à partir des figures est donc un peu compliqué du fait de la non-équiprobabilité des figures.
Nous allons donc faire ce calcul à partir des triplets (avant réordonnancement en figures) en ne considérant que les triplets
correspondants au lancer **jet** sur **resCour**.

Dans notre exemple, les triplets correspondant au lancer de `d1` et `d3` sur `[6, 4, 1]` sont les triplets de la forme `(x_1, 4, x_3)`, avec `x_1` de `1 à 6` et `x_3` de `1 à 6`, qui ont donc tous la même probabilité `1/(6*6) = 1/36`.

On reprécise que même si deux triplets différents peuvent donner la même figure une fois réordonnés,
on considérera bien ces triplets de manière indépendante dans nos calculs, comme dans l'exemple
donné plus haut : si on a le résultat courant `[6, 4, 1]` et qu'on effectue le jet `[true, false, true]`, 
on peut par exemple obtenir les triplets `(1, 4, 2)` ou bien `(2, 4, 1)`. On compte donc `1/36` pour chaque triplet 
malgré le fait qu'une fois réordonnés ces triplets donnent la figure `[4,2,1]`.

Les triplets sont donc différents des figures. Par contre, comme on se sert du **classement de la figure** correspondant 
au triplet dans nos calculs, il faudra réordonner le triplet pour obtenir le bon classement.

Par exemple, si on veut savoir le classement "donné" par l'obtention du triplet `(2,4,1)`, on réordonnera en figure : `[4,2,1]`.

Pour tout triplet `(x_1, x_2, x_3)` d'entiers de **1 à 6**, on appelle **figure(x_1, x_2, x_3)** la figure obtenue à partir de `(x_1, x_2, x_3)` en le réordonnant en ordre décroissant.

Bref, revenons sur notre exemple :

`EC1([true, false, true]` sur `[6, 4, 1])`

=  
 
**Somme** pour `x_1` de `1 à 6` et `x_3` de `1 à 6` [(**indice** dans **figures** de la figure correspondant à `figure(x_1, 4, x_3)`) * **probabilité** (obtenir `(x_1, 4, x_3)` en lançant `d1` et `d3` sur `[6, 4, 1]`) ]
   
   
= 
 
 **Somme** pour `x_1` de `1 à 6` et `x_3` de `1 à 6` [(**indice** dans **figures** de la figure correspondant à `figure(x_1, 4, x_3)`) * `1/36`]

= 

`(1/36)` * **Somme** pour `x_1` de `1 à 6` et `x_3` de `1 à 6` [**indice** dans **figures** de la figure correspondant à `figure(x_1, 4, x_3)`]

= 

`(1/36)` * **Somme** des **indices** dans **figures** des figures `figure(1,4,1)`, `figure(1,4,2)`, ..., `figure(6,4,5)` et `figure(6,4,6)`

= 

`(1/36)` * **Somme** des indices dans **figures** des figures `[4,1,1]`, `[4,2,1]`, ..., `[6,5,4]` et `[6,6,4]`.

**Un second exemple :**

`EC1([false, true, false]` sur `[4, 3, 3])` = `(1/6)` * **Somme** pour `x_2` de `1 à 6` [**indice** dans **figures** de la figure correspondant à `figure(4, x_2, 3)`]

**Un troisième exemple :**

`EC1([true, true, true]` sur `[2, 2, 1])` = `(1/216)` * **Somme** pour `x_1` de `1 à 6`, `x_2` de `1 à 6` et `x_3` de `1 à 6` [**indice** dans **figures** de la figure correspondant à `figure(x_1, x_2, x_3)`] (on peut obtenir toutes les figures en relançant tous les dés).

De manière générale, pour calculer l'espérance, il faut identifier les triplets qu'il est possible d'atteindre en effectuant un
jet sur un résultat. Pour cela, on doit identifier les **bornes** (valeurs minimale et maximale) des valeurs de dés 
que l'on pourra obtenir en effectuant le jet.
**Si un dé n'est pas relancé** (dans le jet), alors les 2 bornes sont égales à la valeur de ce dé : [4,4].
**Si un dé est relancé** (dans le jet), ses bornes varient entre 1 et 6 :

* Dans l'exemple `EC1([true, false, true]` sur `[6, 4, 1])`, les bornes sont `[[1,6], [4,4], [1,6]]`. On sait donc que tous les triplets accessibles par le jet sont : pour `x_1` de `1 à 6` et `x_3` de `1 à 6` les triplets `(x_1, 4, x_3)`.
* Dans l'exemple `EC1([false, true, false]` sur `[4, 3, 3])`, les bornes sont `[[4], [1,6], [3]]`. On sait donc que tous les triplets accessibles par le jet sont : pour `x_2` de `1 à 6` les triplets `(4, x_2, 3)`.
* Dans l'exemple `EC1([true, true, true]` sur `[2, 2, 1])`, les bornes sont `[[1,6], [1,6], [1,6]]`. On sait donc que tous les triplets accessibles par le jet sont : pour `x_1` de `1 à 6`, `x2` de `1 à 6` et `x_3` de `1 à 6` les triplets `(x_1, x_2, x_3)`.

Pour déterminer les probabilités d'obtenir un triplet à partir d'un jet, cela dépend essentiellement du nombre de dés à relancer.
À vous de déterminer ces probabilités.

Enfin, comme tous les triplets que l'on peut obtenir à partir d'un jet ont donc la même probabilité, on peut simplement multiplier cette probabilité par la somme des classements des figures correspondant aux triplets (une fois réordonnés) qu'il est possible d'obtenir en effectuant le jet depuis le résultat courant.

Si le joueur ordinateur a comme résultat courant **resCour** et doit (ou décide de s'il est le leader) rejouer au moins 1 coup, 
il choisit un lancer de dés **jet** tel que **EC1** de **jet** sur **resCour** est la **plus grande** possible. 
On appelle cette plus grande espérance **la meilleure espérance de classement en 1 coup** sur **resCour**.

Concernant le **leader** ordinateur, le fait qu'il doit rejouer ou non sera décidé en comparant le **classement**
de son **résultat courant** et la **meilleure espérance de classement en 1 coup sur ce résultat courant**. Si cette espérance
est plus grande que le classement de son résultat courant, alors il décide de rejouer.

### Structures de données du programme

Afin d'optimiser le programme, tous les calculs (meilleurs jets et espérances de classement) sont pré-calculés une seule fois au
lancement du programme et les résultats sont stockés dans différentes **structures de données**.

Chaque figure est représentée par son indice dans le tableau **figures**, et chaque jet est représenté par son indice dans le tableau
 **jetsPossibles** de tous les jets possibles que vous commencerez par générer.

Vous générerez ensuite un tableau de **56 entiers** et un tableau de **56 réels** :

Le tableau d'entiers `jetPourMeilleureEsperanceClassementEn1Coup` et le tableau de réels `meilleureEsperanceClassementEn1Coup` contenant respectivement dans chaque case d'index `i` (compris entre 0 et 55), **l'indice** (dans le tableau `jetsPossibles`) du jet pour lequel `EC1` sur le résultat courant `figures[i]` est la meilleure, et **cette meilleure espérance**.

**Par exemple :**

Pour chercher le meilleur jet à effectuer pour obtenir la meilleure espérance à partir du résultat `figures[2]` (**[3,3,1]**), on regarde dans le tableau `jetPourMeilleureEsperanceClassementEn1Coup[2]` ce qui donne la valeur `5`. Alors le meilleur jet à effectuer pour obtenir la meilleure espérance à partir de ce résultat est `jetPossibles[5]`, c'est-à-dire `[true, true, false]`. Et la meilleure espérance en question est `meilleureEsperanceClassementEn1Coup[2]`, c'est-à-dire `29.94` (arrondi).
   
Le tableau d'entiers permet au joueur ordinateur de choisir les dés à relancer.

Le tableau de réels permet à l'ordinateur leader de décider s'il doit rejouer ou non en comparant le classement 
de son résultat courant et la meilleure espérance de classement en 1 coup sur ce résultat courant.

## Second livrable

Le second livrable consiste donc à :

* programmer les fonctions qui permettent de générer les tableaux définis ci-dessus,
* adapter les fonctions liées à l'IA de l'ordinateur pour utiliser ces données
* coder les extensions (présentation de celles-ci d'ici peu)