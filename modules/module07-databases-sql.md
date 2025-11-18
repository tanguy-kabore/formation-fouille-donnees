# Module 7: Bases de Données Relationnelles et SQL

## 🎯 Objectifs

- Comprendre les concepts des bases de données relationnelles
- Maîtriser SQL (CREATE, SELECT, INSERT, UPDATE, DELETE)
- Utiliser les jointures et sous-requêtes
- Normaliser les bases de données
- Connecter Python aux bases de données

---

## 1. Introduction aux Bases de Données

### 1.1 Pourquoi les Bases de Données?

**Avantages:**
- ✅ Stockage persistant et structuré
- ✅ Accès concurrent multiutilisateur
- ✅ Intégrité et cohérence des données
- ✅ Requêtes complexes et rapides
- ✅ Sécurité et permissions

### 1.2 Systèmes de Gestion de BD (SGBD)

| SGBD | Type | Utilisation |
|------|------|-------------|
| **MySQL** | Open Source | Web, e-commerce |
| **PostgreSQL** | Open Source | Data warehouse, analyse |
| **SQLite** | Fichier | Applications mobiles, tests |
| **Oracle** | Commercial | Entreprises |
| **SQL Server** | Commercial | Microsoft ecosystème |

---

## 2. Modèle Relationnel

### 2.1 Concepts de Base

```
TABLE: Etudiants
┌────┬─────────┬──────┬───────────┐
│ ID │ Nom     │ Age  │ Ville     │
├────┼─────────┼──────┼───────────┤
│ 1  │ Alice   │ 20   │ Paris     │
│ 2  │ Bob     │ 22   │ Lyon      │
│ 3  │ Charlie │ 21   │ Paris     │
└────┴─────────┴──────┴───────────┘

- TABLE: Collection de données
- LIGNE (tuple): Un enregistrement
- COLONNE (attribut): Une caractéristique
- CLÉ PRIMAIRE: ID (unique)
```

### 2.2 Types de Données SQL

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

**Numériques:**
```sql
INT, BIGINT         -- Entiers
DECIMAL(10, 2)      -- Décimaux précis
FLOAT, DOUBLE       -- Flottants
```

**Texte:**
```sql
CHAR(10)            -- Fixe, 10 chars
VARCHAR(255)        -- Variable
TEXT                -- Texte long
```

</div>

<div>

**Date/Heure:**
```sql
DATE                -- 2024-01-15
TIME                -- 14:30:00
DATETIME            -- 2024-01-15 14:30:00
TIMESTAMP           -- Auto-update
```

**Autres:**
```sql
BOOLEAN             -- TRUE/FALSE
BLOB                -- Données binaires
```

</div>

</div>

---

## 3. SQL - Langage de Requête

### 3.1 CREATE - Créer des Tables

```sql
-- Créer une base de données
CREATE DATABASE universite;
USE universite;

-- Créer une table Etudiants
CREATE TABLE etudiants (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(100) NOT NULL,
    prenom VARCHAR(100),
    age INT,
    email VARCHAR(255) UNIQUE,
    date_inscription DATE DEFAULT CURRENT_DATE
);

-- Table Cours
CREATE TABLE cours (
    id INT PRIMARY KEY AUTO_INCREMENT,
    code VARCHAR(10) UNIQUE NOT NULL,
    titre VARCHAR(200) NOT NULL,
    credits INT DEFAULT 3
);

-- Table Inscriptions (relation many-to-many)
CREATE TABLE inscriptions (
    etudiant_id INT,
    cours_id INT,
    note DECIMAL(4, 2),
    date_inscription DATE,
    PRIMARY KEY (etudiant_id, cours_id),
    FOREIGN KEY (etudiant_id) REFERENCES etudiants(id),
    FOREIGN KEY (cours_id) REFERENCES cours(id)
);
```

### 3.2 INSERT - Insérer des Données

```sql
-- Insérer un étudiant
INSERT INTO etudiants (nom, prenom, age, email)
VALUES ('Dupont', 'Alice', 20, 'alice@uni.fr');

-- Insérer plusieurs
INSERT INTO etudiants (nom, prenom, age, email) VALUES
    ('Martin', 'Bob', 22, 'bob@uni.fr'),
    ('Bernard', 'Charlie', 21, 'charlie@uni.fr'),
    ('Dubois', 'Diana', 23, 'diana@uni.fr');

-- Insérer des cours
INSERT INTO cours (code, titre, credits) VALUES
    ('CS101', 'Introduction à la Programmation', 3),
    ('MATH201', 'Mathématiques Avancées', 4),
    ('DATA301', 'Data Mining', 3);

-- Inscrire des étudiants aux cours
INSERT INTO inscriptions (etudiant_id, cours_id, note) VALUES
    (1, 1, 85.5),
    (1, 3, 92.0),
    (2, 1, 78.5),
    (2, 2, 88.0);
```

