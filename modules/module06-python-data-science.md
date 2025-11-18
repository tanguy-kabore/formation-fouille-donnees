# Module 6: Python pour la Data Science

## 🎯 Objectifs

- Maîtriser NumPy pour le calcul numérique
- Utiliser Pandas pour la manipulation de données
- Créer des visualisations avec Matplotlib et Seaborn
- Travailler avec Jupyter Notebook

---

## 1. NumPy - Calcul Numérique

### 1.1 Arrays (Tableaux)

```python
import numpy as np

# Création d'arrays
arr1 = np.array([1, 2, 3, 4, 5])
arr2 = np.array([[1, 2, 3], [4, 5, 6]])
zeros = np.zeros((3, 4))
ones = np.ones((2, 3))
arange = np.arange(0, 10, 2)  # [0, 2, 4, 6, 8]
linspace = np.linspace(0, 1, 5)  # 5 valeurs entre 0 et 1

# Propriétés
print(arr2.shape)   # (2, 3)
print(arr2.dtype)   # dtype('int64')
print(arr2.size)    # 6
print(arr2.ndim)    # 2 (dimensions)
```

### 1.2 Opérations

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

```python
# Opérations vectorisées
a = np.array([1, 2, 3, 4])
b = np.array([10, 20, 30, 40])

print(a + b)    # [11, 22, 33, 44]
print(a * 2)    # [2, 4, 6, 8]
print(a ** 2)   # [1, 4, 9, 16]

# Fonctions mathématiques
print(np.sqrt(a))
print(np.exp(a))
print(np.sin(a))
print(np.log(a))
```

</div>

<div>

```python
# Statistiques
data = np.array([1, 2, 3, 4, 5])

print(np.mean(data))    # 3.0
print(np.median(data))  # 3.0
print(np.std(data))     # 1.41...
print(np.min(data))     # 1
print(np.max(data))     # 5
print(np.sum(data))     # 15
```

</div>

</div>

### 1.3 Indexation et Slicing

```python
# Array 2D
arr = np.array([[1, 2, 3, 4],
                [5, 6, 7, 8],
                [9, 10, 11, 12]])

print(arr[0, 0])      # 1
print(arr[1, 2])      # 7
print(arr[:, 0])      # [1, 5, 9] (première colonne)
print(arr[0, :])      # [1, 2, 3, 4] (première ligne)
print(arr[0:2, 1:3])  # [[2, 3], [6, 7]]

# Boolean indexing
data = np.array([1, 2, 3, 4, 5])
mask = data > 3
print(data[mask])  # [4, 5]
```

---

## 2. Pandas - Manipulation de Données

### 2.1 Series et DataFrame

```python
import pandas as pd

# Series (1D)
s = pd.Series([10, 20, 30, 40], index=['a', 'b', 'c', 'd'])
print(s['b'])  # 20

# DataFrame (2D)
data = {
    'nom': ['Alice', 'Bob', 'Charlie', 'David'],
    'age': [25, 30, 35, 28],
    'ville': ['Paris', 'Lyon', 'Paris', 'Marseille'],
    'salaire': [50000, 60000, 55000, 52000]
}

df = pd.DataFrame(data)
print(df)
print(df.head())      # Premières lignes
print(df.info())      # Informations
print(df.describe())  # Statistiques
```

### 2.2 Sélection de Données

```python
# Sélection de colonnes
print(df['nom'])
print(df[['nom', 'age']])

# Sélection de lignes
print(df.iloc[0])       # Première ligne (index)
print(df.loc[0])        # Par label
print(df[df['age'] > 28])  # Filtrage

# Combinaison
parisiens = df[df['ville'] == 'Paris'][['nom', 'salaire']]
```

### 2.3 Opérations Courantes

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

```python
# Ajouter une colonne
df['bonus'] = df['salaire'] * 0.1

# Supprimer
df = df.drop('bonus', axis=1)

# Trier
df_sorted = df.sort_values('age', 
                           ascending=False)

# Grouper
group = df.groupby('ville')['salaire'].mean()
```

</div>

<div>

```python
# Valeurs manquantes
print(df.isnull().sum())
df_clean = df.dropna()  # Supprimer
df_filled = df.fillna(0)  # Remplir

# Renommer
df = df.rename(columns={'nom': 'name'})

# Apply function
df['age_double'] = df['age'].apply(lambda x: x * 2)
```

</div>

</div>

### 2.4 Lecture/Écriture de Fichiers

```python
# CSV
df.to_csv('data.csv', index=False)
df_read = pd.read_csv('data.csv')

# Excel
df.to_excel('data.xlsx', index=False)
df_excel = pd.read_excel('data.xlsx')

# JSON
df.to_json('data.json')
df_json = pd.read_json('data.json')
```

---

## 3. Matplotlib - Visualisation

### 3.1 Graphiques de Base

```python
import matplotlib.pyplot as plt

# Line plot
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

plt.figure(figsize=(10, 6))
plt.plot(x, y, marker='o', linestyle='-', color='blue', label='y = 2x')
plt.xlabel('X')
plt.ylabel('Y')
plt.title('Graphique Linéaire')
plt.legend()
plt.grid(True)
plt.savefig('plot.png', dpi=300, bbox_inches='tight')
plt.show()

# Bar chart
categories = ['A', 'B', 'C', 'D']
values = [25, 40, 30, 55]

plt.bar(categories, values, color='skyblue')
plt.title('Diagramme en Barres')
plt.show()

# Histogram
data = np.random.randn(1000)
plt.hist(data, bins=30, color='green', alpha=0.7)
plt.title('Histogramme')
plt.show()

# Scatter plot
x = np.random.rand(100)
y = np.random.rand(100)
colors = np.random.rand(100)
sizes = 1000 * np.random.rand(100)

plt.scatter(x, y, c=colors, s=sizes, alpha=0.5, cmap='viridis')
plt.colorbar()
plt.title('Nuage de Points')
plt.show()
```

