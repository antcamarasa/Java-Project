# Java-Project

- [Bagels](#bagels)
- 
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

### Comment représenter un couple A♥ ? 
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

Ici, on choisit la première option, utilisation d'un set (hashSet en java). 

Rappel rapide des Set :
- Pas de doublons : chaque élément est unique.
- Pas d’ordre garanti
- Accès rapide (en général O(1) avec HashSet).
- Permet les opérations ensemblistes : union, intersection, différence via méthodes (addAll, retainAll, removeAll).
- Basé sur equals() et hashCode() pour vérifier l’unicité des éléments (un cours complet sur le sujet a faire)

Comme un set, supprime les doublons pour être sur que mon teste est fonctionnel, je vais vérifier la taille de outputList avant de le transformer en set(). 
Si taille égale ainsi que les deux set sont identiques alors on a bien un jeu de 52 cartes complet.


#### Implémentation de getHand() fonction qui permet de tiré des cartes ?
Afin de pouvoir simuler une pile de carte quoi de mieux que d'utiliser une stack ? Rien. Du coup on utilise une stack. 
On va donc transformer dans notre retour de fonction generateCartDeck un cast vers une Dequeu(interface qui implémente les stacks en java) voici comment :

    Deque<String> deck = new ArrayDeque<>(blackJack.generateCardDeck());

Ensuite notre méthode getHand prendra en paramètre une stack et donc on n'a plus qui simuler la pioche avec un stack.pop().


#### ArrayList vs List | Deque vs ArrayDequeue

1. Différence entre List<String> hand = new ArrayList<>(); et ArrayList<String> hand = new ArrayList<>();
- List est une interface → un contrat qui dit “cette structure se comporte comme une liste” (méthodes : add, get, size…).
- ArrayList est une implémentation concrète → la version qui utilise un tableau dynamique derrière.

👉 Quand tu écris :

- ArrayList<String> hand = new ArrayList<>(); → tu dis “ma variable est exactement une ArrayList”.
- List<String> hand = new ArrayList<>(); → tu dis “ma variable est une Liste (n’importe laquelle), et là je choisis ArrayList comme implémentation”.

⚡ Bonne pratique en Java = coder au plus proche de l’interface (List, Deque, Set…), pas pour une implémentation précise. Ça rend ton code plus flexible.

2. Idem pour Deque et ArrayDeque.

Question intéressante, en argument d'une fonction on doit passer l'interface ou l'implémentation ?

👉 Toujours l’interface (Deque<String>) dans la signature.
- Ça rend ta fonction indépendante de l’implémentation (ArrayDeque, LinkedList, …).
- Tu peux changer l’implémentation plus tard sans casser ton code.
