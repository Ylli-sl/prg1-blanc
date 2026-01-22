### **Q4** / 5 (6 pts) - Afficher le type

Ecrire dans le fichier `reponse.h` la fonction `afficher_type` et ses éventuelles surcharges / spécialisations pour qu'elle retourne :

* la chaine "int" pour un paramètre de type `int`
* la chaine "unsigned int" pour un paramètre de type `unsigned int`
* la chaine "other" pour tout autre type en paramètre

Les tests doivent donc afficher :

```text
int
other
unsigned int
other

```

Le code est le suivant :

### Code à compléter

src/main.cpp

```cpp
#include <iostream>
#include "reponse.h"

using namespace std;

int main() {
   cout << afficher_type(1) << endl;
   cout << afficher_type(1.) << endl;
   cout << afficher_type(1u) << endl;
   cout << afficher_type(1L) << endl;
}

```

<details>
<summary>Solution</summary>

reponse.h
```cpp
#ifndef AFFICHER_TYPE
#define AFFICHER_TYPE

#include <string>

template<typename T>
std::string afficher_type(T) {
   return "other";
}

template<>
std::string afficher_type(int) {
   return "int";
}

template<>
std::string afficher_type(unsigned) {
   return "unsigned int";
}

/*
template<typename T>
std::string afficher_type(T) {
  if (typeid(T).name() == typeid(int).name()) return std::string("int");
  if (typeid(T).name() == typeid(unsigned).name()) return std::string("unsigned int");
  return std::string("other");
}
*/

#endif // AFFICHER_TYPE

// |--------------------------------------------------|---------|
// | Critères de correction                           | Points  |
// |--------------------------------------------------|---------|
// | fonction générique principe                      |         |
// |    template<typename T>                          |   /  1 |
// |    paramètre afficher_type(T)                    |   /  1 |
// | specialisations                                  |         |
// |    template<>                                    |   /  2 |
// |    int      => afficher_type(int)                |   /  1 |
// |    unsigned => afficher_type(unsigned)           |   /  1 |
// |--------------------------------------------------|---------|
// |                                          TOTAL   |   /  6 |
// |--------------------------------------------------|---------|
```
</details>
