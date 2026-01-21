## Classe Duree - 18 pts
Écrire une classe `Duree` permettant de représenter et manipuler des durées exprimées en heures et minutes (par exemple `2h45` ou `0h30`) de sorte que le code fourni compile et s'execute correctement.

La classe doit contenir:

**Attributs privés**

* `heures` : nombre d'heures (int)
* `minutes` : nombre de minutes (int, **toujours entre 0 et 59**)
**Exemple** : `Duree(2, 63)` doit créer une durée de 3h03 (et non 2h63).

**Les durées peuvent être considérées comme toujours possitives** (inutile de traiter le cas de durée négative).

**A l'utilisation, le code complet affiche la sortie suivante**
```
Test 1 : Duree d
0h00
----------------------------------------
Test 2 : Duree p(2, 63)
3h03
----------------------------------------
Test 3 : q = 130
2h10
----------------------------------------
Test 4 : r = p + q
5h13
----------------------------------------
Test 5 : s(1, 30) puis s += Duree(0, 45)
Avant : 1h30
Après : 2h15
----------------------------------------
Test 6 : Duree t(1, 60)
2h00

```

duree.cpp
```cpp
#include "duree.h"
#include <iostream>

using namespace std;

// Votre réponse ici
```

duree.h
`à compléter`

main.cpp
```cpp
#include <iostream>
#include <string>
#include <cstdlib>
#include "duree.h"
using namespace std;

int main() {
    cout << "Test 1 : Duree d" << endl;
    Duree d;
    cout << d << endl;
    cout << "----------------------------------------" << endl;
    
    cout << "Test 2 : Duree p(2, 63)" << endl;
    Duree p(2, 63);
    cout << p << endl;
    cout << "----------------------------------------" << endl;
    
    cout << "Test 3 : q = 130" << endl;
    Duree q = 130;
    cout << q << endl;
    cout << "----------------------------------------" << endl;
    
    cout << "Test 4 : r = p + q" << endl;
    Duree r = p + q;
    cout << r << endl;
    cout << "----------------------------------------" << endl;
    
    cout << "Test 5 : s(1, 30) puis s += Duree(0, 45)" << endl;
    Duree s(1, 30);
    cout << "Avant : " << s << endl;
    s += Duree(0, 45);
    cout << "Après : " << s << endl;
    cout << "----------------------------------------" << endl;
    
    cout << "Test 6 : Duree t(1, 60)" << endl;
    Duree t(1, 60);
    cout << t << endl;
    cout << "----------------------------------------" << endl;
    
    
    return EXIT_SUCCESS;
}
```

<details>
<summary>Solution</summary>

duree.cpp
```cpp
#include "duree.h"
#include <iostream>

using namespace std;

// Constructeur par défaut
Duree::Duree() : heures(0), minutes(0) {}

// Constructeur avec heures et minutes
Duree::Duree(int h, int m) : heures(h), minutes(m) {
    if (minutes >= 60) {
        heures += minutes / 60;
        minutes = minutes % 60;
    }
}

// Constructeur de conversion depuis int
Duree::Duree(int m) : heures(m / 60), minutes(m % 60) {}


// Opérateur <<
ostream& operator<<(ostream& os, const Duree& d) {
    os << d.heures << "h" << (d.minutes < 10 ? "0" : "") << d.minutes;
    return os;
}

// Opérateur +=
Duree& Duree::operator+=(const Duree& autre) {
    *this = Duree(heures + autre.heures, minutes + autre.minutes);
    return *this;
}

// Opérateur +
Duree operator+(Duree lhs, const Duree& rhs) {
    return lhs += rhs;
}
```

duree.h
```cpp
#ifndef DUREE_H
#define DUREE_H

#include <iostream>

class Duree {
    int heures;
    int minutes;

    friend std::ostream& operator<<(std::ostream& os, const Duree& d);
    friend Duree operator+(Duree lhs, const Duree& rhs);
    
public:
    Duree();
    Duree(int h, int m);
    Duree(int m);
    
    Duree& operator+=(const Duree& autre);
};

#endif
```

```
// | Critères de correction                           | Points |
// |--------------------------------------------------|--------|
// | Structure de base                                |        |
// |    attributs privés  : heures et minutes (int)   |  / 2  |
// |    déclarations      : public/privé correct      |  / 1  |
// |    constructeur      : par défaut (0h00)         |  / 1  |
// | Constructeur Duree(int h, int m)                 |        |
// |    fonctionnel       : initialise correctement   |  / 2  |
// |    normalisation     : minutes > 59 → heures     |  / 1  |
// | Conversion int → Duree                           |        |
// |    constructeur      : implicite (non explicit)  |  / 2  |
// | Opérateur <<                                     |        |
// |    déclaration       : friend non-membre         |  / 1  |
// |    format            : Xh00 avec padding         |  / 2  |
// | Opérateur +=                                     |        |
// |    type              : membre (non friend)       |  / 1  |
// |    retour            : Duree&                    |  / 1  |
// |    normalisation     : après addition            |  / 1  |
// | Opérateur +                                      |        |
// |    type              : non-membre                |  / 1  |
// |    implémentation    : utilise += ou équivalent  |  / 1  |
// |    résultat          : correct (5h13)            |  / 1  |
// |--------------------------------------------------|--------|
// |                                          TOTAL   |/ 18  |
```
</details>
