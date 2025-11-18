# Module 9: Java - Fondamentaux

## 🎯 Objectifs

- Comprendre Java et la JVM
- Maîtriser la syntaxe et la POO en Java
- Utiliser les Collections Framework
- Gérer les exceptions

---

## 1. Introduction à Java

### 1.1 Pourquoi Java pour le Data Mining?

✅ **Hadoop** et **Spark** sont écrits en Java/Scala  
✅ **Performance** pour le traitement de gros volumes  
✅ **Scalabilité** et multithread natif  
✅ **Écosystème Big Data** mature  

### 1.2 Installation

```bash
# Vérifier Java
java -version
javac -version

# Si non installé:
# Windows: télécharger JDK depuis Oracle ou Adoptium
# Linux: sudo apt install openjdk-17-jdk
# Mac: brew install openjdk@17
```

---

## 2. Premier Programme Java

### 2.1 Hello World

```java
// HelloWorld.java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
        System.out.println("Bienvenue en Java!");
    }
}
```

**Compilation et exécution:**
```bash
javac HelloWorld.java  # Compile → HelloWorld.class
java HelloWorld        # Exécute
```

### 2.2 Structure de Base

```java
// Programme Java complet
public class MonProgramme {
    // Variables de classe (static)
    private static int compteur = 0;
    
    // Méthode main (point d'entrée)
    public static void main(String[] args) {
        // Variables locales
        int nombre = 42;
        String texte = "Java";
        
        // Appel de méthodes
        afficherMessage(texte);
        calculer(10, 20);
    }
    
    // Méthode static
    public static void afficherMessage(String msg) {
        System.out.println("Message: " + msg);
    }
    
    // Méthode avec retour
    public static int calculer(int a, int b) {
        return a + b;
    }
}
```

---

## 3. Types de Données et Variables

### 3.1 Types Primitifs

```java
// Entiers
byte b = 127;          // -128 à 127
short s = 32000;       // -32,768 à 32,767
int i = 2000000;       // -2^31 à 2^31-1
long l = 9000000000L;  // -2^63 à 2^63-1

// Flottants
float f = 3.14f;       // 32 bits
double d = 3.14159;    // 64 bits (par défaut)

// Caractère
char c = 'A';

// Booléen
boolean actif = true;
boolean termine = false;
```

### 3.2 Strings et Tableaux

```java
// String (objet, pas primitif)
String nom = "Alice";
String prenom = "Dupont";
String complet = nom + " " + prenom;

// Méthodes String
int longueur = nom.length();
char premier = nom.charAt(0);
String majuscule = nom.toUpperCase();
boolean contient = nom.contains("lic");
String[] parts = "a,b,c".split(",");

// Tableaux
int[] nombres = {1, 2, 3, 4, 5};
String[] fruits = new String[3];
fruits[0] = "Pomme";
fruits[1] = "Banane";

// Longueur
System.out.println(nombres.length);  // 5

// Parcourir
for (int n : nombres) {
    System.out.println(n);
}
```

---

## 4. POO en Java

### 4.1 Classes et Objets

```java
public class Etudiant {
    // Attributs (private)
    private String nom;
    private int age;
    private double[] notes;
    
    // Constructeur
    public Etudiant(String nom, int age) {
        this.nom = nom;
        this.age = age;
        this.notes = new double[10];
    }
    
    // Getters/Setters
    public String getNom() {
        return nom;
    }
    
    public void setNom(String nom) {
        this.nom = nom;
    }
    
    public int getAge() {
        return age;
    }
    
    // Méthodes
    public void ajouterNote(double note) {
        // Implementation
    }
    
    public double calculerMoyenne() {
        double somme = 0;
        int count = 0;
        for (double note : notes) {
            if (note > 0) {
                somme += note;
                count++;
            }
        }
        return count > 0 ? somme / count : 0;
    }
    
    @Override
    public String toString() {
        return "Etudiant: " + nom + ", " + age + " ans";
    }
}

// Utilisation
Etudiant alice = new Etudiant("Alice", 20);
alice.ajouterNote(85.5);
System.out.println(alice.calculerMoyenne());
```

### 4.2 Héritage

```java
// Classe parent
public class Personne {
    protected String nom;
    protected int age;
    
    public Personne(String nom, int age) {
        this.nom = nom;
        this.age = age;
    }
    
    public void sePresenter() {
        System.out.println("Je suis " + nom + ", " + age + " ans");
    }
}

// Classe enfant
public class Etudiant extends Personne {
    private String matricule;
    
    public Etudiant(String nom, int age, String matricule) {
        super(nom, age);  // Appel constructeur parent
        this.matricule = matricule;
    }
    
    @Override
    public void sePresenter() {
        super.sePresenter();
        System.out.println("Matricule: " + matricule);
    }
}
```

---

## 5. Collections Framework

### 5.1 ArrayList

