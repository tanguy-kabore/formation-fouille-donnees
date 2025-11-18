# Module 21: Projet Capstone

## 🎯 Objectif
Réaliser un projet complet de Data Mining de A à Z

---

## 1. Cahier des Charges

### 1.1 Choix du Projet

**Options suggérées:**
1. Prédiction de churn clients
2. Système de recommandation
3. Détection de fraude
4. Analyse de sentiments
5. Prédiction de ventes
6. Classification d'images médicales

### 1.2 Livrables

✅ Code source complet  
✅ Documentation technique  
✅ Rapport d'analyse  
✅ Présentation (slides)  
✅ Dashboard interactif  

---

## 2. Méthodologie

### Phase 1: Définition (Semaine 1)
- Choisir le problème
- Définir les objectifs
- Identifier les données
- Planifier les étapes

### Phase 2: Collecte et Préparation (Semaine 2)
- Collecter les données
- Nettoyage
- Feature engineering
- EDA complète

### Phase 3: Modélisation (Semaine 3)
- Sélectionner algorithmes
- Entraîner modèles
- Optimiser hyperparamètres
- Évaluer performances

### Phase 4: Déploiement (Semaine 4)
- Créer API
- Développer dashboard
- Documentation
- Présentation

---

## 3. Structure du Projet

```
capstone-project/
├── data/
│   ├── raw/
│   ├── processed/
│   └── external/
├── notebooks/
│   ├── 01_exploration.ipynb
│   ├── 02_preprocessing.ipynb
│   ├── 03_modeling.ipynb
│   └── 04_evaluation.ipynb
├── src/
│   ├── data_processing.py
│   ├── feature_engineering.py
│   ├── models.py
│   └── utils.py
├── app/
│   ├── api.py
│   └── dashboard.py
├── tests/
├── models/
│   └── trained_models/
├── reports/
│   ├── figures/
│   └── final_report.pdf
├── requirements.txt
├── README.md
└── presentation.pptx
```

---

## 4. Exemple: Prédiction de Churn

### 4.1 Data Processing

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

class ChurnPredictor:
    def __init__(self):
        self.scaler = StandardScaler()
        self.model = None
        
    def load_data(self, filepath):
        df = pd.read_csv(filepath)
        return df
    
    def preprocess(self, df):
        # Nettoyage
        df = df.dropna()
        
        # Features
        df['tenure_years'] = df['tenure'] / 12
        df['avg_monthly_charges'] = df['total_charges'] / df['tenure']
        
        # Encoder
        df = pd.get_dummies(df, drop_first=True)
        
        return df
    
    def train(self, X, y):
        from sklearn.ensemble import RandomForestClassifier
        
        X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
        X_train_scaled = self.scaler.fit_transform(X_train)
        
        self.model = RandomForestClassifier(n_estimators=100)
        self.model.fit(X_train_scaled, y_train)
        
        score = self.model.score(self.scaler.transform(X_test), y_test)
        return score
```

### 4.2 API Flask

```python
from flask import Flask, request, jsonify
import joblib
import pandas as pd

app = Flask(__name__)
model = joblib.load('models/churn_model.pkl')

@app.route('/predict', methods=['POST'])
def predict():
    data = request.json
    df = pd.DataFrame([data])
    prediction = model.predict(df)
    probability = model.predict_proba(df)[0][1]
    
    return jsonify({
        'churn': int(prediction[0]),
        'probability': float(probability)
    })

if __name__ == '__main__':
    app.run(debug=True)
```

### 4.3 Dashboard Streamlit

```python
import streamlit as st
import pandas as pd
import plotly.express as px

st.title("📊 Churn Prediction Dashboard")

# Upload data
uploaded_file = st.file_uploader("Upload CSV", type=['csv'])

if uploaded_file:
    df = pd.read_csv(uploaded_file)
    
    # Métriques
    col1, col2, col3 = st.columns(3)
    col1.metric("Total Clients", len(df))
    col2.metric("Churn Rate", f"{df['churn'].mean()*100:.1f}%")
    col3.metric("Avg Tenure", f"{df['tenure'].mean():.1f} mois")
    
    # Visualisations
    fig = px.histogram(df, x='tenure', color='churn', title='Distribution Tenure')
    st.plotly_chart(fig)
    
    # Prédiction
    st.header("Faire une Prédiction")
    tenure = st.slider("Tenure (mois)", 0, 100, 12)
    monthly = st.number_input("Charges mensuelles", 0.0, 200.0, 50.0)
    
    if st.button("Prédire"):
        # Appeler API
        result = make_prediction({'tenure': tenure, 'monthly': monthly})
        st.success(f"Probabilité de churn: {result['probability']:.2%}")
```

---

## 5. Évaluation du Projet

### Critères (100 points)

**Code et Technique (40 points)**
- Qualité du code: 10 pts
- Méthodologie: 10 pts
- Performances du modèle: 15 pts
- Reproductibilité: 5 pts

**Documentation (25 points)**
- README complet: 5 pts
- Code commenté: 10 pts
- Rapport technique: 10 pts

**Analyse et Insights (20 points)**
- EDA approfondie: 10 pts
- Interprétation résultats: 10 pts

**Présentation (15 points)**
- Clarté: 5 pts
- Visualisations: 5 pts
- Communication: 5 pts

---

## 6. Conseils

✅ Commencer simple, itérer  
✅ Documenter au fur et à mesure  
✅ Versionner avec Git  
✅ Tester le code  
✅ Demander feedback régulièrement  
✅ Gérer le temps efficacement  

---

## 🏆 Félicitations!

Vous avez complété la formation Data Mining!

Vous êtes maintenant capable de:
- ✅ Programmer en Python, Java, Julia/Spark
- ✅ Gérer des bases de données
- ✅ Utiliser le cloud
- ✅ Appliquer des techniques de Data Mining
- ✅ Construire et déployer des modèles ML

**Prochaines étapes:**
1. Construire votre portfolio
2. Contribuer à des projets open source
3. Participer à des compétitions Kaggle
4. Chercher des opportunités professionnelles

**Bonne chance dans votre carrière en Data Science! 🚀**

---

*© 2024 - Formation Data Mining Professionnelle*
