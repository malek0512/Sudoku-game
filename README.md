# Sudoku-game

==Team members==
DONIAS & MAMMAR & REN & SALIM

RAPPORT FINAL DU PROJET DE LOGIQUE

Présentation du problème choisi

Règles :

Une grille de sudoku est composée de 9 lignes, 9 colonnes, et 9 régions (carré de 3 cases par 3 

cases, cf exemple). Au début, la grille est composée de cases contenant un chiffre de 1 à 9 et de 

cases vides. 

Le but du jeu est de compléter les cases vides avec des chiffres de 1 à 9 en respectant la règle 

suivante :

"chaque chiffre doit être présent une et une seule fois par ligne, par colonne et par région."

Une grille initiale de sudoku correctement constituée n’aboutit qu’à une unique solution (un seul 

remplissage possible).

 Exemples :

Si l’on regarde la case bleutée, on ne peut pas y placer le chiffre 1 car il est déjà présent sur 

la même ligne. De même, on ne peut pas placer le chiffre 2 car il est présent dans la même 

colonne. Pour le chiffre 8, on ne peut pas le choisir non plus car il est déjà présent dans la 

même région.

Si l’on procède de la même façon pour tous les chiffres de 1 à 9, on constate que seul le 

chiffre 5 peut être placé dans cette case. On écrira donc « 5 » dans la case et on continuera la

résolution de la grille.

Traduction du sujet en logique de premier ordre

Chaque chiffre doit être présent une et une seule fois par ligne, par colonne et par région.

C'est-à-dire que chaque chiffre doit être présent au moins une fois dans chaque ligne, chaque 

colonne et chaque région ET au plus une fois dans chaque ligne, chaque colonne et chaque région.

On constate que si on traduit cette phrase en logique du 1er ordre, certaines clauses seront 

redondantes.

Ainsi, on peut se ramener à dire que chaque chiffre doit être présent au moins une fois dans chaque 

ligne, chaque colonne et chaque région et qu'il doit y avoir au plus un chiffre dans chaque case (on 

vérifie facilement que c’est équivalent à la phrase précédente).

DONIAS & MAMMAR & REN & SALIM

On choisit que P(i,j,x) signifie que le chiffre x se trouve à la case de coordonnées (i,j).

En logique du premier ordre cela donne :

"Au moins chaque chiffre dans chaque ligne, chaque colonne et chaque région" :

1) ∀i ∈[ 1..9] ,∀ x ∈[ 1..9] ,∃ j, P(i , j , x )

2) ∀ j ∈ [1..9 ] ,∀ x ∈[ 1..9] , ∃ i, P(i , j , x )

3) Soit A=[1..3], B=[4..6], C=[7..9]

 Soit I=A ou B ou C et J= A ou B ou C

∀i ∈ I , ∀ x ∈ J , ∃ x ∈[1..9], P(i , j, x)

"Au plus un chiffre dans chaque case" :

∀ x , ∀ y , x≠ y ⇒ ∀i ,∀ j,¬P(i , j, x ) ∨¬ P(i, j, x)

1 – Etat d'avancement du problème Sudoku : 

Nous avons implanté, comme l'énoncé le demande, les algorithmes DP et WalkSat. Le

problème a été mis sous forme 3-SAT et réduit autant qu'il nous paraissait possible : notre nombre

de variables est passé de ~4000 à ~2000. Malheureusement, nous n'avons, malgré tous nos efforts

acharnés, pu résoudre notre Sudoku. En effet, les exécutions de DP et WalkSat provoquent un

STORAGE ERROR, avec ou sans heuristique de branchement (MOMS). Cependant, nos

programmes s'exécutent correctement avec des ensembles de clauses moins « imposants » que celui

du Sudoku.

2 – Implantation des programmes :

• DP :

 Nous avons donc implanté l'algorithme DP en langage ADA. Nous nous sommes servis de

fonctions secondaires ou auxiliaires pour simplifier cette implantation, par exemple la

procedure suppr_clauses qui prend en paramètre un entier et un ensemble de clauses et qui

supprime les clauses de cet ensemble qui contiennent l'entier en question. Pour représenter

les clauses et l'ensemble de clauses, nous avons choisi la représentation sous forme de

tableaux (de même que pour le calcul des occurrences et pour l'inventaire des affectations).

Cette représentation présente des avantages et des inconvénients. Parmi les avantages, elle

permet d'accéder directement à une clause en connaissant l'indice de celle-ci dans le

tableau : cela facilite, par exemple et surtout, la suppression des clauses. Quant aux

inconvénients, le plus notable, sans doute, est le fait d'être limité par la longueur intialement

définie des tableaux. On perd donc en généralité et une grande quantité de mémoire reste

inutilisée lors de la résolution des petits ensembles. A l'inverse, pour les trop grands

ensembles, mais dont la longueur, pourtant, ne dépasse pas celle intialement définie pour le

tableau de clauses, l'exécution du programme provoque une surcharge en mémoire à cause

d'une multiplication des tableaux de clauses (d'une longueur prédéfinie conséquente) et des