### 3.3 SELECT - Interroger les Données

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

```sql
-- Sélectionner tout
SELECT * FROM etudiants;

-- Colonnes spécifiques
SELECT nom, prenom, email 
FROM etudiants;

-- Avec condition (WHERE)
SELECT * FROM etudiants
WHERE age > 21;

-- Opérateurs logiques
SELECT * FROM etudiants
WHERE age >= 20 AND ville = 'Paris';

SELECT * FROM etudiants
WHERE ville = 'Paris' OR ville = 'Lyon';

-- LIKE (patterns)
SELECT * FROM etudiants
WHERE email LIKE '%@uni.fr';

SELECT * FROM etudiants
WHERE nom LIKE 'D%';  -- Commence par D
```

</div>

<div>

```sql
-- IN (liste de valeurs)
SELECT * FROM etudiants
WHERE ville IN ('Paris', 'Lyon', 'Marseille');

-- BETWEEN
SELECT * FROM etudiants
WHERE age BETWEEN 20 AND 25;

-- ORDER BY (trier)
SELECT * FROM etudiants
ORDER BY age DESC;

SELECT * FROM etudiants
ORDER BY ville ASC, age DESC;

-- LIMIT
SELECT * FROM etudiants
ORDER BY age DESC
LIMIT 5;

-- DISTINCT (valeurs uniques)
SELECT DISTINCT ville 
FROM etudiants;
```

</div>

</div>

### 3.4 Fonctions d'Agrégation

```sql
-- COUNT
SELECT COUNT(*) AS total_etudiants
FROM etudiants;

SELECT COUNT(DISTINCT ville) AS nombre_villes
FROM etudiants;

-- AVG, MIN, MAX, SUM
SELECT 
    AVG(note) AS moyenne,
    MIN(note) AS note_min,
    MAX(note) AS note_max,
    COUNT(*) AS nombre_notes
FROM inscriptions;

-- GROUP BY
SELECT ville, COUNT(*) AS nb_etudiants
FROM etudiants
GROUP BY ville;

SELECT cours_id, 
       AVG(note) AS moyenne,
       COUNT(*) AS nb_etudiants
FROM inscriptions
GROUP BY cours_id;

-- HAVING (filtre après GROUP BY)
SELECT ville, COUNT(*) AS nb
FROM etudiants
GROUP BY ville
HAVING COUNT(*) > 5;
```

### 3.5 UPDATE - Modifier des Données

```sql
-- Modifier un enregistrement
UPDATE etudiants
SET age = 21
WHERE id = 1;

-- Modifier plusieurs colonnes
UPDATE etudiants
SET age = 22, ville = 'Marseille'
WHERE nom = 'Dupont';

-- Modifier avec condition
UPDATE etudiants
SET age = age + 1
WHERE ville = 'Paris';

-- Mettre à jour avec sous-requête
UPDATE inscriptions
SET note = note * 1.05
WHERE cours_id = (SELECT id FROM cours WHERE code = 'CS101');
```

### 3.6 DELETE - Supprimer des Données

```sql
-- Supprimer avec condition
DELETE FROM etudiants
WHERE id = 5;

-- Supprimer plusieurs
DELETE FROM inscriptions
WHERE note < 50;

-- Supprimer tout (ATTENTION!)
DELETE FROM etudiants;  -- Vide la table

-- TRUNCATE (plus rapide, réinitialise auto-increment)
TRUNCATE TABLE etudiants;
```

---

## 4. Jointures (JOINS)

### 4.1 Types de Jointures

```
INNER JOIN: Seulement les correspondances
LEFT JOIN: Tout à gauche + correspondances
RIGHT JOIN: Tout à droite + correspondances
FULL JOIN: Tout

┌─────────┐         ┌─────────┐
│ Table A │         │ Table B │
│  1  2   │         │  2  3   │
└─────────┘         └─────────┘

INNER: {2}
LEFT:  {1, 2}
RIGHT: {2, 3}
FULL:  {1, 2, 3}
```

### 4.2 INNER JOIN

