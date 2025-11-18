# Module 5: Python - Programmation Orientée Objet

## 🎯 Objectifs

- Comprendre les concepts de la POO
- Créer et utiliser des classes
- Maîtriser l'héritage et le polymorphisme
- Gérer les exceptions

---

## 1. Classes et Objets

### 1.1 Première Classe

```python
class Etudiant:
    """Représente un étudiant"""
    
    def __init__(self, nom, age):
        self.nom = nom
        self.age = age
        self.notes = []
    
    def ajouter_note(self, note):
        self.notes.append(note)
    
    def moyenne(self):
        if self.notes:
            return sum(self.notes) / len(self.notes)
        return 0
    
    def __str__(self):
        return f"Étudiant: {self.nom}, {self.age} ans"

# Utilisation
alice = Etudiant("Alice", 20)
alice.ajouter_note(85)
alice.ajouter_note(90)
print(alice.moyenne())  # 87.5
print(alice)  # Étudiant: Alice, 20 ans
```

### 1.2 Attributs de Classe

```python
class Compteur:
    nombre_instances = 0  # Attribut de classe
    
    def __init__(self, nom):
        self.nom = nom  # Attribut d'instance
        Compteur.nombre_instances += 1
    
    @classmethod
    def get_nombre(cls):
        return cls.nombre_instances

c1 = Compteur("A")
c2 = Compteur("B")
print(Compteur.get_nombre())  # 2
```

---

## 2. Héritage

```python
class Personne:
    def __init__(self, nom, age):
        self.nom = nom
        self.age = age
    
    def se_presenter(self):
        return f"Je suis {self.nom}, {self.age} ans"

class Etudiant(Personne):
    def __init__(self, nom, age, matricule):
        super().__init__(nom, age)
        self.matricule = matricule
        self.notes = []
    
    def se_presenter(self):
        base = super().se_presenter()
        return f"{base}, matricule {self.matricule}"

class Professeur(Personne):
    def __init__(self, nom, age, matiere):
        super().__init__(nom, age)
        self.matiere = matiere
    
    def se_presenter(self):
        base = super().se_presenter()
        return f"{base}, j'enseigne {self.matiere}"

# Utilisation
e = Etudiant("Alice", 20, "E001")
p = Professeur("Dr. Smith", 45, "Data Mining")
print(e.se_presenter())
print(p.se_presenter())
```

---

## 3. Gestion des Exceptions

### 3.1 Try-Except

```python
# Exception de base
try:
    nombre = int(input("Entrez un nombre: "))
    resultat = 100 / nombre
    print(f"Résultat: {resultat}")
except ValueError:
    print("Erreur: Ce n'est pas un nombre valide")
except ZeroDivisionError:
    print("Erreur: Division par zéro")
except Exception as e:
    print(f"Erreur inattendue: {e}")
else:
    print("Opération réussie")
finally:
    print("Fin du programme")
```

### 3.2 Lever des Exceptions

```python
class CompteEnBanque:
    def __init__(self, solde=0):
        self.solde = solde
    
    def retirer(self, montant):
        if montant < 0:
            raise ValueError("Le montant doit être positif")
        if montant > self.solde:
            raise ValueError("Solde insuffisant")
        self.solde -= montant
    
    def deposer(self, montant):
        if montant <= 0:
            raise ValueError("Le montant doit être positif")
        self.solde += montant

# Utilisation
compte = CompteEnBanque(1000)
try:
    compte.retirer(1500)
except ValueError as e:
    print(f"Erreur: {e}")
```

---

## 📝 Exercice: Système de Bibliothèque

Créez un système avec les classes:
- `Livre` (titre, auteur, ISBN, disponible)
- `Membre` (nom, id, livres_empruntes)
- `Bibliotheque` (livres, membres)

<details>
<summary>Solution</summary>

```python
class Livre:
    def __init__(self, titre, auteur, isbn):
        self.titre = titre
        self.auteur = auteur
        self.isbn = isbn
        self.disponible = True
    
    def __str__(self):
        statut = "Disponible" if self.disponible else "Emprunté"
        return f"{self.titre} par {self.auteur} [{statut}]"

class Membre:
    def __init__(self, nom, id_membre):
        self.nom = nom
        self.id_membre = id_membre
        self.livres_empruntes = []
    
    def emprunter(self, livre):
        if not livre.disponible:
            raise ValueError("Livre non disponible")
        if len(self.livres_empruntes) >= 3:
            raise ValueError("Maximum 3 livres")
        livre.disponible = False
        self.livres_empruntes.append(livre)
    
    def rendre(self, livre):
        if livre not in self.livres_empruntes:
            raise ValueError("Livre non emprunté par ce membre")
        livre.disponible = True
        self.livres_empruntes.remove(livre)

class Bibliotheque:
    def __init__(self):
        self.livres = []
        self.membres = []
    
    def ajouter_livre(self, livre):
        self.livres.append(livre)
    
    def ajouter_membre(self, membre):
        self.membres.append(membre)
    
    def afficher_livres_disponibles(self):
        for livre in self.livres:
            if livre.disponible:
                print(livre)

# Test
bib = Bibliotheque()
livre1 = Livre("Python pour la Data Science", "McKinney", "123")
membre1 = Membre("Alice", "M001")

bib.ajouter_livre(livre1)
bib.ajouter_membre(membre1)
membre1.emprunter(livre1)
bib.afficher_livres_disponibles()
```
</details>

---

## ➡️ Prochaine Étape

[Module 6: Python pour la Data Science →](./module06-python-data-science.md)

---

*© 2025 - Formation Data Mining Professionnelle*
