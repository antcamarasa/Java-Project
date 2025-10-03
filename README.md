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


<details>
  <summary>Voir la solution en code</summary>
  
    public class HelloWorld {
    ... Code to do.
</details> 

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
### 1. Mon implémentation :
 Question auxquelles j'ai été confronté pendant la résolution de mon exercices

#### L'objectif est de réprésenter un jeux de cartes  
1. Structures de données les plus utilisées en Java (équivalents Python)
- Array → équivalent list fixe en Python (taille immuable).
- ArrayList → équivalent list dynamique en Python.
- LinkedList → liste chaînée (ajouts/suppressions rapides).
- HashMap → équivalent dict.
- HashSet → équivalent set.
- Queue / Deque → file (FIFO / LIFO).
- Enum → proche d’un Tuple/Enum Python, utile pour catégories fixes (ex : couleurs, signes).

2. Blueprint pour choisir la structure
- Taille fixe ou variable ?
- Ordre important ou non ?
- Doublons autorisés ?
- Accès rapide par index ou par clé ?
- Besoin de parcours dans les deux sens ?
- Besoin de recherche rapide d’éléments ?

ArrayListe semble être une bonne idée de shuffle et de stocker dans une stack pour ensuité pioché des cartes me parait idéale.

#### Comment représenter un couple A♥ ? 
Il y a trois options principales en Java pour représenter ton couple (valeur, couleur) dans mon ArrayList :
1. Créer une classe Card (la plus propre) → chaque carte est un objet avec deux champs (rank, suit).
- Lisible, extensible, orienté objet.
- C’est la solution classique en Java.

2. Utiliser un Map.Entry ou un record (Java moderne) → sorte de mini-tuple clé/valeur.
- Rapide si tu veux juste stocker deux valeurs groupées.
- Moins expressif qu’une vraie classe.

3. Stocker un String formaté genre "A♥" → simple mais pas flexible.
- Suffisant si c’est uniquement pour affichage.
- Pas pratique pour manipuler les données.

Je choisis la troisième options.

#### Comment tester efficacement que my_list<String> == expected_list<String> ou chaque liste répresente mes 52 cartes

Ma solution naïve :
         
         @Test
        public void testGenerateCardDeck(){
        /*
            1. Créer un tableau fixe de 52 cartes de String
            2. Récupérer le resultat du retour de ma fonction generateCardDeck
            3. Itérer sur touts les élément de mon arrayList et supprimer des que l'lément est trouvé.
            4. si l'arrayList est vide return True else False
         */

        List<String> expectedList = new ArrayList<>(Arrays.asList(
                "♥A", "♥2", "♥3", "♥4", "♥5", "♥6", "♥7", "♥8", "♥9", "♥X", "♥J", "♥Q", "♥K",
                "♦A", "♦2", "♦3", "♦4", "♦5", "♦6", "♦7", "♦8", "♦9", "♦X", "♦J", "♦Q", "♦K",
                "♠A", "♠2", "♠3", "♠4", "♠5", "♠6", "♠7", "♠8", "♠9", "♠X", "♠J", "♠Q", "♠K",
                "♣A", "♣2", "♣3", "♣4", "♣5", "♣6", "♣7", "♣8", "♣9", "♣X", "♣J", "♣Q", "♣K"
        ));

        List<String> outputList = blackJack.generateCardDeck();


        for(int i = 0; i < expectedList.size(); i++){
            for (int j =  outputList.size() - 1; j >= 0; j--){
                if(expectedList.get(i).equals(outputList.get(j))){
                    outputList.remove(j);
                }
            }
        }
        Assertions.assertTrue(outputList.isEmpty());
    }

Point positif : 
- Boucles imbriquées peu lisibles et coûteuses (O(n²)).
- Tu compares à sens unique (expected → output) : pas sûr que expected soit vide aussi.
- Test fragile : trop dépendant de la représentation exacte des Strings.

Options plus adapté :
- Comparer deux Set → transforme tes deux listes en Set et compare l’égalité.
  - Avantage : ignore l’ordre, élimine les doublons, test concis.

- Comparer deux listes triées → trie les deux puis vérifie l’égalité.
  - Avantage : simple et robuste si ordre non garanti.

- Utiliser des assertions de collections (librairies type JUnit/AssertJ/Hamcrest).
 - Avantage : lisible, expressif, directement pensé pour tester des collections.
