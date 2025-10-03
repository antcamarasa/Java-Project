# Java-Project

- [Bagels](#bagels)
- [BlackJack](#blackjack)

---

## Bagels

### 1. Version “tout String” (Scanner + String)
- Utiliser `Scanner` pour lire les guesses.  
- Le nombre secret est une `String` (ex: `"248"`).  
- Comparer caractère par caractère avec `charAt`.  
- Vérifier validité de l’input avec `guess.length()` et `guess.matches("\\d+")`.  
👉 **Objectif** : Manipulation basique de `String` et I/O console.

---

### 2. Version `char[]`
- Convertir le nombre secret en tableau de `char` (ex: `char[] secret = ...`).  
- Convertir aussi le guess en `char[]`.  
- Comparer via les indices du tableau.  
👉 **Objectif** : Apprendre la manipulation bas-niveau d’un tableau, indices, boucles.

---

### 3. Version `int[]`
- Générer le nombre secret sous forme d’un tableau d’entiers (`int[]`).  
- Lire le guess, convertir chaque caractère en chiffre (`Character.getNumericValue`).  
- Comparer directement des entiers.  
👉 **Objectif** : Travailler sur des entiers plutôt que des chars, pratiquer parsing et conversion.

---

### 4. Version avec `enum` pour les indices
- Créer un `enum Clue { FERMI, PICO, BAGELS }`.  
- La méthode `getClues` retourne une `List<Clue>` au lieu d’une `String`.  
- Affichage : transformer la liste en texte (`join`).  
👉 **Objectif** : Découvrir et manipuler les `enum` + Collections.

---

### 5. Version OOP simple (classe `Game` + `Engine`)
- `Game` : gère l’interface console, les inputs, le flow.  
- `Engine` : contient la logique (`generateSecret`, `evaluateGuess`).  
- Encapsulation : les variables (secret, maxGuesses, etc.) sont privées, accessibles via méthodes.  
👉 **Objectif** : Structurer le code avec classes, méthodes, séparation des responsabilités.

---

### 6. (Optionnelle) Version `ArrayList` (collections)
- Stocker le secret dans une `List<Integer>` plutôt qu’un tableau.  
- Comparer avec les guesses en utilisant des méthodes comme `contains`, `indexOf`.  
👉 **Objectif** : Découvrir `ArrayList` et manipuler les collections dynamiques.

---

## BlackJack
*(À définir)*
