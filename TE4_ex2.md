### **Q3** / 5 (17 pts) - Classe Entier

Déclarer et définir la classe `Entier` dans les fichiers `entier.h` et `entier.cpp`.
La déclaration de la classe `Entier` ne doit pas contenir de définitions en ligne.

Cette classe `Entier` n'est pas générique et doit stocker en **attribut privé** un entier signé codé sur 32 bits et permettre :

* De convertir implicitement tout type numérique vers `Entier` ; cette possibilité de conversion est l'unique élément générique de la classe.
* De convertir explicitement de `Entier` vers `int`.
* D'incrémenter en préfixe ou en postfixe en respectant la convention usuelle pour la valeur retournée.
* D'additionner deux `Entier` avec les opérateurs `+` et `+=`.
* De comparer deux `Entier`.
* D'afficher un `Entier` dans un flux.

Par ailleurs, lorsque la conversion d'un type quelconque à un type `Entier` entraine une perte de précision, elle lève une exception de type `std::invalid_argument`. Par contre, si la conversion est exacte, elle est autorisée.

**Les 5 tests doivent afficher :**

Test 1 :

```text
2 0
0 3 2
1 5 4
2 8 7
3 12 11
4 17 16

```

---

Test 2 :

```text
2

```

---

Test 3 :

```text
2

```

---

Test 4 :

```text
sh: 1: /src/main4: not found

```

## *(Note : Il faut que le test 4 ne compile pas)*

Test 5 :

```text
2
Conversion inexacte

```

### Code à compléter

src/main.cpp

```cpp
#include <iostream>
#include "entier.h"

using namespace std;

int main() {
#if TEST == 1    
   Entier a = 2;
   Entier b;
   cout << a << " " << b << endl;
   for (Entier i{0}; i != 5; ++i) {
      a += i;
      b = a++;
      cout << i << " " << a << " " << b << endl;
   }
#endif
   
#if TEST == 2
   Entier a = 2;
   int b = int(a);
   cout << b << endl;
#endif

#if TEST == 3
   const Entier a = 2;
   int b = int(a);
   cout << b << endl;
#endif

#if TEST == 4
   Entier a = 2;
   int b = a; // cette ligne ne compile pas
#endif 

#if TEST == 5    
   Entier a = 2.0;
   cout << a << endl;
   try {
      Entier b = 3.14;
      cout << b << endl;
   } catch (std::invalid_argument& e) {
      cout << e.what() << endl;
   }
#endif
   
}

```


<details>
<summary>Solution</summary>

entier.h
```cpp
#ifndef ENTIER_H
#define ENTIER_H

#include <cstdint>
#include <ostream>
#include <stdexcept>

class Entier {
   int32_t i;
public:
   Entier();
   template<typename T> Entier(T d);
   explicit operator int() const;
   Entier&  operator+=(Entier const& other);  
   Entier   operator+(Entier other) const;
   Entier   operator++(int);
   Entier&  operator++();
   auto     operator<=>(Entier const& other) const = default;
};

std::ostream& operator<<(std::ostream& os, Entier const& rhs);

/* Variante
class Entier {
   friend bool operator==(Entier const& lhs, Entier const& rhs);
   friend std::ostream& operator<<(std::ostream& os, Entier const& rhs);

public:
   Entier();
   template<typename T> Entier(T d);

   explicit operator int() const;
   Entier&  operator+=(Entier const& other);  
   Entier   operator++(int);
   Entier&  operator++();

private:
   int32_t i;
};

Entier operator+(Entier lhs, Entier const& rhs);
*/

template<typename T> Entier::Entier(T d) : i((int32_t) d) {
      if (d != i)
         throw std::invalid_argument("Conversion inexacte");
}

#endif // ENTIER_H
```

entier.cpp
```cpp
#include "entier.h"
   
//------------------------------------------------------------
Entier::Entier() : i() {}

//------------------------------------------------------------
Entier::operator int() const {
      return i;
}
   
//------------------------------------------------------------
Entier& Entier::operator+=(Entier const& other) {
      i += other.i;
      return *this;
}

//------------------------------------------------------------
Entier& Entier::operator++() {
      ++i;
      return *this;
}

//------------------------------------------------------------
Entier Entier::operator++(int) {
      auto temp = *this;
      ++i;
      return temp;
}

//------------------------------------------------------------
Entier Entier::operator+(Entier other) const {
      return other += *this;
}

//------------------------------------------------------------
std::ostream& operator<<(std::ostream& os, Entier const& rhs) {
      return os << int(rhs);
}

/* Variante
//------------------------------------------------------------
Entier operator+(Entier lhs, Entier const& rhs) {
      return lhs += rhs;
}

//------------------------------------------------------------
bool operator==(Entier const& lhs, Entier const& rhs) {
      return lhs.i == rhs.i;
}

//------------------------------------------------------------
std::ostream& operator<<(std::ostream& os, Entier const& rhs) {
      return os << rhs.i;
}
*/

// |--------------------------------------------------|---------|
// | Critères de correction                           | Points  |
// |--------------------------------------------------|---------|
// | déclaration de la classe                         |         |
// |    public / private                              |   /  1 |
// |    propriété entier en int32t                    |   /  1 |
// |    séparation déclaration / définition           |   /  1 |
// | Entier::Entier()                                 |         |
// |    entête correcte                               |   /  1 |
// | Entier::Entier(T d)                              |         |
// |    template <typename T>                         |   /  1 |
// |    levée d'execption                             |   /  1 |
// | Entier::operator int()                           |         |
// |    explicit                                      |   /  1 |
// |    operator const                                |   /  1 |
// | Entier::operator+=                               |         |
// |    Entier:: => méthode operator                  |   /  1 |
// |    type de retour Entier&                        |   /  1 |
// |    return *this                                  |   /  1 |
// | Entier::operator++()                             |         |
// |    type de retour Entier& valeur de retour       |   /  1 |
// | Entier::operator++(int)                          |         |
// |    type de retour Entier, valeur de retour       |   /  1 |
// | operator+(Entier ...)                            |         |
// |    type de retour Entier                         |   /  1 |
// |    utilisation de +=                             |   /  1 |
// | operator<=>(Entier const& ...) ou ==             |         |
// |    fonction operator== ou default                |   /  1 |
// | ostream& operator<<                              |         |
// |    correct y compris param en const&             |   /  1 |
// |--------------------------------------------------|---------|
// |                                          TOTAL   |  / 17 |
// |--------------------------------------------------|---------|
```

</details>
