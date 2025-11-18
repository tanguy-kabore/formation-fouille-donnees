# Module 3: Python - Partie 1: Les Bases

## 🎯 Objectifs d'Apprentissage

- Écrire et exécuter des programmes Python
- Utiliser les types de données et variables
- Maîtriser les opérateurs
- Implémenter les structures de contrôle (if, for, while)

---

## 1. Introduction à Python

### Pourquoi Python pour le Data Mining?

✅ **Simple et lisible** - Syntaxe claire  
✅ **Bibliothèques puissantes** - NumPy, Pandas, Scikit-learn  
✅ **Communauté active** - Support et ressources  
✅ **Polyvalent** - Web, Data Science, AI, Automation  

### Installation

```bash
# Vérifier l'installation
python --version  # Python 3.10+
pip --version

# Installer des packages
pip install numpy pandas matplotlib
```

---

## 2. Premier Programme Python

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

### hello.py

```python
# Commentaire: Mon premier programme
print("Hello, World!")
print("Bienvenue en Data Mining")

# Variables
nom = "Alice"
age = 25
print(f"Je m'appelle {nom}")
print(f"J'ai {age} ans")
```

</div>

<div>

### Exécution

```bash
# Dans le terminal
python hello.py

# Sortie:
# Hello, World!
# Bienvenue en Data Mining
# Je m'appelle Alice
# J'ai 25 ans
```

### Python Interactif

```python
>>> 2 + 2
4
>>> print("Test")
Test
```

</div>

</div>

---

## 3. Variables et Types de Données

### 3.1 Types de Base

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

```python
# Entiers (int)
age = 25
population = 1000000
negatif = -42

# Flottants (float)
prix = 19.99
temperature = -5.5
pi = 3.14159

# Chaînes de caractères (str)
nom = "Alice"
ville = 'Paris'
description = """Texte
multi-lignes"""

# Booléens (bool)
actif = True
termine = False
```

</div>

<div>

```python
# Type None (valeur nulle)
resultat = None

# Vérifier le type
print(type(age))        # <class 'int'>
print(type(prix))       # <class 'float'>
print(type(nom))        # <class 'str'>
print(type(actif))      # <class 'bool'>

# Conversion de types
x = "42"
y = int(x)     # Convertir en int
z = float(x)   # Convertir en float

nombre = 100
texte = str(nombre)  # "100"
```

</div>

</div>

### 3.2 Conventions de Nommage

```python
# ✅ Bon
nom_complet = "Jean Dupont"
age_utilisateur = 30
CONSTANTE_PI = 3.14159

# ❌ Mauvais
NomComplet = "Jean"  # Style Java (éviter en Python)
2_nom = "Test"        # Ne peut pas commencer par un chiffre
nom-complet = "Test"  # Tirets interdits
```

---

## 4. Opérateurs

### 4.1 Opérateurs Arithmétiques

```python
a = 10
b = 3

addition = a + b       # 13
soustraction = a - b   # 7
multiplication = a * b # 30
division = a / b       # 3.333...
division_entiere = a // b  # 3
modulo = a % b         # 1 (reste)
puissance = a ** b     # 1000

# Opérateurs raccourcis
x = 5
x += 3   # x = x + 3 → 8
x -= 2   # x = x - 2 → 6
x *= 4   # x = x * 4 → 24
x /= 3   # x = x / 3 → 8.0
```

### 4.2 Opérateurs de Comparaison

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

```python
x = 10
y = 20

egal = (x == y)          # False
different = (x != y)     # True
inferieur = (x < y)      # True
superieur = (x > y)      # False
inf_egal = (x <= 10)     # True
sup_egal = (y >= 20)     # True
```

</div>

<div>

```python
# Comparaisons multiples
age = 25
majeur = (age >= 18)     # True

# Comparaison de chaînes
nom1 = "Alice"
nom2 = "Bob"
print(nom1 == nom2)      # False
print(nom1 < nom2)       # True (ordre alphabétique)
```