```sql
-- Étudiants avec leurs inscriptions
SELECT 
    e.nom,
    e.prenom,
    c.titre,
    i.note
FROM etudiants e
INNER JOIN inscriptions i ON e.id = i.etudiant_id
INNER JOIN cours c ON i.cours_id = c.id;

-- Moyenne par étudiant
SELECT 
    e.nom,
    e.prenom,
    AVG(i.note) AS moyenne,
    COUNT(i.cours_id) AS nb_cours
FROM etudiants e
INNER JOIN inscriptions i ON e.id = i.etudiant_id
GROUP BY e.id, e.nom, e.prenom
ORDER BY moyenne DESC;
```

### 4.3 LEFT JOIN

```sql
-- Tous les étudiants, même sans inscription
SELECT 
    e.nom,
    e.prenom,
    c.titre,
    i.note
FROM etudiants e
LEFT JOIN inscriptions i ON e.id = i.etudiant_id
LEFT JOIN cours c ON i.cours_id = c.id;

-- Étudiants sans inscription
SELECT e.nom, e.prenom
FROM etudiants e
LEFT JOIN inscriptions i ON e.id = i.etudiant_id
WHERE i.etudiant_id IS NULL;
```

---

## 5. Sous-Requêtes

```sql
-- Sous-requête dans WHERE
SELECT nom, prenom, age
FROM etudiants
WHERE age > (SELECT AVG(age) FROM etudiants);

-- Sous-requête dans FROM
SELECT ville, AVG(moyenne) AS moyenne_ville
FROM (
    SELECT e.ville, e.id, AVG(i.note) AS moyenne
    FROM etudiants e
    JOIN inscriptions i ON e.id = i.etudiant_id
    GROUP BY e.id, e.ville
) AS stats
GROUP BY ville;

-- Sous-requête avec IN
SELECT nom, prenom
FROM etudiants
WHERE id IN (
    SELECT DISTINCT etudiant_id
    FROM inscriptions
    WHERE note >= 90
);

-- EXISTS
SELECT nom, prenom
FROM etudiants e
WHERE EXISTS (
    SELECT 1 FROM inscriptions i
    WHERE i.etudiant_id = e.id AND i.note >= 90
);
```

---

## 6. Normalisation

### 6.1 Formes Normales

**1NF (First Normal Form):**
- Valeurs atomiques (pas de listes)
- Chaque colonne a un type unique
- Pas de groupes répétés

**2NF:**
- Respecte 1NF
- Pas de dépendance partielle à la clé

**3NF:**
- Respecte 2NF
- Pas de dépendance transitive

### 6.2 Exemple de Normalisation

**Avant (Non normalisé):**
```sql
Commandes
┌────┬────────┬────────────┬──────────────┬────────────┐
│ ID │ Client │ Téléphone  │ Produits     │ Prix       │
├────┼────────┼────────────┼──────────────┼────────────┤
│ 1  │ Alice  │ 0123456789 │ A, B, C      │ 10, 20, 15 │
└────┴────────┴────────────┴──────────────┴────────────┘
```

**Après (Normalisé):**
```sql
Clients                    Commandes
┌────┬────────┬──────────┐  ┌────┬───────────┬──────┐
│ ID │ Nom    │ Tel      │  │ ID │ Client_ID │ Date │
├────┼────────┼──────────┤  ├────┼───────────┼──────┤
│ 1  │ Alice  │ 0123...  │  │ 1  │ 1         │ ...  │
└────┴────────┴──────────┘  └────┴───────────┴──────┘

Produits                   Details_Commande
┌────┬──────┬──────┐      ┌────────────┬───────────┬────┐
│ ID │ Nom  │ Prix │      │ Commande_ID│ Produit_ID│ Qté│
├────┼──────┼──────┤      ├────────────┼───────────┼────┤
│ 1  │ A    │ 10   │      │ 1          │ 1         │ 2  │
│ 2  │ B    │ 20   │      │ 1          │ 2         │ 1  │
└────┴──────┴──────┘      └────────────┴───────────┴────┘
```

---

## 7. Python et SQL

### 7.1 SQLite avec Python

