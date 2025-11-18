# Module 16: Data Mining - Exploration et Visualisation

## 🎯 Objectifs
- Analyser statistiquement les données
- Créer des visualisations avancées
- Détecter des patterns
- Analyser les corrélations

---

## 1. Analyse Statistique Descriptive

```python
import pandas as pd
import numpy as np

# Charger données
df = pd.read_csv('data.csv')

# Vue d'ensemble
print(df.info())
print(df.describe())
print(df.head())

# Statistiques par colonne
for col in df.select_dtypes(include=[np.number]).columns:
    print(f"\n=== {col} ===")
    print(f"Moyenne: {df[col].mean():.2f}")
    print(f"Médiane: {df[col].median():.2f}")
    print(f"Écart-type: {df[col].std():.2f}")
    print(f"Min: {df[col].min()}, Max: {df[col].max()}")
    print(f"Q1: {df[col].quantile(0.25):.2f}")
    print(f"Q3: {df[col].quantile(0.75):.2f}")

# Distribution des valeurs
print(df['category'].value_counts())
print(df['category'].value_counts(normalize=True) * 100)
```

---

## 2. Visualisations Avancées

### 2.1 Distribution et Box Plots

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Configuration
sns.set_style("whitegrid")
plt.rcParams['figure.figsize'] = (15, 10)

# Distribution
fig, axes = plt.subplots(2, 2)

# Histogram
axes[0, 0].hist(df['age'], bins=30, edgecolor='black')
axes[0, 0].set_title('Distribution de l\'âge')
axes[0, 0].set_xlabel('Âge')
axes[0, 0].set_ylabel('Fréquence')

# Box plot
sns.boxplot(data=df, x='category', y='value', ax=axes[0, 1])
axes[0, 1].set_title('Valeur par Catégorie')
axes[0, 1].tick_params(axis='x', rotation=45)

# Violin plot
sns.violinplot(data=df, x='category', y='value', ax=axes[1, 0])
axes[1, 0].set_title('Distribution par Catégorie')

# KDE plot
df['value'].plot(kind='kde', ax=axes[1, 1])
axes[1, 1].set_title('Densité de Probabilité')

plt.tight_layout()
plt.savefig('distributions.png', dpi=300)
plt.show()
```

### 2.2 Corrélations

```python
# Matrice de corrélation
correlation = df.select_dtypes(include=[np.number]).corr()

# Heatmap
plt.figure(figsize=(12, 10))
sns.heatmap(correlation, 
            annot=True, 
            fmt='.2f',
            cmap='coolwarm',
            center=0,
            square=True,
            linewidths=1)
plt.title('Matrice de Corrélation', fontsize=16)
plt.tight_layout()
plt.savefig('correlation_matrix.png', dpi=300)
plt.show()

# Pairplot
sns.pairplot(df, hue='target', diag_kind='kde')
plt.savefig('pairplot.png', dpi=300)
```

### 2.3 Time Series

```python
# Préparer données temporelles
df['date'] = pd.to_datetime(df['date'])
df.set_index('date', inplace=True)

# Line plot
plt.figure(figsize=(15, 6))
df['value'].plot()
plt.title('Évolution Temporelle')
plt.xlabel('Date')
plt.ylabel('Valeur')
plt.grid(True)
plt.savefig('timeseries.png', dpi=300)

# Moyennes mobiles
df['MA_7'] = df['value'].rolling(window=7).mean()
df['MA_30'] = df['value'].rolling(window=30).mean()

plt.figure(figsize=(15, 6))
plt.plot(df.index, df['value'], label='Valeur', alpha=0.5)
plt.plot(df.index, df['MA_7'], label='MA 7 jours', linewidth=2)
plt.plot(df.index, df['MA_30'], label='MA 30 jours', linewidth=2)
plt.legend()
plt.title('Tendances avec Moyennes Mobiles')
plt.savefig('trends.png', dpi=300)
```

---

## 3. Détection d'Outliers

```python
import matplotlib.pyplot as plt

# Visualiser outliers
fig, axes = plt.subplots(1, 3, figsize=(18, 5))

# Box plot
axes[0].boxplot(df['value'])
axes[0].set_title('Box Plot - Outliers')