</div>

</div>

### 4.3 Opérateurs Logiques

```python
# AND, OR, NOT
age = 25
salaire = 50000

# AND - toutes les conditions doivent être vraies
eligible = (age >= 18) and (salaire > 30000)  # True

# OR - au moins une condition vraie
reduit = (age < 18) or (age > 65)  # False

# NOT - inverse la valeur
non_eligible = not eligible  # False

# Combinaisons
complexe = (age > 20 and salaire > 40000) or (age < 30 and salaire > 35000)
```

---

## 5. Structures de Contrôle

### 5.1 Conditions (if-elif-else)

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

```python
# If simple
age = 20

if age >= 18:
    print("Vous êtes majeur")
    print("Vous pouvez voter")

# If-else
temperature = 25

if temperature > 30:
    print("Il fait chaud")
else:
    print("Température agréable")
```

</div>

<div>

```python
# If-elif-else
note = 85

if note >= 90:
    grade = "A"
    print("Excellent!")
elif note >= 80:
    grade = "B"
    print("Très bien")
elif note >= 70:
    grade = "C"
    print("Bien")
elif note >= 60:
    grade = "D"
    print("Passable")
else:
    grade = "F"
    print("Insuffisant")
```

</div>

</div>

### 5.2 Boucle For

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

```python
# Boucle de base
for i in range(5):
    print(i)
# Affiche: 0, 1, 2, 3, 4

# Range avec début et fin
for i in range(2, 7):
    print(i)
# Affiche: 2, 3, 4, 5, 6

# Range avec pas
for i in range(0, 10, 2):
    print(i)
# Affiche: 0, 2, 4, 6, 8

# Décroissant
for i in range(5, 0, -1):
    print(i)
# Affiche: 5, 4, 3, 2, 1
```

</div>

<div>

```python
# Parcourir une chaîne
for lettre in "Python":
    print(lettre)
# Affiche: P, y, t, h, o, n

# Parcourir une liste
fruits = ["pomme", "banane", "orange"]
for fruit in fruits:
    print(f"J'aime les {fruit}s")

# Enumerate (index + valeur)
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")
# 0: pomme
# 1: banane
# 2: orange
```

</div>

</div>

### 5.3 Boucle While

```python
# While basique
compteur = 0
while compteur < 5:
    print(f"Compteur: {compteur}")
    compteur += 1

# Avec condition complexe
nombre = 100
while nombre > 1:
    nombre = nombre // 2
    print(nombre)

# Boucle infinie avec break
while True:
    reponse = input("Voulez-vous continuer? (o/n): ")
    if reponse.lower() == 'n':
        break
    print("On continue...")
```

### 5.4 Break, Continue, Pass

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

```python
# BREAK - Sortir de la boucle
for i in range(10):
    if i == 5:
        break
    print(i)
# Affiche: 0, 1, 2, 3, 4

# CONTINUE - Passer à l'itération suivante
for i in range(5):
    if i == 2:
        continue
    print(i)
# Affiche: 0, 1, 3, 4
```

</div>

<div>

```python
# PASS - Ne rien faire (placeholder)
for i in range(5):
    if i == 2:
        pass  # TODO: implémenter plus tard
    print(i)
# Affiche: 0, 1, 2, 3, 4

# Utile pour définir des structures vides
def fonction_future():
    pass  # À implémenter
```

</div>

</div>

---

## 6. Entrées/Sorties

### 6.1 Print (Sortie)

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

```python
# Print simple
print("Hello")
print(42)
print(3.14)

# Print multiple
print("Nom:", "Alice", "Age:", 25)

# Séparateur personnalisé
print("A", "B", "C", sep="-")
# Affiche: A-B-C

# Sans retour à la ligne
print("Début", end=" ")
print("Fin")
# Affiche: Début Fin
```

</div>

<div>

