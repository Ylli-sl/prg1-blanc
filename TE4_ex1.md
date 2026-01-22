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

<details>
<summary>Fichiers tests</summary>
   
logs_valid.txt
```
15/03/2024;ERROR;Connexion échouée
07/11/2023;INFO;Démarrage du service
01/01/2025;WARNING;Espace disque faible
22/06/2024;INFO;Utilisateur connecté
31/12/2023;ERROR;Timeout base de données
```

logs_mixed.txt
```
15/03/2024;ERROR;Connexion échouée
ligne sans separateur
07/11/2023;INFO;Démarrage du service
;WARNING;Date manquante
01/01/2025;WARNING;Espace disque faible
22/06/2024;INFO
31/12/2023;ERROR;Timeout base de données
seulement;deux;
03/05/2024;DEBUG;Niveau inconnu mais valide
```

logs_empty.txt
```
```

logs_extra.txt
```
15/03/2024;ERROR;Connexion échouée;données ignorées;encore plus
07/11/2023;INFO;Démarrage du service;extra
01/01/2025;WARNING;Espace disque faible;192.168.1.1;admin
```

logs_edge.txt
```
01/01/2000;INFO;A
;;message sans date ni niveau
15/03/2024;;Message sans niveau
;INFO;Message sans date
15/03/2024;ERROR;
15/03/2024;ERROR;Message normal
```

</details>


<details>
<summary>Solution</summary>
analyser_logs.h
   
```cpp
#ifndef ANALYSER_LOGS_H
#define ANALYSER_LOGS_H

#include <string>
#include <vector>

struct Evenement {
   std::string date;
   std::string niveau;
   std::string message;
};

std::vector<Evenement> analyser_logs(const std::string& nom_fichier);

#endif

// 3 pts 
// - # guards + #include
// - const string& ou string_view
// - std::vector<Evenement>
```

analyser_logs.cpp
```cpp
#include <fstream>
#include <sstream>
#include <iostream>
#include "analyser_logs.h"
using namespace std;

vector<Evenement> analyser_logs(const string& nom_fichier) {
   ifstream fichier(nom_fichier);
   
   if (not fichier)
      cerr << "fichier illisible";
   
   vector<Evenement> v;
   string ligne;
   
   while (getline(fichier, ligne)) {
      Evenement e;
      stringstream ss(ligne);
      
      if (not getline(ss, e.date, ';'))    continue;
      if (not getline(ss, e.niveau, ';'))  continue;
      if (not getline(ss, e.message, ';')) continue;
      
      /* plus "risqué" mais ça marche aussi...
      getline(ss, e.date, ';');
      getline(ss, e.niveau, ';');
      getline(ss, e.message, ';');
      */
      if (e.date.empty() || e.niveau.empty() || e.message.empty()) continue;
      
      v.push_back(e);
   }
   
   return v;
}

// 9 pts
// - ouverture fichier
// - test ouverture réussie
// - lecture 1 ligne
// - lecture date
// - lecture niveau
// - lecture message
// - gestion lignes non valides
// - remplissage vecteur
// - retour tableau
```

</details>
