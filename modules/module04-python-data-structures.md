# Module 4: Python - Partie 2: Structures de Données

## 🎯 Objectifs

- Maîtriser les listes, tuples, sets et dictionnaires
- Créer et utiliser des fonctions
- Organiser le code en modules
- Manipuler des fichiers

---

## 1. Les Listes

### 1.1 Création et Manipulation

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

```python
# Création
fruits = ["pomme", "banane", "orange"]
nombres = [1, 2, 3, 4, 5]
mixte = [1, "deux", 3.0, True]
vide = []

# Accès aux éléments
premier = fruits[0]      # "pomme"
dernier = fruits[-1]     # "orange"
deuxieme = fruits[1]     # "banane"

# Slicing (tranches)
fruits = ["a", "b", "c", "d", "e"]
print(fruits[1:3])   # ["b", "c"]
print(fruits[:2])    # ["a", "b"]
print(fruits[2:])    # ["c", "d", "e"]
print(fruits[::2])   # ["a", "c", "e"]
```

</div>

<div>

```python
# Modification
fruits[0] = "fraise"
fruits[-1] = "kiwi"

# Longueur
taille = len(fruits)  # 5

# Vérifier existence
if "banane" in fruits:
    print("Banane trouvée!")

# Parcourir
for fruit in fruits:
    print(fruit)

for i, fruit in enumerate(fruits):
    print(f"{i}: {fruit}")
```

</div>

</div>

### 1.2 Méthodes de Liste

```python
nombres = [3, 1, 4, 1, 5]

# Ajouter
nombres.append(9)           # [3, 1, 4, 1, 5, 9]
nombres.insert(0, 0)        # [0, 3, 1, 4, 1, 5, 9]
nombres.extend([2, 6])      # [0, 3, 1, 4, 1, 5, 9, 2, 6]

# Retirer
nombres.remove(1)           # Retire le premier 1
dernier = nombres.pop()     # Retire et retourne le dernier
element = nombres.pop(2)    # Retire à l'index 2

# Trier
nombres.sort()              # Trie en place
nombres_tries = sorted(nombres)  # Nouvelle liste triée

# Inverser
nombres.reverse()           # Inverse en place

# Compter
count = nombres.count(1)    # Nombre d'occurrences de 1

# Trouver index
index = nombres.index(5)    # Index du premier 5
```

### 1.3 List Comprehensions

```python
# Méthode classique
carres = []
for x in range(10):
    carres.append(x ** 2)

# List comprehension (Pythonic!)
carres = [x ** 2 for x in range(10)]

# Avec condition
pairs = [x for x in range(20) if x % 2 == 0]

# Transformation
noms = ["alice", "bob", "charlie"]
majuscules = [nom.upper() for nom in noms]

# Exemples avancés
matrice = [[i*j for j in range(5)] for i in range(5)]

mots = ["hello", "world", "python"]
longueurs = [len(mot) for mot in mots]
```

---

## 2. Les Tuples

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

### Immutables (Non Modifiables)

```python
# Création
coordonnees = (10, 20)
rgb = (255, 128, 0)
singleton = (42,)  # Virgule nécessaire
sans_parentheses = 1, 2, 3

# Accès
x, y = coordonnees  # Unpacking
r, g, b = rgb

# Indexation (comme listes)
premier = coordonnees[0]
dernier = rgb[-1]

# Slicing
nombres = (0, 1, 2, 3, 4, 5)
slice1 = nombres[1:4]  # (1, 2, 3)
```

</div>

<div>

### Quand Utiliser?

**Tuples:**
- Données immuables
- Retours de fonctions multiples
- Clés de dictionnaires
- Performance (plus rapide)

```python
# Retour de fonction
def get_stats(data):
    return min(data), max(data), sum(data)

mini, maxi, total = get_stats([1, 2, 3, 4, 5])

# Swap de variables
a, b = 10, 20
a, b = b, a  # Swap élégant!
```

</div>

</div>

---

## 3. Les Sets (Ensembles)

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

### Non Ordonnés, Sans Doublons

```python
# Création
nombres = {1, 2, 3, 4, 5}
vide = set()  # Pas {} (dict)
from_list = set([1, 2, 2, 3, 3, 3])
# Résultat: {1, 2, 3}

# Ajouter/Retirer
nombres.add(6)
nombres.remove(1)  # Erreur si absent
nombres.discard(1)  # Pas d'erreur

# Opérations ensemblistes
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

union = a | b          # {1,2,3,4,5,6}
intersection = a & b   # {3, 4}
difference = a - b     # {1, 2}
sym_diff = a ^ b       # {1,2,5,6}
```

</div>

<div>

### Utilisation