```python
# F-strings (recommandé)
nom = "Bob"
age = 30
print(f"Je suis {nom} et j'ai {age} ans")

# Formatage de nombres
pi = 3.14159
print(f"Pi = {pi:.2f}")  # Pi = 3.14

prix = 1234.56
print(f"Prix: {prix:,.2f} €")
# Prix: 1,234.56 €

# Format method (ancien)
print("Nom: {}, Age: {}".format(nom, age))
```

</div>

</div>

### 6.2 Input (Entrée)

```python
# Input basique
nom = input("Entrez votre nom: ")
print(f"Bonjour {nom}!")

# Convertir en nombre
age_str = input("Entrez votre âge: ")
age = int(age_str)

if age >= 18:
    print("Vous êtes majeur")

# En une ligne
nombre = int(input("Entrez un nombre: "))
carre = nombre ** 2
print(f"Le carré de {nombre} est {carre}")
```

---

## 📝 Exercices Pratiques

### Exercice 1: Calculatrice Simple

```python
# Créer une calculatrice qui:
# 1. Demande deux nombres à l'utilisateur
# 2. Demande l'opération (+, -, *, /)
# 3. Affiche le résultat

# Votre code ici:
```

<details>
<summary>Solution</summary>

```python
# calculatrice.py
num1 = float(input("Premier nombre: "))
num2 = float(input("Deuxième nombre: "))
operation = input("Opération (+, -, *, /): ")

if operation == "+":
    resultat = num1 + num2
elif operation == "-":
    resultat = num1 - num2
elif operation == "*":
    resultat = num1 * num2
elif operation == "/":
    if num2 != 0:
        resultat = num1 / num2
    else:
        print("Erreur: Division par zéro!")
        exit()
else:
    print("Opération invalide!")
    exit()

print(f"Résultat: {num1} {operation} {num2} = {resultat}")
```
</details>

### Exercice 2: Nombre Pair ou Impair

```python
# Programme qui:
# 1. Demande un nombre
# 2. Indique s'il est pair ou impair
# 3. Indique s'il est positif, négatif ou nul

# Votre code ici:
```

<details>
<summary>Solution</summary>

```python
nombre = int(input("Entrez un nombre: "))

# Pair ou impair
if nombre % 2 == 0:
    parite = "pair"
else:
    parite = "impair"

# Positif, négatif, nul
if nombre > 0:
    signe = "positif"
elif nombre < 0:
    signe = "négatif"
else:
    signe = "nul"

print(f"{nombre} est {parite} et {signe}")
```
</details>

### Exercice 3: Table de Multiplication

```python
# Créer un programme qui affiche la table de multiplication
# d'un nombre donné par l'utilisateur (de 1 à 10)

# Exemple de sortie pour 7:
# 7 x 1 = 7
# 7 x 2 = 14
# ...
# 7 x 10 = 70
```

<details>
<summary>Solution</summary>

```python
nombre = int(input("Entrez un nombre: "))

print(f"\nTable de multiplication de {nombre}:")
print("-" * 20)

for i in range(1, 11):
    resultat = nombre * i
    print(f"{nombre} x {i:2} = {resultat}")
```
</details>

### Exercice 4: Somme des N premiers nombres

```python
# Calculer la somme de 1 + 2 + 3 + ... + N
# où N est donné par l'utilisateur

# Bonus: Comparer avec la formule N*(N+1)/2
```

<details>
<summary>Solution</summary>

```python
n = int(input("Entrez N: "))

# Méthode 1: Boucle
somme_boucle = 0
for i in range(1, n + 1):
    somme_boucle += i

# Méthode 2: Formule mathématique
somme_formule = n * (n + 1) // 2

print(f"Somme (boucle): {somme_boucle}")
print(f"Somme (formule): {somme_formule}")
print(f"Égales? {somme_boucle == somme_formule}")
```
</details>

### Exercice 5: Devinez le Nombre

