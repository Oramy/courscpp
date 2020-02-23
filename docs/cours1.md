[Cours suivant]({% link cours2.md %})
# Modélisation et programmation C++, cours 1.

Dans ce premier cours, on présente beaucoup de concepts nouveaux. Tous les
concepts présentés ici ont un lien, proche ou lointain avec le premier
cours de *Modélisation et programmation C++* de deuxième année de l'Ensimag.

## Nouveaux concepts

Cette section introduit différents nouveaux concepts. Il faut retenir d'abord :
* La bonne utilisation du mot clé `virtual` (indispensable)
* Utilisation des différents opérateurs de conversion
* Les pointeurs de fonction, de méthodes et de membres de classe (bonus)

#### Héritage virtuel et abstractions.

##### Méthodes virtuelles.

Pour bien comprendre, il faut regarder en détail le test `Examples/virtual.cxx`.

Supposons que la classe `B` hérite de `A` et considérons une méthode `m` de `A` redéfinie dans `B`.
```
A a* = new B();
```
* Si `m` n'est pas virtuelle, `a->m` appelle l'implémentation de `A`, si `m` est virtuelle, `a->m` appelle l'implémentation de `B`.

Pour déclarer une fonction virtuelle, on écrit:
```
virtual _R_ fun(_Args_)
```

En Java, toutes les méthodes sont virtuelles.

##### Fonctions virtuelles pures.
Une fonction virtuelle pure correspond aux méthodes abstraites
en Java. On note :
```
virtual _R_ fun(_Args_) = 0
```

Une classe est abstraite si elle déclare ou hérite
d'une fonction virtuelle pure qui n'est pas implémentée

##### Héritage virtuel
L'héritage virtuel est nécessaire pour gérer le problème du diamant.
Considérons que B, C héritent de A et D hérite de B et C.

```
class C : virtual A
```
### Pointeurs sur des membres de classe
#### Fonctions et méthodes
En C++, les fonctions sont des fonctions dites de première classe : on peut les manipuler comme des variables ([first_class_fun]).

Dans tout le paragraphe, `R` représente le type de retour de la fonction et `Args`
la liste des types des arguments de la fonction concernée.
Si la fonction ne prend pas de paramètre, le type de paramètre est `void`.

Pour rappel, en **C**, on peut manipuler des pointeurs sur des fonctions,
dont la déclaration s'écrit : `R (*fun) (Args)`, et l'assignation et
l'appel de la fonction se font avec `fun`.

En **C++**, les pointeurs sur les fonctions peuvent être déclarés
avec le type `std::function<R(Args)>`.
La définition de `std::function` est déclarée dans l'en-tête `functional`.

Pour déclarer un pointeur vers une fonction membre, on utilise la notation
```
return_type (class::*fun) (type_arg_1, type_arg_2, ...)`
```
ou bien avec `std::function` : `std::function<R(class*, Args)>`.

#### Fixer un certains nombres de paramètres.

La méthode `std::bind` ([bind]) permet de fixer un certains nombres de paramètre.
Par exemple, avec `Foo:doSomething` de type `void (Foo::*)(int, int)`, considérons l'appel:
```
std::function<void(int)> f = std::bind(&Foo::doSomethingArgs, this, 3, _1);
```
L'appel à `f(a)` à alors le même comportement
qu'un appel à `this.doSomethingArgs(3, a)`.

Les constantes pour les placeholders sont définis dans le namespace
`placeholders` ([placeholders]).

#### Pointeur sur un membre de classe.

Plus généralement, on peut obtenir des pointeurs sur des membres de classes. La déclaration d'un tel pointeur
s'écrit :
```
datatype class_name :: *pointer_name;
```
et on note l'affectation :
```
pointeur_name = &class_name::datamember_name;
```
Enfin, pour accéder au dit membre ou l'appeler, on utilise la notation :
```
Object.*pointerToMember
```
On peut déclarer un objet et un pointeur en même temps avec la notation évidente :
```
Data d, *dp;
```
### Casts
    - Pour faire un cast explicite, on peut utiliser les deux notations :
    ```
        float x;
        int i;
        i = int (x); // Notation C++
        i = (int) x; // Notation C
    ```
    - On peut également choisir un type de cast explicite, parmi :
    ```
        const_cast	// Enlève ou rajoute le mot clé const (et enlève
		  	// volatile quand cela enlève const).
            		// Comportement indéfini quand une variable déclarée
			//  constante est modifiée grâce à ce cast.
            		// Erreur à la compilation si un cast de type
			// est également effectué.
        dynamic_cast	// Utilisé seulement pour le polymorphisme, ne
			// fonctionne pas avec le diamant sans héritage virtuel
			// et ne fonctionne pas sur les héritages privés ou protégés.
        reinterpret_cast	// Peut presque tout caster, très dangereux.
			// Par exemple, permet de caster un pointeur en int.
        static_cast 	// Aucune vérification, tout type sauf
			// si il s'agit de classe virtuelles.
    ```

## Snippets
* Snippets/snippet-s24.cxx :
    - attributs et fonctions regroupées par portée :
    `public :`
    - Déréférencement avec &
    - Déclaration pointeur `POINT *pQ;`
    - Accès aux attributs avec `->` comme en C.
    - Nécessité de mettre . pour les `double` ?
    - Utiliser un namespace : `using namespace std`;
    - Inclusion de fichiers de la bibliothèque standard : `#include <*included_files*>`
    - Inclusion de fichiers utilisateurs : `#include "*included_files*"`
    - Compilation : g++ ressemble à gcc.
