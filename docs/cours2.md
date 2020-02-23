# Cours 2 de Modélisation et programmation C++

## Outil CMake

CMake permet de générer des Makefiles automatiquement. C'est un outil qui permet de simplifier la création de Makefile. On utilisera un fichier minimal :
```
cmake_minimum_required(VERSION 2.8.11)

project(Demo)

set(CMAKE_CXX_FLAGS "-g -Wall")

# Commentaire
add_executable(exe source1.cxx source2.cxx source3.cxx)
```

## Surcharge d'opérateur.

Un opérateur a le format : `return_type **operator** operator_name(Args)`.
Dans l'exemple on s'intéresse à une redéfinition de `operator=`. Il faut alors réfléchir :
* À la signature : référence ou pas, `const` ou pas.
* Au type de retour.
* À la pertinence des allocations et des copies : allocation alors que déjà alloué ? Copie d'un objet sur lui-même ?

Un opérateur se définit dans une classe (**opérateur interne**) ou en dehors (**opérateur externe**). Un opérateur appelle la méthode surchargée du membre de gauche dans le cas d'un opérateur interne.
On conseille d'utiliser les opérateurs externes lorsque l'opérateur n'induit pas de modification de l'objet.

### Opérateur de conversion

On peut redéfinir l'opérateur de conversion. Il n'a pas de type de retour (constructeur) et suit la syntaxe générale : `operator type_Sortie ()(type_Entree)`. Il est utilisé **IMPLICITEMENT** par le compilateur, donc mieux vaut utiliser un constructeur classique.


### Opérateur de flux

On considère les opérateurs `<<` et `>>`, de sortie et d'entrée respectivement.
* `cin` est l'entrée clavier, de classe `istream`
* `cout` est la sortie écran, de classe `ostream`

Un opérateur de flux renvoie le flux pour le chaînage, on travaille par référence.

### Implémentation des opérateurs

* On préfère avoir une seule implémentation des opérations. Par exemple `+=` et `+` devrait avoir du code en commun. Cela permet de maintenir plus simplement et d'avoir du code robuste.
* Utiliser le sense commun
* Ne surcharger que l'essentiel pour une classe personnelle,
* Tous les cas pour une classe générale
* Éviter de renvoyer des pointeurs pour enchaîner les opérations.

## Différences entre C et C++
### Pointeurs

* `nullptr` remplace `NULL`
* Comme en Rust, il existe des pointeurs intelligents. En particulier `std::unique_ptr` garantit la propriété unique de l'objet pointé et détruit l'objet pointé lorsqu'il est innacessible.
  On peut utiliser `std::move` sur un `std::unique_ptr` pour donner la propriété par exemple.
* Le `shared_ptr` est un pointeur intelligent qui permet d'avoir plusierus possesseurs.

#### Références sur les rvalue

Une référence de type `type&` est une référence sur une lvalue, tandis qu'une référence de type `type&&` est une référence sur une `rvalue`. Comme son nom l'indique, une référence sur rvalue permet de référencer une rvalue et donc d'y accéder plusieurs fois, sans avoir besoin de faire une copie.
Pour obtenir une référence `rvalue` sur un objet, on utilise `std::move`.
Après une utilisation de `std::move`, le contenu de l'objet initial
n'est plus garantit (cela permet de faire des constructeurs par déplacement. Par exemple, on ne peut plus utiliser un `std::unique_ptr après avoir appelé  `std::move` dessus.



### Fonctions inline.

En rajoutant `**inline**` devant la définition de la fonction, l'appel de fonction est remplacé par son contenu. Il faut faire très attention au parenthésage de fait. Cela évite d'utiliser la pile. 