```python
# Créer un jeu où:
# 1. L'ordinateur choisit un nombre entre 1 et 100
# 2. L'utilisateur essaie de deviner
# 3. L'ordinateur indique "trop grand" ou "trop petit"
# 4. Compte le nombre de tentatives

# Indice: utiliser random.randint()
```

<details>
<summary>Solution</summary>

```python
import random

nombre_secret = random.randint(1, 100)
tentatives = 0
trouve = False

print("J'ai choisi un nombre entre 1 et 100.")
print("Essayez de le deviner!")

while not trouve:
    guess = int(input("\nVotre proposition: "))
    tentatives += 1
    
    if guess < nombre_secret:
        print("C'est plus grand!")
    elif guess > nombre_secret:
        print("C'est plus petit!")
    else:
        trouve = True
        print(f"\n🎉 Bravo! Vous avez trouvé en {tentatives} tentatives!")
```
</details>

---

## 🎯 Mini-Projet: Analyse de Notes

Créez un programme complet qui:

1. Demande le nombre d'étudiants
2. Pour chaque étudiant, demande le nom et la note
3. Calcule:
   - La moyenne de la classe
   - La note minimale et maximale
   - Le nombre d'étudiants ayant réussi (note ≥ 60)
4. Affiche un résumé complet

<details>
<summary>Solution Complète</summary>

```python
# analyse_notes.py

print("=== SYSTÈME D'ANALYSE DE NOTES ===\n")

# Demander le nombre d'étudiants
nb_etudiants = int(input("Nombre d'étudiants: "))

# Initialiser les variables
somme_notes = 0
note_min = 100
note_max = 0
nb_reussis = 0

# Collecter les données
print("\n--- Saisie des notes ---")
for i in range(nb_etudiants):
    print(f"\nÉtudiant {i+1}:")
    nom = input("  Nom: ")
    note = float(input("  Note: "))
    
    # Mettre à jour les statistiques
    somme_notes += note
    
    if note < note_min:
        note_min = note
    
    if note > note_max:
        note_max = note
    
    if note >= 60:
        nb_reussis += 1

# Calculer la moyenne
moyenne = somme_notes / nb_etudiants
taux_reussite = (nb_reussis / nb_etudiants) * 100

# Afficher le résumé
print("\n" + "="*40)
print("RÉSUMÉ DES RÉSULTATS")
print("="*40)
print(f"Nombre d'étudiants: {nb_etudiants}")
print(f"Moyenne de classe:  {moyenne:.2f}")
print(f"Note minimale:      {note_min:.2f}")
print(f"Note maximale:      {note_max:.2f}")
print(f"Étudiants réussis:  {nb_reussis} ({taux_reussite:.1f}%)")
print("="*40)

# Évaluation de la classe
if moyenne >= 80:
    evaluation = "Excellente classe! 🌟"
elif moyenne >= 70:
    evaluation = "Bonne classe 👍"
elif moyenne >= 60:
    evaluation = "Classe moyenne"
else:
    evaluation = "Classe en difficulté - besoin de soutien"

print(f"\nÉvaluation: {evaluation}")
```
</details>

---

## 🎯 Points Clés à Retenir

✅ Python utilise l'**indentation** (espaces) pour structurer le code  
✅ Les **variables** stockent des données de différents types  
✅ Les **opérateurs** permettent de manipuler les données  
✅ **if-elif-else** pour les décisions  
✅ **for** pour les boucles avec nombre d'itérations connu  
✅ **while** pour les boucles avec condition  
✅ **input()** pour lire, **print()** pour afficher  

---

## 📚 Ressources

- [Python.org Official Tutorial](https://docs.python.org/3/tutorial/)
- [W3Schools Python](https://www.w3schools.com/python/)
- [Real Python](https://realpython.com/)

---

## ➡️ Prochaine Étape

[Module 4: Python - Structures de Données →](./module04-python-data-structures.md)

---

*© 2024 - Formation Data Mining Professionnelle*
