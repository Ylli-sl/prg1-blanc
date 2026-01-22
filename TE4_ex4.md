### **Q5** / 5 (12 pts) - Tri par insertion générique

Rendre générique le code ci-dessous qui met en oeuvre un tri par insertion, sans en affecter l'efficacité.

```cpp
void insertion_sort(vector<int> & v) {
   for (size_t i = 1; i < v.size(); ++i) {
      size_t j = i;
      int t = v[i];
      while(j > 0 and t < v[j-1]) {
         v[j] = v[j-1];
         --j; 
      }
      v[j] = t;
   }
}

```

La fonction générique prend en paramètres deux itérateurs bidirectionnels `first` et `last`.

Ces itérateurs disposent donc des fonctionnalités suivantes :

* le déréférencement `*` (en *l-value* et *r-value*)
* le passage à l'élément suivant `++`
* l'accès à l'élément suivant `next(.)`
* le passage à l'élément précédent `--`
* l'accès à l'élément précédent `prev(.)`
* les tests d'égalité (`==` et `!=`).

**Par contre, ils n'implémentent pas :**

* l'accès aléatoire `+`, `-`, `[]`
* les tests d'inégalité entre itérateurs `<`, `>`, `<=`, `>=`

La fonction `imprime(first, last)` est mise à disposition dans un fichier caché pour les tests.

---

### Tests

5 tests vous sont fournis :

1. Trie un `std::vector<int>` de 9 éléments.
2. Trie un `std::vector<int>` de 1 élément.
3. Trie un `std::vector<int>` de 0 élément.
4. Trie un `std::vector<string>` de 5 éléments.
5. Trie une `std::list<T>` (dont les itérateurs respectent strictement la description ci-dessus).

**Résultats attendus :**

Test 1 :

```text
[3, 6, 17, 15, 13, 15, 6, 12, 9]
[3, 6, 6, 9, 12, 13, 15, 15, 17]

```

Test 2 :

```text
[3]
[3]

```

Test 3 :

```text
[]
[]

```

Test 4 :

```text
[bon, courage, pour, cet, examen]
[bon, cet, courage, examen, pour]

```

Test 5 :

```text
[9, 12, 6, 15, 13, 15, 17, 6, 3]
[3, 6, 6, 9, 12, 13, 15, 15, 17]

```

### Code à compléter

src/main.cpp

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <string>
#include <iterator>
#include "insertion_sort.h"

using namespace std;

template<typename Iter>
void imprime(Iter first, Iter last) {
    cout << "[";
    if (first != last) {
        cout << *first;
        for (auto it = next(first); it != last; ++it) {
            cout << ", " << *it;
        }
    }
    cout << "]" << endl;
}

int main() {
    // Note: Décommentez le test que vous souhaitez exécuter ou utilisez -DTEST=X à la compilation

    // Test 1: Vector normal
    // #define TEST 1 
    #if TEST == 1
    vector<int> v {3, 6, 17, 15, 13, 15, 6, 12, 9};
    imprime(v.begin(), v.end());
    insertion_sort(v.begin(), v.end());
    imprime(v.begin(), v.end());
    #endif

    // Test 2: Vector taille 1
    #if TEST == 2
    vector<int> v {3};
    imprime(v.begin(), v.end());
    insertion_sort(v.begin(), v.end());
    imprime(v.begin(), v.end());
    #endif

    // Test 3: Vector vide
    #if TEST == 3
    vector<int> v {};
    imprime(v.begin(), v.end());
    insertion_sort(v.begin(), v.end());
    imprime(v.begin(), v.end());
    #endif

    // Test 4: Strings
    #if TEST == 4
    vector<string> v {"bon", "courage", "pour", "cet", "examen"};
    imprime(v.begin(), v.end());
    insertion_sort(v.begin(), v.end());
    imprime(v.begin(), v.end());
    #endif

    // Test 5: List (Iterateurs bidirectionnels stricts)
    #if TEST == 5
    list<int> l {9, 12, 6, 15, 13, 15, 17, 6, 3};
    imprime(l.begin(), l.end());
    insertion_sort(l.begin(), l.end());
    imprime(l.begin(), l.end());
    #endif

    return 0;
}

```

<details>
<summary>Tests</summary>

test_vector.cpp
```cpp
#include <iostream>
#include <vector>
#include <cstdlib>
using namespace std;
#include "imprime.hpp"

#include "reponse_tri_gen.cpp"

int main() {
  size_t n;
  cin >> n;

  vector<int> v(n);  // Dispose d'itérateurs à accès directs
  for (auto& e : v)
    e = rand() % 20; // Remplissage avec des valeurs aléatoires
          
  imprime(v.begin(), v.end());        // Vecteur avant tri
  insertion_sort(v.begin(), v.end());
  imprime(v.begin(), v.end());        // Vecteur apres tri
} 
```

test_list.cpp
```cpp
#include <iostream>
#include <list>
#include <cstdlib>
using namespace std;
#include "imprime.hpp"

#include "reponse_tri_gen.cpp"

int main() {
  size_t n;
  cin >> n;

  list<int> v;         // Dispose d'itérateurs minimaux seulement
  for (size_t i = 0; i < n; ++i)
    v.push_front(rand() % 20); // Remplissage avec des valeurs aléatoires
           
  imprime(v.begin(), v.end());      // Liste avant tri
  insertion_sort(v.begin(), v.end()); 
  imprime(v.begin(), v.end());      // Liste après tri
}    

```

test_string.cpp
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <cstdlib>
using namespace std;
#include "imprime.hpp"

#include "reponse_tri_gen.cpp"

int main() {
  vector v = { "bon"s, "courage"s, "pour"s, "ce"s, "test"s };    
           
  imprime(v.begin(), v.end());      // avant tri
  insertion_sort(v.begin(), v.end()); 
  imprime(v.begin(), v.end());      // après tri
}    
```



</details>

<details>
<summary>Solution</summary>
  
reponse_tri_gen.cpp
```cpp
/*
template <typename Iterator>
void insertion_sort(Iterator first, Iterator last) {
  if(first == last) return;
  for(Iterator i = next(first); i != last; ++i) {
      Iterator j = i, k = prev(j);
      auto t = *i;
      while(j != first and t < *k) {
         *j = *k;
         --j;
         --k;
      }
      *j = t;
   }
}
*/

template <typename Iterator>
void insertion_sort(Iterator first, Iterator last) {
  if(first == last) return;
  for (Iterator it = std::next(first); it != last; ++it) {      
    auto tmp = *it; 
    Iterator j = it;           
    while (j != first and tmp < *std::prev(j)) {   
      *j = *std::prev(j); 
      --j;
    }
    *j = tmp;
  }
}

// |--------------------------------------------------|---------|
// | Critères de correction                           | Points  |
// |--------------------------------------------------|---------|
// | template<typename Iterator>                      |   /  1 |
// | paramètre iterator   : first et last             |   /  1 |
// | return               : void                      |   /  1 |
// | condition initiale   : first == last             |   /  2 |
// | boucle for                                       |         |
// | - initialisation     : it = std::next(first)     |   /  2 |
// | - condition          : i != last                 |   /  1 |
// | boucle while                                     |         |
// | - variable d'échange       auto tmp              |   /  1 |
// | - iterateur de recherche   Iterator j = it;      |   /  1 |
// | algorithme                 *j = *std::prev(j)    |   /  2 |
// |--------------------------------------------------|---------|
// |                                          TOTAL   |   / 12 |
// |--------------------------------------------------|---------|
```
</details>