```python
import sqlite3
import pandas as pd

# Connexion
conn = sqlite3.connect('universite.db')
cursor = conn.cursor()

# Créer table
cursor.execute('''
    CREATE TABLE IF NOT EXISTS etudiants (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        nom TEXT NOT NULL,
        age INTEGER,
        email TEXT UNIQUE
    )
''')

# Insérer données
cursor.execute(
    "INSERT INTO etudiants (nom, age, email) VALUES (?, ?, ?)",
    ("Alice", 20, "alice@uni.fr")
)

# Insérer plusieurs
etudiants = [
    ("Bob", 22, "bob@uni.fr"),
    ("Charlie", 21, "charlie@uni.fr")
]
cursor.executemany(
    "INSERT INTO etudiants (nom, age, email) VALUES (?, ?, ?)",
    etudiants
)

conn.commit()

# Requête SELECT
cursor.execute("SELECT * FROM etudiants")
rows = cursor.fetchall()
for row in rows:
    print(row)

# Avec Pandas
df = pd.read_sql_query("SELECT * FROM etudiants", conn)
print(df)

# Insérer DataFrame dans SQL
new_data = pd.DataFrame({
    'nom': ['David', 'Eva'],
    'age': [23, 24],
    'email': ['david@uni.fr', 'eva@uni.fr']
})
new_data.to_sql('etudiants', conn, if_exists='append', index=False)

# Fermer
conn.close()
```

### 7.2 MySQL avec Python

```python
import mysql.connector
import pandas as pd

# Connexion
conn = mysql.connector.connect(
    host="localhost",
    user="votre_user",
    password="votre_password",
    database="universite"
)

cursor = conn.cursor()

# Requête
query = "SELECT * FROM etudiants WHERE age > %s"
cursor.execute(query, (20,))
results = cursor.fetchall()

# Avec Pandas
df = pd.read_sql("SELECT * FROM etudiants", conn)

# Fermer
cursor.close()
conn.close()
```

---

## 📝 Projet Pratique: Système E-Commerce

<details>
<summary>Schéma Complet</summary>

```sql
-- Base de données e-commerce

CREATE TABLE clients (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    telephone VARCHAR(20),
    date_inscription DATE DEFAULT CURRENT_DATE
);

CREATE TABLE produits (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(200) NOT NULL,
    description TEXT,
    prix DECIMAL(10, 2) NOT NULL,
    stock INT DEFAULT 0,
    categorie VARCHAR(50)
);

CREATE TABLE commandes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    client_id INT NOT NULL,
    date_commande DATETIME DEFAULT CURRENT_TIMESTAMP,
    statut VARCHAR(20) DEFAULT 'en_cours',
    total DECIMAL(10, 2),
    FOREIGN KEY (client_id) REFERENCES clients(id)
);

CREATE TABLE details_commande (
    id INT PRIMARY KEY AUTO_INCREMENT,
    commande_id INT NOT NULL,
    produit_id INT NOT NULL,
    quantite INT NOT NULL,
    prix_unitaire DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (commande_id) REFERENCES commandes(id),
    FOREIGN KEY (produit_id) REFERENCES produits(id)
);

-- Requêtes analytiques

-- 1. Top 10 produits les plus vendus
SELECT 
    p.nom,
    SUM(dc.quantite) AS total_vendu,
    SUM(dc.quantite * dc.prix_unitaire) AS revenu_total
FROM produits p
JOIN details_commande dc ON p.id = dc.produit_id
GROUP BY p.id, p.nom
ORDER BY total_vendu DESC
LIMIT 10;

-- 2. Clients les plus actifs
SELECT 
    c.nom,
    c.email,
    COUNT(cmd.id) AS nb_commandes,
    SUM(cmd.total) AS montant_total
FROM clients c
JOIN commandes cmd ON c.id = cmd.client_id
GROUP BY c.id, c.nom, c.email
ORDER BY montant_total DESC;

-- 3. Évolution des ventes par mois
SELECT 
    DATE_FORMAT(date_commande, '%Y-%m') AS mois,
    COUNT(*) AS nb_commandes,
    SUM(total) AS chiffre_affaires
FROM commandes
WHERE statut = 'livré'
GROUP BY mois
ORDER BY mois;

-- 4. Produits en rupture de stock
SELECT nom, stock, prix
FROM produits
WHERE stock < 10
ORDER BY stock;
```
</details>

---

## 🎯 Points Clés

✅ **Bases relationnelles**: Tables, lignes, colonnes  
✅ **SQL**: CREATE, INSERT, SELECT, UPDATE, DELETE  
✅ **Jointures**: INNER, LEFT, RIGHT pour combiner tables  
✅ **Agrégations**: COUNT, AVG, SUM, GROUP BY  
✅ **Normalisation**: Éviter la redondance  
✅ **Python + SQL**: sqlite3, Pandas integration  

---

## ➡️ Prochaine Étape

[Module 8: Bases de Données Avancées →](./module08-advanced-databases.md)

---

*© 2024 - Formation Data Mining Professionnelle*