# Scatter plot avec threshold
Q1 = df['value'].quantile(0.25)
Q3 = df['value'].quantile(0.75)
IQR = Q3 - Q1
lower = Q1 - 1.5 * IQR
upper = Q3 + 1.5 * IQR

colors = ['red' if (x < lower or x > upper) else 'blue' 
          for x in df['value']]
axes[1].scatter(range(len(df)), df['value'], c=colors, alpha=0.5)
axes[1].axhline(y=lower, color='r', linestyle='--', label='Lower bound')
axes[1].axhline(y=upper, color='r', linestyle='--', label='Upper bound')
axes[1].set_title('Scatter - Outliers en Rouge')
axes[1].legend()

# Histogram avec outliers marqués
axes[2].hist(df['value'], bins=50, alpha=0.7)
axes[2].axvline(x=lower, color='r', linestyle='--')
axes[2].axvline(x=upper, color='r', linestyle='--')
axes[2].set_title('Distribution avec Limites')

plt.tight_layout()
plt.savefig('outliers_analysis.png', dpi=300)
```

---

## 4. Analyse Multivariée

```python
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler

# Préparer données
X = df.select_dtypes(include=[np.number])
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# PCA
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X_scaled)

# Visualiser PCA
plt.figure(figsize=(10, 8))
scatter = plt.scatter(X_pca[:, 0], X_pca[:, 1], 
                     c=df['target'], 
                     cmap='viridis',
                     alpha=0.6)
plt.xlabel(f'PC1 ({pca.explained_variance_ratio_[0]:.2%})')
plt.ylabel(f'PC2 ({pca.explained_variance_ratio_[1]:.2%})')
plt.title('PCA - Visualisation 2D')
plt.colorbar(scatter, label='Target')
plt.savefig('pca_analysis.png', dpi=300)

# Variance expliquée
plt.figure(figsize=(10, 6))
plt.bar(range(1, len(pca.explained_variance_ratio_) + 1),
        pca.explained_variance_ratio_)
plt.xlabel('Composante Principale')
plt.ylabel('Variance Expliquée')
plt.title('Variance Expliquée par Composante')
plt.savefig('pca_variance.png', dpi=300)
```

---

## 5. Analyse Géographique

```python
import folium
from folium import plugins

# Créer carte
m = folium.Map(location=[48.8566, 2.3522], zoom_start=6)

# Ajouter points
for idx, row in df.iterrows():
    folium.Marker(
        location=[row['latitude'], row['longitude']],
        popup=f"{row['name']}: {row['value']}",
        icon=folium.Icon(color='blue')
    ).add_to(m)

# Heatmap
heat_data = [[row['latitude'], row['longitude'], row['value']] 
             for idx, row in df.iterrows()]
plugins.HeatMap(heat_data).add_to(m)

