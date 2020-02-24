[Cours précédent]({{site.url}}/courscpp/cours2.html)

# Modélisation et programmation C++, cours 3

## Rappels.

### La règle de 3 et de 5
Le destructeur, le constructeur par recopie et l'opérateur de recopie (`operator=`), doivent être implémentées simultanément.
À partir de C++ 2011, on rajoute le constructeur par déplacement de mémoire et la recopie par déplacement de mémoire.

Il faut donc implémenter les 5 méthodes suivantes simultanément :
* `~POINT()`
* `POINT(const POINT &)`
* `operator=(const POINT &)`
* `POINT(POINT &&)`
* `operator=(POINT &&)`

### Qualificateurs `default` et `delete`

À investiguer.

### Autres

Le mot clé `const` protège une variable en écriture.
Il permet également d'écrire des pointeurs constants, pointeurs sur valeurs constantes et référence constante sur objet constant.

Les `float` ne sont pas significativement plus rapides que les `double`.

## Pointeurs et références

Un pointeur permet :
* de manipuler de façon simple des données volumineuses et complexes.
* de créer des tableaux dont chacun des éléments est de taille différente.
* d'ajouter des éléments à un tableau en cours d'utilisation.
* de créer des chaînes.

Une référence **n'est ni une adresse, ni un pointeur**.
Elle permet de manipuler des variables existantes ayant
des portées différentes. Elle doit être initialisée.

Lorsqu'on manipule une référence, elle pointe forcément vers
un objet. Elle est forcément initialisée à sa définition et
ne peut pas être réinitialisée.

Pour les classes, une référence s'initialise dans la liste
d'initialisation.

## Héritage

### Constructeurs et destructeurs

Dans une classe dérivée, on peut appeler explicitement un
constructeur de base dans la liste d'initialisation.

Exemple:
```
POINT_COLOR::POINT_COLOR(int x, int y, int c): POINT(x,y)
{
  couleur = c;
}
```

Si aucun constructeur de base n'est appelé, le constructeur
de base par défaut est appelé avant le début du constructeur.

De même, *après* l'appel du destructeur dérivé, le destructeur
de base est appelé.

### Transtypage

Le **upcast** s'effectue avec la notation C.
Le **downcast** s'effectue à l'aide du transtypage dynamique :
```
pPointColor = dynamic_cast<POINT_COLOR *> (pPoint)
```

### Héritage multiple et classes virtuelles

Considérons le problème du diamant avec A, B, C, D et considérons que chacun possède un attribut `x`. On peut alors spécifier la filiation : A::B::x, A::C::x, B::x ou C::x. Si on ne veut pas de duplication, il faut faire un héritage virtuel :
```
class B: public virtual A{};
class C: public virtual A{};
class D: public B, public C{};
```

Dans l'exemple, les arguments perettant de construire A sont alors ceux de D.
On appelle le constructeur d'une classe virtuelle avant
les autres dans la liste d'initialisation.

Le destructeur virtuel doit toujours être défini même s'il est vide: il n'est jamais surchargé. De plus, une classe virtuelle doit contenir un destructeur virtuel.

L’héritage virtuel est trop cher pour être utilisé en permanence.
On l'utilise lorsque des conditions précises sont réunies : un problème du diamant avec une classe ancêtre commune à toute une bibliothèque.

## Les amis

### Fonction amie d'une classe

On déclare la fonction amie dans la classe, 
avec le mot clé `friend`. L'amie d'une classe
de base n'est pas amie de la classe héritée.

### Fonction d'une classe, amie

La notation est celle qu'on attend. Soit `A` et `B`. Si
`B` possède une fonction amie de `A`, il faut définir `B`
avant `A` et déclarer `A` avant `B`.

Toute fonction (membre ou non) peut être amie de plusieurs
classes.

### Toutes fonctions membres amies d'une autre classe

On peut déclarer la classe amie : `friend class B`.
Pour compiler `A`, il faut déclarer `B` avant.

Conclusion : **Utilisez une fonction membre quand vous le pouvez, 
utilisez une fonction amie quand vous le devez.**

## Les exceptions

Stratégies possibles :
* Renvoyer un statut : demande de la vigilance.
* Expression conditionnelle : gestion au cas par cas.
* Assertions : en mode debug
* Exceptions : flot de contrôle non local.

Les exceptions permettent de découpler la détection de
l'anomalie de son traitement.

- Détection : `throw`
- Traitement : `catch`

Une exception non traitée stoppe le programme.

Il faut dériver la classe exception, dont la définition est:

```
class exception
{
	public:
	exception() throw();
	exception(const exception& rhs) throw();
	exception& operator=(const exception& rhs) throw();
	virtual ~exception() throw();
	virtual const char *what() const throw();
};
```

Dans une bibliothèque, **il faut** implémenter de nouvelles
exceptions.

On peut limiter les exceptions qu'une méthode peut lever avec

```
f(int a, int b) throw(Error)
```


[Cours précédent]({{site.url}}/courscpp/cours2.html)
