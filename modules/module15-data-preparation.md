# Module 15: Data Mining - Préparation des Données

## 🎯 Objectifs
- Collecter et nettoyer les données
- Gérer les valeurs manquantes
- Transformer et normaliser
- Feature Engineering

---

## 1. Collecte de Données

### 1.1 Web Scraping

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

# Scraper simple
url = "https://example.com/data"
response = requests.get(url)
soup = BeautifulSoup(response.content, 'html.parser')

# Extraire données
data = []
for item in soup.find_all('div', class_='item'):
    title = item.find('h2').text
    price = item.find('span', class_='price').text
    data.append({'title': title, 'price': price})

df = pd.DataFrame(data)
```

### 1.2 APIs

```python
import requests
import json

# API REST
response = requests.get('https://api.example.com/data',
                       params={'key': 'YOUR_API_KEY'})
data = response.json()
df = pd.DataFrame(data['results'])
```

---

## 2. Nettoyage des Données

### 2.1 Valeurs Manquantes

```python
import pandas as pd
import numpy as np

# Identifier
print(df.isnull().sum())
print(df.isnull().sum() / len(df) * 100)  # Pourcentage

# Supprimer
df_clean = df.dropna()  # Toutes les lignes avec NA
df_clean = df.dropna(subset=['colonne_importante'])

# Remplir
df['age'].fillna(df['age'].mean(), inplace=True)
df['category'].fillna('Unknown', inplace=True)
df.fillna(method='ffill', inplace=True)  # Forward fill

# Interpolation
df['value'].interpolate(method='linear', inplace=True)
```

### 2.2 Doublons

```python
# Détecter
duplicates = df.duplicated()
print(f"Doublons: {duplicates.sum()}")

# Supprimer
df_unique = df.drop_duplicates()
df_unique = df.drop_duplicates(subset=['email'], keep='first')
```

### 2.3 Outliers

```python
# Méthode IQR
Q1 = df['value'].quantile(0.25)
Q3 = df['value'].quantile(0.75)
IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

df_no_outliers = df[(df['value'] >= lower_bound) & 
                     (df['value'] <= upper_bound)]

# Z-score
from scipy import stats
z_scores = np.abs(stats.zscore(df['value']))
df_no_outliers = df[z_scores < 3]
```

---

## 3. Transformation des Données

### 3.1 Normalisation

```python
from sklearn.preprocessing import MinMaxScaler, StandardScaler

# Min-Max (0 à 1)
scaler = MinMaxScaler()
df[['age', 'salary']] = scaler.fit_transform(df[['age', 'salary']])

# Standardisation (moyenne=0, std=1)
scaler = StandardScaler()
df[['feature1', 'feature2']] = scaler.fit_transform(df[['feature1', 'feature2']])
```

### 3.2 Encodage

```python
from sklearn.preprocessing import LabelEncoder, OneHotEncoder

# Label Encoding
le = LabelEncoder()
df['category_encoded'] = le.fit_transform(df['category'])

# One-Hot Encoding
df_encoded = pd.get_dummies(df, columns=['category', 'region'])

# Ordinal Encoding
education_order = ['High School', 'Bachelor', 'Master', 'PhD']
df['education_encoded'] = df['education'].map({v: i for i, v in enumerate(education_order)})
```

---

## 4. Feature Engineering

### 4.1 Création de Features

```python
# Dates
df['date'] = pd.to_datetime(df['date'])
df['year'] = df['date'].dt.year
df['month'] = df['date'].dt.month
df['day_of_week'] = df['date'].dt.dayofweek
df['is_weekend'] = df['day_of_week'].isin([5, 6]).astype(int)

# Texte
df['text_length'] = df['text'].str.len()
df['word_count'] = df['text'].str.split().str.len()
df['has_keyword'] = df['text'].str.contains('important').astype(int)

# Binning
df['age_group'] = pd.cut(df['age'], 
                          bins=[0, 18, 30, 50, 100],
                          labels=['Enfant', 'Jeune', 'Adult', 'Senior'])