### 3.2 Subplots (Sous-graphiques)

```python
fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# Plot 1
axes[0, 0].plot([1, 2, 3], [1, 4, 9])
axes[0, 0].set_title('Plot 1')

# Plot 2
axes[0, 1].bar(['A', 'B', 'C'], [10, 20, 15])
axes[0, 1].set_title('Plot 2')

# Plot 3
axes[1, 0].hist(np.random.randn(1000), bins=30)
axes[1, 0].set_title('Plot 3')

# Plot 4
axes[1, 1].scatter(np.random.rand(50), np.random.rand(50))
axes[1, 1].set_title('Plot 4')

plt.tight_layout()
plt.show()
```

---

## 4. Seaborn - Visualisation Avancée

```python
import seaborn as sns

# Style
sns.set_style("whitegrid")
sns.set_palette("husl")

# Données exemple
tips = sns.load_dataset("tips")

# Box plot
plt.figure(figsize=(10, 6))
sns.boxplot(x='day', y='total_bill', data=tips)
plt.title('Distribution des factures par jour')
plt.show()

# Violin plot
sns.violinplot(x='day', y='total_bill', hue='sex', data=tips)
plt.show()

# Heatmap (corrélation)
df_numeric = tips.select_dtypes(include=[np.number])
correlation = df_numeric.corr()
sns.heatmap(correlation, annot=True, cmap='coolwarm')
plt.title('Matrice de Corrélation')
plt.show()

# Pair plot
sns.pairplot(tips, hue='sex')
plt.show()
```

---

## 5. Jupyter Notebook

### 5.1 Installation et Démarrage

```bash
# Installation
pip install jupyter

# Lancer
jupyter notebook

# Ou avec JupyterLab (recommandé)
pip install jupyterlab
jupyter lab
```

### 5.2 Raccourcis Clavier

| Raccourci | Action |
|-----------|--------|
| `Shift + Enter` | Exécuter cellule |
| `Ctrl + Enter` | Exécuter sans avancer |
| `A` | Insérer cellule au-dessus |
| `B` | Insérer cellule en-dessous |
| `DD` | Supprimer cellule |
| `M` | Markdown |
| `Y` | Code |

---

## 📝 Projet Pratique: Analyse de Données

### Dataset: Ventes de Produits

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# 1. Créer des données de vente
np.random.seed(42)
n = 1000

data = {
    'date': pd.date_range('2023-01-01', periods=n, freq='D'),
    'produit': np.random.choice(['A', 'B', 'C', 'D'], n),
    'quantite': np.random.randint(1, 100, n),
    'prix': np.random.uniform(10, 200, n),
    'region': np.random.choice(['Nord', 'Sud', 'Est', 'Ouest'], n)
}

df = pd.DataFrame(data)
df['montant'] = df['quantite'] * df['prix']

# 2. Analyse exploratoire
print("=== STATISTIQUES DESCRIPTIVES ===")
print(df.describe())

print("\n=== VENTES PAR PRODUIT ===")
print(df.groupby('produit')['montant'].sum().sort_values(ascending=False))

print("\n=== VENTES PAR RÉGION ===")
print(df.groupby('region')['montant'].sum())

# 3. Visualisations
fig, axes = plt.subplots(2, 2, figsize=(15, 10))

# Ventes par produit
ventes_produit = df.groupby('produit')['montant'].sum()
axes[0, 0].bar(ventes_produit.index, ventes_produit.values)
axes[0, 0].set_title('Ventes Totales par Produit')
axes[0, 0].set_ylabel('Montant (€)')

# Évolution temporelle
df_daily = df.groupby('date')['montant'].sum()
axes[0, 1].plot(df_daily.index, df_daily.values)
axes[0, 1].set_title('Évolution des Ventes')
axes[0, 1].set_ylabel('Montant (€)')

# Distribution des prix
axes[1, 0].hist(df['prix'], bins=30, edgecolor='black')
axes[1, 0].set_title('Distribution des Prix')
axes[1, 0].set_xlabel('Prix (€)')

# Ventes par région
ventes_region = df.groupby('region')['montant'].sum()
axes[1, 1].pie(ventes_region.values, labels=ventes_region.index, 
               autopct='%1.1f%%')
axes[1, 1].set_title('Répartition par Région')

plt.tight_layout()
plt.savefig('analyse_ventes.png', dpi=300)
plt.show()

# 4. Top 10 jours de ventes
print("\n=== TOP 10 JOURS DE VENTES ===")
top_days = df.groupby('date')['montant'].sum().nlargest(10)
print(top_days)

# 5. Exporter les résultats
summary = df.groupby(['produit', 'region']).agg({
    'quantite': 'sum',
    'montant': 'sum'
}).reset_index()

summary.to_csv('rapport_ventes.csv', index=False)
print("\n✅ Rapport exporté: rapport_ventes.csv")
```

---

## 🎯 Points Clés

✅ **NumPy**: Arrays performants, opérations vectorisées  
✅ **Pandas**: DataFrames, manipulation de données tabulaires  
✅ **Matplotlib**: Visualisations de base personnalisables  
✅ **Seaborn**: Visualisations statistiques élégantes  
✅ **Jupyter**: Environnement interactif pour l'analyse  

---

## ➡️ Prochaine Étape

[Module 7: Bases de Données et SQL →](./module07-databases-sql.md)

---

*© 2024 - Formation Data Mining Professionnelle*
