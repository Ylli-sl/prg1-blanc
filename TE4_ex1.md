### **Q2** / 4 (12 pts) - Gestion d'un fichier de logs

Déclarez et définissez la fonction `analyser_logs` dans les fichiers `analyser_logs.h` et `analyser_logs.cpp` de sorte qu'elle reçoive le nom du fichier à lire en paramètre et retourne un tableau de `Evenement` déclaré ainsi :

```cpp
struct Evenement {
   string date;
   string niveau;
   string message;
};

```

Les fichiers sont au format texte avec les champs séparés par des `;` :

```text
15/03/2024;ERROR;Connexion échouée
07/11/2023;INFO;Démarrage du service

```

**Règles de gestion :**

* Si le fichier n'est pas lisible, affichez `fichier illisible` à la **sortie d'erreur standard** (`std::cerr`) et retournez un tableau vide.
* Si une ligne ne respecte pas le format attendu, n'incluez pas les données de cette ligne dans le tableau retourné, mais continuez la lecture du fichier.
* Une ligne respecte le format dès lors que les 3 informations sont lisibles et séparées par des `;`.
* Il n'y a pas besoin de valider le format de la date ou du niveau, mais de s'assurer que ces derniers ne sont pas vides.
* Si la ligne contient d'autres informations à la suite, elles sont simplement ignorées.

### Code à compléter

src/main.cpp

```cpp
#include <iostream>
#include <fstream>
#include <vector>
#include "analyser_logs.h"

using namespace std;

int main() {
    // Création d'un fichier de test
    ofstream testFile("logs.txt");
    testFile << "15/03/2024;ERROR;Connexion échouée" << endl;
    testFile << "INVALID_LINE_WITHOUT_SEMICOLONS" << endl;
    testFile << "07/11/2023;INFO;Démarrage du service" << endl;
    testFile << ";WARNING;Date manquante" << endl; // Doit être ignoré
    testFile << "01/01/2024;DEBUG;Trop de champs;Ignorer ceci" << endl;
    testFile.close();

    // Test de la fonction
    vector<Evenement> events = analyser_logs("logs.txt");

    cout << "--- Evénements lus (" << events.size() << ") ---" << endl;
    for (const auto& e : events) {
        cout << "[" << e.date << "] " << e.niveau << " : " << e.message << endl;
    }

    cout << "\n--- Test fichier inexistant ---" << endl;
    analyser_logs("inconnu.txt");

    return 0;
}

```