m.save('map.html')
```

---

## 📝 Projet: Rapport d'Analyse Complet

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from datetime import datetime

class DataExplorer:
    def __init__(self, df):
        self.df = df
        self.report = []
        
    def generate_summary(self):
        """Générer résumé statistique"""
        summary = {
            'total_rows': len(self.df),
            'total_columns': len(self.df.columns),
            'memory_usage': self.df.memory_usage(deep=True).sum() / 1024**2,
            'missing_values': self.df.isnull().sum().sum(),
            'duplicates': self.df.duplicated().sum()
        }
        self.report.append(("Summary", summary))
        return summary
    
    def analyze_numerical(self):
        """Analyser colonnes numériques"""
        num_cols = self.df.select_dtypes(include=[np.number]).columns
        
        stats = {}
        for col in num_cols:
            stats[col] = {
                'mean': self.df[col].mean(),
                'median': self.df[col].median(),
                'std': self.df[col].std(),
                'min': self.df[col].min(),
                'max': self.df[col].max(),
                'skewness': self.df[col].skew(),
                'kurtosis': self.df[col].kurtosis()
            }
        
        self.report.append(("Numerical Analysis", stats))
        return stats
    
    def analyze_categorical(self):
        """Analyser colonnes catégorielles"""
        cat_cols = self.df.select_dtypes(include=['object']).columns
        
        stats = {}
        for col in cat_cols:
            stats[col] = {
                'unique_values': self.df[col].nunique(),
                'top_values': self.df[col].value_counts().head(5).to_dict(),
                'missing': self.df[col].isnull().sum()
            }
        
        self.report.append(("Categorical Analysis", stats))
        return stats
    
    def detect_outliers(self):
        """Détecter outliers"""
        num_cols = self.df.select_dtypes(include=[np.number]).columns
        
        outliers = {}
        for col in num_cols:
            Q1 = self.df[col].quantile(0.25)
            Q3 = self.df[col].quantile(0.75)
            IQR = Q3 - Q1
            lower = Q1 - 1.5 * IQR
            upper = Q3 + 1.5 * IQR
            
            outlier_count = ((self.df[col] < lower) | 
                           (self.df[col] > upper)).sum()
            outliers[col] = {
                'count': outlier_count,
                'percentage': outlier_count / len(self.df) * 100
            }
        
        self.report.append(("Outliers", outliers))
        return outliers
    
    def create_visualizations(self):
        """Créer visualisations"""
        num_cols = self.df.select_dtypes(include=[np.number]).columns
        
        # Distribution plots
        fig, axes = plt.subplots(len(num_cols), 2, 
                                figsize=(15, 5*len(num_cols)))
        
        for idx, col in enumerate(num_cols):
            # Histogram
            axes[idx, 0].hist(self.df[col].dropna(), bins=30, 
                             edgecolor='black')
            axes[idx, 0].set_title(f'Distribution - {col}')
            axes[idx, 0].set_xlabel(col)
            
            # Box plot
            axes[idx, 1].boxplot(self.df[col].dropna())
            axes[idx, 1].set_title(f'Box Plot - {col}')
            axes[idx, 1].set_ylabel(col)
        
        plt.tight_layout()
        plt.savefig('data_distributions.png', dpi=300)
        plt.close()
        
        # Correlation heatmap
        if len(num_cols) > 1:
            plt.figure(figsize=(12, 10))
            sns.heatmap(self.df[num_cols].corr(), 
                       annot=True, fmt='.2f', cmap='coolwarm')
            plt.title('Correlation Matrix')
            plt.tight_layout()
            plt.savefig('correlation_heatmap.png', dpi=300)
            plt.close()
    
    def export_report(self, filename='data_exploration_report.txt'):
        """Exporter rapport"""
        with open(filename, 'w', encoding='utf-8') as f:
            f.write("="*60 + "\n")
            f.write("RAPPORT D'EXPLORATION DE DONNÉES\n")
            f.write(f"Généré le: {datetime.now()}\n")
            f.write("="*60 + "\n\n")
            
            for section, content in self.report:
                f.write(f"\n{'='*60}\n")
                f.write(f"{section}\n")
                f.write(f"{'='*60}\n")
                f.write(str(content))
                f.write("\n\n")
        
        print(f"✅ Rapport exporté: {filename}")
    
    def run_full_analysis(self):
        """Analyse complète"""
        print("1. Résumé général...")
        self.generate_summary()
        
        print("2. Analyse numérique...")
        self.analyze_numerical()
        
        print("3. Analyse catégorielle...")
        self.analyze_categorical()
        
        print("4. Détection outliers...")
        self.detect_outliers()
        
        print("5. Visualisations...")
        self.create_visualizations()
        
        print("6. Export rapport...")
        self.export_report()
        
        print("✅ Analyse terminée!")

# Utilisation
df = pd.read_csv('data.csv')
explorer = DataExplorer(df)
explorer.run_full_analysis()
```

---

## 🎯 Points Clés

✅ **EDA**: Comprendre les données avant modélisation  
✅ **Statistiques**: Mean, median, std, quartiles  
✅ **Visualisations**: Histograms, box plots, heatmaps  
✅ **Corrélations**: Identifier relations entre variables  
✅ **Outliers**: Détecter et traiter anomalies  
✅ **PCA**: Réduction de dimensionnalité  

---

## ➡️ Prochaine Étape

[Module 17: Algorithms de Classification →](./module17-classification.md)

---

*© 2025 - Formation Data Mining Professionnelle*
