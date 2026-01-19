### **Q3** / 5 (10 pts) - Encodeur / Decodeur

Écrivez deux fonctions en C++, `encodeFile` et `decodeFile`.

* **Fonction `encodeFile` :**
* **Entrée :**
* `inputFilename` : le nom du fichier texte source à chiffrer.
* `outputFilename` : le nom du fichier de sortie où le texte encodé sera enregistré.
* `shift` : un entier représentant le décalage utilisé pour l'encodage.


* **Traitement :**
Chaque lettre alphabétique est décalée du nombre donné de positions dans l'alphabet. Le décalage est cyclique, les majuscules et les minuscules doivent être décalées dans leurs casses respectives :
```text
encode 'a', décalage 2 -> 'c'
encode 'a', décalage -1 -> 'z'
encode 'Z', décalage 3 -> 'C'

```


Pour les caractères non alphabétiques, aucun décalage n'est effectué.
* **Sortie :**
Le fichier `outputFilename` doit contenir le texte encodé.


* **Fonction `decodeFile` :**
* **Entrée :** Les mêmes que pour `encodeFile`.
* **Traitement :** Cette fonction doit inverser le processus d'encodage (décoder), en prenant le nom du fichier encodé et la même valeur de décalage pour retourner le contenu original décodé. **Pensez à éviter au maximum le code en double.**
* **Sortie :** Le fichier `outputFilename` doit contenir le texte décodé.


* **Gestion des erreurs :**
Votre code doit pouvoir gérer les cas où les fichiers ne peuvent pas être ouverts ou créés.
* **Note :** Supposons que le décalage peut être un entier positif ou négatif.

**Exemples :**

```cpp
// source.txt = "Hello, World! 123
//               How are you ?"

// chiffre.txt = "Khoor, Zruog! 123
//                Krz duh brx ?"

// clair.txt = "Hello, World! 123
//              How are you ?"

encodeFile("source.txt", "chiffre.txt", 3);
decodeFile("chiffre.txt", "clair.txt", 3);

```

### Code à compléter

<details>
<summary>Fichiers fournis (main.cpp & source.txt)</summary>

src/main.cpp

```cpp
#include <cstdlib>
#include <iostream>
#include <string>
#include <cctype>
#include <fstream>

// Inclusion des fichiers à compléter (simulation de compilation simple)
#include "encodeFile.cpp"
#include "decodeFile.cpp"

int main() {
    // Création du fichier source pour le test si nécessaire, 
    // ou assurez-vous qu'il existe dans le dossier src/
    
    encodeFile("src/source.txt",  "src/chiffre.txt", 3);
    decodeFile("src/chiffre.txt", "src/claire.txt",  3);

    std::cout << "--- Contenu du fichier chiffré ---" << std::endl;
    system("cat src/chiffre.txt");
    
    std::cout << "\n--- Contenu du fichier décodé ---" << std::endl;
    system("cat src/claire.txt");

    return EXIT_SUCCESS;
}

```

src/source.txt

```text
Hello, World! 123
How are you ?

```

</details>

<details>
<summary>Solution</summary>

src/encodeFile.cpp

```cpp
void encodeFile(const std::string& inputFilename, const std::string& outputFilename, int shift) {
    std::ifstream inputFile(inputFilename);
    if (!inputFile) {
        std::cerr << "Erreur lors de l'ouverture du fichier." << std::endl;
        return;
    }

    std::ofstream outputFile(outputFilename);
    if (!outputFile) {
        std::cerr << "Erreur lors de la création du fichier de sortie." << std::endl;
        return;
    }

    // Normalisation du décalage pour rester dans [-25, 25]
    // (Optionnel mais propre)
    shift %= 26; 

    std::string line;
    while (std::getline(inputFile, line)) {
        for (char &c : line) {
            if (std::isalpha(c)) {
                char base = std::islower(c) ? 'a' : 'A';
                // La formule +26 permet de gérer correctement les décalages négatifs
                c = ((c - base + shift + 26) % 26) + base;
            }
        }
        outputFile << line << std::endl;
    } 

    inputFile.close();
    outputFile.close();
}

```

src/decodeFile.cpp

```cpp
void decodeFile(const std::string& inputFilename, const std::string& outputFilename, int shift) {
    // Réutilisation intelligente de encodeFile avec un décalage inversé
    encodeFile(inputFilename, outputFilename, -shift);
}

```

</details>