```java
import java.util.ArrayList;
import java.util.Collections;

ArrayList<String> fruits = new ArrayList<>();

// Ajouter
fruits.add("Pomme");
fruits.add("Banane");
fruits.add("Orange");

// Accéder
String premier = fruits.get(0);

// Taille
int taille = fruits.size();

// Modifier
fruits.set(1, "Fraise");

// Supprimer
fruits.remove(0);
fruits.remove("Orange");

// Parcourir
for (String fruit : fruits) {
    System.out.println(fruit);
}

// Trier
Collections.sort(fruits);

// Chercher
boolean existe = fruits.contains("Pomme");
int index = fruits.indexOf("Banane");
```

### 5.2 HashMap

```java
import java.util.HashMap;
import java.util.Map;

HashMap<String, Integer> ages = new HashMap<>();

// Ajouter
ages.put("Alice", 20);
ages.put("Bob", 22);
ages.put("Charlie", 21);

// Accéder
int ageAlice = ages.get("Alice");
int ageDefault = ages.getOrDefault("David", 0);

// Vérifier
boolean existe = ages.containsKey("Bob");

// Supprimer
ages.remove("Charlie");

// Parcourir
for (Map.Entry<String, Integer> entry : ages.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}

// Clés et valeurs
for (String nom : ages.keySet()) {
    System.out.println(nom);
}

for (int age : ages.values()) {
    System.out.println(age);
}
```

### 5.3 HashSet

```java
import java.util.HashSet;

HashSet<String> tags = new HashSet<>();

// Ajouter
tags.add("python");
tags.add("java");
tags.add("data");
tags.add("python");  // Pas de doublon

// Taille
System.out.println(tags.size());  // 3

// Vérifier
boolean existe = tags.contains("java");

// Parcourir
for (String tag : tags) {
    System.out.println(tag);
}
```

---

## 6. Gestion des Exceptions

```java
public class ExceptionDemo {
    public static void main(String[] args) {
        // Try-catch basique
        try {
            int resultat = diviser(10, 0);
            System.out.println(resultat);
        } catch (ArithmeticException e) {
            System.out.println("Erreur: Division par zéro");
        } catch (Exception e) {
            System.out.println("Erreur générale: " + e.getMessage());
        } finally {
            System.out.println("Bloc finally toujours exécuté");
        }
        
        // Lancer une exception
        try {
            verifierAge(-5);
        } catch (IllegalArgumentException e) {
            System.out.println(e.getMessage());
        }
    }
    
    public static int diviser(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("Division par zéro interdite");
        }
        return a / b;
    }
    
    public static void verifierAge(int age) {
        if (age < 0) {
            throw new IllegalArgumentException("L'âge ne peut pas être négatif");
        }
    }
}
```

---

## 📝 Projet: Gestionnaire de Données

```java
import java.util.*;
import java.io.*;

public class DataManager {
    private ArrayList<Map<String, String>> data;
    
    public DataManager() {
        this.data = new ArrayList<>();
    }
    
    public void addRecord(Map<String, String> record) {
        data.add(record);
    }
    
    public void loadCSV(String filename) throws IOException {
        BufferedReader reader = new BufferedReader(new FileReader(filename));
        String line;
        String[] headers = null;
        boolean firstLine = true;
        
        while ((line = reader.readLine()) != null) {
            String[] values = line.split(",");
            
            if (firstLine) {
                headers = values;
                firstLine = false;
            } else {
                Map<String, String> record = new HashMap<>();
                for (int i = 0; i < headers.length; i++) {
                    record.put(headers[i], values[i]);
                }
                data.add(record);
            }
        }
        reader.close();
    }
    
    public void displayRecords() {
        for (Map<String, String> record : data) {
            System.out.println(record);
        }
    }
    
    public List<Map<String, String>> filter(String key, String value) {
        List<Map<String, String>> results = new ArrayList<>();
        for (Map<String, String> record : data) {
            if (record.get(key).equals(value)) {
                results.add(record);
            }
        }
        return results;
    }
    
    public static void main(String[] args) {
        DataManager dm = new DataManager();
        
        // Ajouter des enregistrements
        Map<String, String> r1 = new HashMap<>();
        r1.put("nom", "Alice");
        r1.put("age", "20");
        dm.addRecord(r1);
        
        // Afficher
        dm.displayRecords();
    }
}
```

---

## 🎯 Points Clés

✅ **JVM**: Write Once, Run Anywhere  
✅ **POO stricte**: Tout est objet (sauf primitifs)  
✅ **Collections**: ArrayList, HashMap, HashSet  
✅ **Exceptions**: try-catch-finally  
✅ **Types statiques**: Sécurité au compile-time  

---

## ➡️ Prochaine Étape

[Module 10: Java pour le Big Data →](./module10-java-big-data.md)

---

*© 2024 - Formation Data Mining Professionnelle*