```python
# Éliminer doublons
data = [1, 2, 2, 3, 3, 3, 4, 4]
unique = list(set(data))  # [1,2,3,4]

# Test d'appartenance (rapide!)
whitelist = {"alice", "bob", "charlie"}

if user in whitelist:
    print("Accès autorisé")

# Comparaisons
a = {1, 2, 3}
b = {1, 2}

print(b < a)      # True (sous-ensemble)
print(a > b)      # True (sur-ensemble)
print(a == b)     # False
```

</div>

</div>

---

## 4. Les Dictionnaires

### 4.1 Paires Clé-Valeur

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

```python
# Création
etudiant = {
    "nom": "Alice",
    "age": 20,
    "note": 85,
    "actif": True
}

# Autre syntaxe
vide = {}
dict_from_pairs = dict([("a", 1), ("b", 2)])

# Accès
nom = etudiant["nom"]
age = etudiant.get("age")
defaut = etudiant.get("email", "N/A")

# Modification
etudiant["note"] = 90
etudiant["email"] = "alice@uni.com"

# Suppression
del etudiant["actif"]
note = etudiant.pop("note")
```

</div>

<div>

```python
# Itération
for cle in etudiant:
    print(cle)

for cle, valeur in etudiant.items():
    print(f"{cle}: {valeur}")

for valeur in etudiant.values():
    print(valeur)

# Méthodes utiles
cles = list(etudiant.keys())
valeurs = list(etudiant.values())

# Vérifier existence
if "nom" in etudiant:
    print("Nom existe")

# Fusion de dictionnaires
d1 = {"a": 1, "b": 2}
d2 = {"b": 3, "c": 4}
d1.update(d2)  # {"a": 1, "b": 3, "c": 4}
```

</div>

</div>

### 4.2 Dictionnaires Imbriqués

```python
# Base de données simple
etudiants = {
    "E001": {
        "nom": "Alice",
        "age": 20,
        "notes": [85, 90, 88]
    },
    "E002": {
        "nom": "Bob",
        "age": 22,
        "notes": [78, 82, 80]
    }
}

# Accès
nom_alice = etudiants["E001"]["nom"]
premiere_note = etudiants["E001"]["notes"][0]

# Calcul de moyennes
for id, data in etudiants.items():
    moyenne = sum(data["notes"]) / len(data["notes"])
    print(f"{data['nom']}: {moyenne:.2f}")
```

---

## 5. Les Fonctions

### 5.1 Définition et Appel

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

```python
# Fonction simple
def saluer():
    print("Bonjour!")

saluer()  # Appel

# Avec paramètres
def saluer_personne(nom):
    print(f"Bonjour {nom}!")

saluer_personne("Alice")

# Avec retour
def carre(x):
    return x ** 2

resultat = carre(5)  # 25

# Plusieurs paramètres
def ajouter(a, b):
    return a + b

somme = ajouter(3, 5)  # 8
```

</div>

<div>

```python
# Paramètres par défaut
def saluer(nom, titre="M./Mme"):
    print(f"Bonjour {titre} {nom}")

saluer("Dupont")              # M./Mme
saluer("Martin", "Dr")        # Dr

# Retours multiples
def stats(data):
    return min(data), max(data), sum(data)

mini, maxi, total = stats([1, 2, 3])

# Arguments variables
def somme(*nombres):
    return sum(nombres)

print(somme(1, 2, 3, 4, 5))  # 15

# Keyword arguments
def info(**kwargs):
    for key, val in kwargs.items():
        print(f"{key}: {val}")

info(nom="Alice", age=20)
```

</div>

</div>

### 5.2 Fonctions Lambda

```python
# Lambda = fonction anonyme courte
carre = lambda x: x ** 2
print(carre(5))  # 25

# Avec map
nombres = [1, 2, 3, 4, 5]
carres = list(map(lambda x: x ** 2, nombres))

# Avec filter
pairs = list(filter(lambda x: x % 2 == 0, nombres))

# Avec sorted
etudiants = [
    {"nom": "Bob", "note": 78},
    {"nom": "Alice", "note": 92},
    {"nom": "Charlie", "note": 85}
]

# Trier par note
tries = sorted(etudiants, key=lambda e: e["note"], reverse=True)
```

---

## 6. Modules et Imports

### 6.1 Modules Standards

```python
# Math
import math

print(math.pi)           # 3.14159...
print(math.sqrt(16))     # 4.0
print(math.ceil(3.2))    # 4
print(math.floor(3.8))   # 3

# Random
import random

nombre = random.randint(1, 100)
choix = random.choice(["A", "B", "C"])
melange = [1, 2, 3, 4, 5]
random.shuffle(melange)

# Datetime
from datetime import datetime, timedelta

maintenant = datetime.now()
print(maintenant.strftime("%Y-%m-%d %H:%M:%S"))

demain = maintenant + timedelta(days=1)

# OS
import os

print(os.getcwd())         # Répertoire actuel
os.listdir(".")            # Fichiers du dossier
os.path.exists("file.txt") # Vérifier existence
```

### 6.2 Créer vos Modules

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