* Snippets/snippet-s25.cxx :
    - Un tableau d'objets *type* est un pointeur sur *type*.
    - Allocation d'un tableau de taille *n* : `tab = new type[n]`, avec n la taille. 
* Snippets/snippet-s27.cxx :
    - Pour renvoyer une référence, on peut utiliser la syntaxe : `double & val(int i)`
    - On modifie la valeur d'une référence directement : `ref = 3;` et on y accède également
    directement.
* Snippets/snippet-s28.cxx :
    - Déclaration d'une méthode en dehors d'une classe : `classe::methode(signature) { corps }`
    - Le constructeur est le même qu'en Java : `classe(signature)`
    - Création d'un objet :
        - Déclaration : `POINT P(3);`
        - Déclaration et initialisation explicite : `POINT M = POINT(3);`
        - Avec *new* : `POINT *pQ = new POINT(3);`
        - Seuls les objets créés avec new doivent être explicitement libérés.
* Snippets/snippet-s29.cxx
    - Le constructeur par copie de la classe POINT est `POINT(const POINT & P)`
    - On passe une référence constante en paramètre :
        pas de modification des valeurs de l'argument.
    - Entier non-signé : unsigned int;
* Snippets/snippet-s31.cxx
    - Par défaut, les attributs sont privés.
* Snippets/snippet-s32.cxx
    - this : pointeur
    - \*this : objet, donc on peut le renvoyer en référence d'une fonction ayant `classe &`
    pour type de retour.
* Snippets/snippet-s33a.cxx
    - Les attributs privés sont notés ici `x_` et `y_`.
    - On peut accéder et modifier les valeurs privées en faisant un getter qui retourne une référence.
    - Pour les types simple, on peut utiliser struct.
* Snippets/snippet-s34.hxx
    - On utilise des macros pour éviter l'inclusion circulaire, comme en C, on s'en sort avec `#ifndef`, `#define` et `#endif`.
    - Valeur par défaut dans la déclaration de la fonction. Par exemple : `POINT(int d, double v=0)`.
    - On doit mettre un `;` à la fin d'une définition de classe.
* Snippets/snippet-s38a.cxx et Snippets/snippet-s38b.cxx
    - On peut instancier une structure avec `struct complexe z` ou `complexe z`
* Snippets/snippet-s39.cxx
    - Les structures (comme les classes) peuvent avoir des méthodes.
* Snippets/snippet-s40a.cxx et Snippets/snippet-s40b.cxx

## Slides
* Les allocation des attributs sont gérés, par convention, par la classe.
* Par défaut les attributs sont privés.
* Libération de la mémoire avec `delete`, pour les tableaux il faut utiliser `delete []`
* Destructeur noté `~classe()` (jamais d'arguments, pas de retour)
* Utilisation de const :
    - Pointeur sur constant : `const double * p`
    - Pointeur constant sur qqchose : `double * const p`
    - Réf et objet constant : `const double & p`

## Compilation
On compile le programme avec *g++* qui est très similaire à *gcc*.
Options:

*  -Wall : all warnings : les erreurs facilement évitables.
* -pedantic : conforme à la norme ISO.
* -g : produit des informations de debug pour gdb.
* -o : Changer le nom du fichier output
* -c : Compilation sans édition de lien

## Sources
* [Pointeurs de fonctions][function_pointer]
* [Page stack overflow sur les fonctions membres][member_functions]
* [Documentation std::function][function]
* [Documentation std::bind][bind]
* [Documentation std::placeholders][placeholders]
* [Page stack overflow sur les différents casts][cast]
* [Fonction constantes en C++][const_function]
* [Héritage virtuel][virtual]
* [Portée dans l'héritage][access_specifier]
* [Fonction de première classe][first_class_fun]
* [Page stack overflow sur l'utilisation de typeid][typeid]
* [Différence entre struct et class et C++][struct_vs_class]

[function]: https://en.cppreference.com/w/cpp/utility/functional/function
[function_pointer]: https://en.wikipedia.org/wiki/Function_pointer
[member_functions]: https://stackoverflow.com/questions/7582546/using-generic-stdfunction-objects-with-member-functions-in-one-class
[bind]: https://fr.cppreference.com/w/cpp/utility/functional/bind
[placeholders]: https://fr.cppreference.com/w/cpp/utility/functional/placeholders
[cast]: https://stackoverflow.com/questions/332030/when-should-static-cast-dynamic-cast-const-cast-and-reinterpret-cast-be-used
[const_function]: https://www.tutorialspoint.com/const-member-functions-in-cplusplus
[virtual]: https://en.wikipedia.org/wiki/Virtual_inheritance
[access_specifier]: https://www.ibm.com/support/knowledgecenter/SSLTBW_2.3.0/com.ibm.zos.v2r3.cbclx01/cplr130.htm
[first_class_fun]: https://en.wikipedia.org/wiki/First-class_function
[typeid]: https://stackoverflow.com/questions/4465872/why-does-typeid-name-return-weird-characters-using-gcc-and-how-to-make-it-prin
[struct_vs_class]: https://blogs.mentor.com/colinwalls/blog/2014/06/02/struct-vs-class-in-c/

[Cours suivant]({% link cours2.md %})