tableaux d'affectations (dont nous parlons plus loin).

Pour parler plus précisément de l'implantation de DP, nous avons aussi créé les types

suivants :

- Affectation : type produit : (entier, naturel) ;

- Affectations : type tableau d'affectation ;

DONIAS & MAMMAR & REN & SALIM

- Satisfaisabilité : type produit : (booléen,Affectations).

Le tableau Affectations répértorie les affectations effectuées au cours de l'exécution de

l'algorithme, i.e l'ensemble des couples (variable, valeur booléenne). Le type Satisfaisabilité

permet d'accumuler les affectations des appels récursifs et sauvegarde la valeur booléenne

une fois celle-ci déterminée.

Heuristiques de branchement :

 Nous avons implanté l'heuristique MOMS. Pour cela, nous avons créé un type 

Occurrences de type tableau.

•Implantation de WalkSat :

Comme pour DP, nous avons utilisé une représentation sous forme de tableaux. Nous avons

implanté une fonction récursive WSAT qui renvoie un modèle, s’il existe, de l’ensemble entré sous

forme 3-SAT. Cette fonction utilise plusieurs sous-fonctions qui simplifient l’écriture et rendent la

lecture plus aisée. Pour le choix aléatoire des affectations, nous nous sommes servis d’une nouvelle

librairie. A l’aide de celle-ci, nous avons instancié deux paquetages permettant de tirer un nombre

aléatoire entre 0 et 1 dans un cas, entre 1, 2 et 3 dans l’autre. Le premier paquetage sert à assigner

arbitrairement la valeur 0 ou 1 à chaque littéral de l’ensemble. Le deuxième paquetage permet de

choisir aléatoirement une variable dans une clause non satisfaite pour ensuite modifier/switcher son

affectation. L’algorithme s’arrête lorsque toutes les clauses sont satisfaites selon les assignations

exhibées.

 Informations complémentaires sur les fichiers :

algo_dimacs.adb : génère les régles du soduku dans le fichier regles.dimacs

dpll.adb : algorithme dpll, prend en argument fichier au format dimacs

dp.adb:algorithme dpll, prend en argument fichier au format dimacs

SAT.adb : algorithme Walksat, prend en argument un fichier au format 3-sat dimacs

conv_3sat.adb : convertie un fichier donné en argument en 3-sat. Le fichier de sorti est nom_3sat

test_sat_paq.adb : permet de générer les règles d'une grille de sudoku selon que la grille est pré-

remplie. Le fichier de sortie est 3sat.dimacs

DONIAS & MAMMAR & REN & SALIM

 Algorithme DP :

Exemples d'execution pertinents :

p cnf 1 1

1 1 1 0

p cnf 1 1

1 1 -1 0

p cnf 3 4

1 2 3 0

-1 -1 -1 0

-2 -2 -2 0

-3 -3 -3 0

Insatisfaisable Satisfaisable. 

p cnf 3 4

1 2 3 0

-2 -2 -2 0

-3 -3 -3 0

-1 2 -3 0

p cnf 3 3

-1 2 3 0

1 -2 3 0

1 2 -3 0

Satisfaisable. 

Modele :

 1 1

Satisfaisable. 

Modele :

 1 0

Modele :

 1 1

 3 0

 2 0

Satisfaisable. 

Modele :

 1 0

 3 0

 2 0

 Algorithme de satisfaisabilité aléatoire :

Exemples d'execution pertinents :

p cnf 6 11

c 

-1 -2 -6 0

1 2 6 0

5 -1 0

-1 -2 0

-1 3 0

4 1 -4 0

1 2 0

-1 -3 -4 0

4 1 0

-4 0

4 1 0

Voici un modele : 

 1 1

 2 0

 6 1

 5 1

 3 1

 4 0

p cnf 3 4

1 2 3 0

-2 -2 -2 0

-3 -3 -3 0

-1 2 -3 0

Voici un modele : 

 1 1

 2 0

 3 0

p cnf 18 14

245 980 0

22 23 0

1 0

2 8 0

5 7 0

3 6 0

-2 -4 0

7 8 0

-3 0

7 9 8 0

3 4 0

112 34 0

45 60 0

19 7 8 0

Voici un modele : 

 245 0

 980 1

 22 1

 23 1

 1 1

 2 0

 8 1

 5 1

 7 1

 3 0

 6 1

 4 1

 9 0

 112 1

 34 1

 45 0

 60 1

 19 0

p cnf 3 3

1 0

-1 0

2 3 0

raised

STORAGE_ERRO

R : stack overflow