**math_utils.py**
```python
"""Module d'utilitaires mathématiques"""

def carre(x):
    """Retourne le carré de x"""
    return x ** 2

def cube(x):
    """Retourne le cube de x"""
    return x ** 3

PI = 3.14159

if __name__ == "__main__":
    # Tests
    print(carre(5))
    print(cube(3))
```

</div>

<div>

**main.py**
```python
# Importer le module
import math_utils

print(math_utils.carre(5))
print(math_utils.PI)

# Import spécifique
from math_utils import carre, cube

print(carre(4))
print(cube(2))

# Import avec alias
import math_utils as mu

print(mu.carre(3))
```

</div>

</div>

---

## 7. Gestion de Fichiers

### 7.1 Lecture de Fichiers

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

```python
# Lecture complète
with open("data.txt", "r") as f:
    contenu = f.read()
    print(contenu)

# Ligne par ligne
with open("data.txt", "r") as f:
    for ligne in f:
        print(ligne.strip())

# Liste de lignes
with open("data.txt", "r") as f:
    lignes = f.readlines()
```

</div>

<div>

```python
# Lecture CSV manuel
with open("data.csv", "r") as f:
    for ligne in f:
        valeurs = ligne.strip().split(",")
        print(valeurs)

# Encoding UTF-8
with open("french.txt", "r", 
          encoding="utf-8") as f:
    texte = f.read()
```

</div>

</div>

### 7.2 Écriture de Fichiers

```python
# Écriture (écrase le fichier)
with open("output.txt", "w") as f:
    f.write("Première ligne\n")
    f.write("Deuxième ligne\n")

# Ajout (append)
with open("output.txt", "a") as f:
    f.write("Ligne supplémentaire\n")

# Écrire des listes
lignes = ["Ligne 1\n", "Ligne 2\n", "Ligne 3\n"]
with open("output.txt", "w") as f:
    f.writelines(lignes)

# CSV simple
data = [
    ["Nom", "Age", "Note"],
    ["Alice", "20", "85"],
    ["Bob", "22", "78"]
]

with open("etudiants.csv", "w") as f:
    for row in data:
        f.write(",".join(row) + "\n")
```

---

## 📝 Exercices Pratiques

### Exercice 1: Gestion de Contacts

```python
# Créer un dictionnaire de contacts
# Permettre: ajouter, supprimer, rechercher, afficher tous
```

<details>
<summary>Solution</summary>

```python
contacts = {}

def ajouter_contact(nom, telephone):
    contacts[nom] = telephone
    print(f"Contact {nom} ajouté")

def supprimer_contact(nom):
    if nom in contacts:
        del contacts[nom]
        print(f"Contact {nom} supprimé")
    else:
        print("Contact non trouvé")

def rechercher_contact(nom):
    return contacts.get(nom, "Non trouvé")

def afficher_tous():
    for nom, tel in contacts.items():
        print(f"{nom}: {tel}")

# Test
ajouter_contact("Alice", "123-456")
ajouter_contact("Bob", "789-012")
afficher_tous()
print(rechercher_contact("Alice"))
```
</details>

### Exercice 2: Analyse de Texte

```python
# Fonction qui compte:
# - Nombre de mots
# - Nombre de caractères
# - Mots les plus fréquents
```

<details>
<summary>Solution</summary>

```python
def analyser_texte(texte):
    # Nombre de mots
    mots = texte.split()
    nb_mots = len(mots)
    
    # Nombre de caractères
    nb_chars = len(texte)
    
    # Fréquence des mots
    freq = {}
    for mot in mots:
        mot = mot.lower().strip(".,!?")
        freq[mot] = freq.get(mot, 0) + 1
    
    # Top 5
    top5 = sorted(freq.items(), 
                  key=lambda x: x[1], 
                  reverse=True)[:5]
    
    return {
        "mots": nb_mots,
        "caracteres": nb_chars,
        "top_mots": top5
    }

texte = "Python est génial. Python est puissant. J'aime Python."
resultat = analyser_texte(texte)
print(resultat)
```
</details>

### Exercice 3: Gestionnaire de Notes

Créez un système complet avec fichiers pour:
- Ajouter des étudiants et notes
- Calculer statistiques
- Sauvegarder/charger depuis fichier

---

## 🎯 Points Clés

✅ **Listes**: Ordonnées, modifiables, doublons autorisés  
✅ **Tuples**: Ordonnés, immutables  
✅ **Sets**: Non ordonnés, sans doublons  
✅ **Dicts**: Paires clé-valeur, accès rapide  
✅ **Fonctions**: Réutilisabilité du code  
✅ **Modules**: Organisation du code  
✅ **Fichiers**: Persistance des données  

---

## ➡️ Prochaine Étape

[Module 5: Python - POO →](./module05-python-oop.md)

---

*© 2025 - Formation Data Mining Professionnelle*