# Interactions
df['interaction'] = df['feature1'] * df['feature2']
df['ratio'] = df['feature1'] / (df['feature2'] + 1)
```

### 4.2 Sélection de Features

```python
from sklearn.feature_selection import SelectKBest, f_classif, RFE
from sklearn.ensemble import RandomForestClassifier

# SelectKBest
selector = SelectKBest(f_classif, k=10)
X_selected = selector.fit_transform(X, y)
selected_features = X.columns[selector.get_support()]

# RFE (Recursive Feature Elimination)
rf = RandomForestClassifier()
rfe = RFE(estimator=rf, n_features_to_select=10)
rfe.fit(X, y)
selected_features = X.columns[rfe.support_]

# Feature Importance
rf.fit(X, y)
importances = pd.DataFrame({
    'feature': X.columns,
    'importance': rf.feature_importances_
}).sort_values('importance', ascending=False)
```

---

## 📝 Projet: Pipeline Complet

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split

class DataPreprocessor:
    def __init__(self):
        self.scaler = StandardScaler()
        
    def load_data(self, filepath):
        """Charger données"""
        return pd.read_csv(filepath)
    
    def clean_data(self, df):
        """Nettoyer"""
        # Supprimer doublons
        df = df.drop_duplicates()
        
        # Gérer valeurs manquantes
        for col in df.select_dtypes(include=[np.number]).columns:
            df[col].fillna(df[col].median(), inplace=True)
        
        for col in df.select_dtypes(include=['object']).columns:
            df[col].fillna('Unknown', inplace=True)
        
        return df
    
    def engineer_features(self, df):
        """Créer features"""
        # Si date existe
        if 'date' in df.columns:
            df['date'] = pd.to_datetime(df['date'])
            df['year'] = df['date'].dt.year
            df['month'] = df['date'].dt.month
        
        return df
    
    def encode_categoricals(self, df):
        """Encoder variables catégorielles"""
        cat_cols = df.select_dtypes(include=['object']).columns
        df_encoded = pd.get_dummies(df, columns=cat_cols, drop_first=True)
        return df_encoded
    
    def scale_features(self, df, target_col):
        """Normaliser features"""
        X = df.drop(target_col, axis=1)
        y = df[target_col]
        
        X_scaled = self.scaler.fit_transform(X)
        X_scaled = pd.DataFrame(X_scaled, columns=X.columns)
        
        return X_scaled, y
    
    def process(self, filepath, target_col):
        """Pipeline complet"""
        print("1. Chargement...")
        df = self.load_data(filepath)
        
        print("2. Nettoyage...")
        df = self.clean_data(df)
        
        print("3. Feature Engineering...")
        df = self.engineer_features(df)
        
        print("4. Encodage...")
        df = self.encode_categoricals(df)
        
        print("5. Normalisation...")
        X, y = self.scale_features(df, target_col)
        
        print("6. Split Train/Test...")
        X_train, X_test, y_train, y_test = train_test_split(
            X, y, test_size=0.2, random_state=42
        )
        
        return X_train, X_test, y_train, y_test

# Utilisation
prep = DataPreprocessor()
X_train, X_test, y_train, y_test = prep.process('data.csv', 'target')
print(f"✅ Données préparées: {X_train.shape}")
```

---

## 🎯 Points Clés

✅ **Nettoyage**: 80% du temps en Data Science  
✅ **Valeurs manquantes**: Supprimer ou imputer  
✅ **Outliers**: IQR ou Z-score  
✅ **Normalisation**: Essentielle pour certains algos  
✅ **Feature Engineering**: Crée de la valeur  
✅ **Encodage**: Convertir catégories en nombres  

---

## ➡️ Prochaine Étape

[Module 16: Exploration et Visualisation →](./module16-data-exploration.md)

---

*© 2025 - Formation Data Mining Professionnelle*
