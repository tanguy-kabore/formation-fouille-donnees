# Module 20: Machine Learning Avancé

## 🎯 Objectifs
- Ensemble Methods (Random Forest, Gradient Boosting)
- Introduction au Deep Learning
- NLP et Time Series
- Best Practices

---

## 1. Ensemble Methods

### 1.1 Random Forest

```python
from sklearn.ensemble import RandomForestClassifier, RandomForestRegressor

# Classification
rf_clf = RandomForestClassifier(n_estimators=100, max_depth=10, random_state=42)
rf_clf.fit(X_train, y_train)
y_pred = rf_clf.predict(X_test)

# Feature importance
importances = pd.DataFrame({
    'feature': X.columns,
    'importance': rf_clf.feature_importances_
}).sort_values('importance', ascending=False)

# Régression
rf_reg = RandomForestRegressor(n_estimators=100)
rf_reg.fit(X_train, y_train)
predictions = rf_reg.predict(X_test)
```

### 1.2 Gradient Boosting

```python
from sklearn.ensemble import GradientBoostingClassifier
from xgboost import XGBClassifier
from lightgbm import LGBMClassifier

# Gradient Boosting
gb = GradientBoostingClassifier(n_estimators=100, learning_rate=0.1)
gb.fit(X_train, y_train)

# XGBoost
xgb = XGBClassifier(n_estimators=100, learning_rate=0.1, max_depth=5)
xgb.fit(X_train, y_train)

# LightGBM
lgbm = LGBMClassifier(n_estimators=100, learning_rate=0.1)
lgbm.fit(X_train, y_train)

# Comparer performances
models = {'GB': gb, 'XGBoost': xgb, 'LightGBM': lgbm}
for name, model in models.items():
    score = model.score(X_test, y_test)
    print(f"{name}: {score:.4f}")
```

### 1.3 Stacking

```python
from sklearn.ensemble import StackingClassifier
from sklearn.linear_model import LogisticRegression

# Base models
estimators = [
    ('rf', RandomForestClassifier(n_estimators=50)),
    ('gb', GradientBoostingClassifier(n_estimators=50)),
    ('svm', SVC(probability=True))
]

# Stacking
stacking = StackingClassifier(
    estimators=estimators,
    final_estimator=LogisticRegression()
)
stacking.fit(X_train, y_train)
print(f"Stacking Score: {stacking.score(X_test, y_test):.4f}")
```

---

## 2. Deep Learning

### 2.1 Neural Network avec TensorFlow/Keras

```python
import tensorflow as tf
from tensorflow import keras
from keras import layers

# Créer modèle
model = keras.Sequential([
    layers.Dense(64, activation='relu', input_shape=(X_train.shape[1],)),
    layers.Dropout(0.3),
    layers.Dense(32, activation='relu'),
    layers.Dropout(0.3),
    layers.Dense(1, activation='sigmoid')
])

# Compiler
model.compile(
    optimizer='adam',
    loss='binary_crossentropy',
    metrics=['accuracy']
)

# Entraîner
history = model.fit(
    X_train, y_train,
    epochs=50,
    batch_size=32,
    validation_split=0.2,
    verbose=1
)

# Évaluer
test_loss, test_acc = model.evaluate(X_test, y_test)
print(f"Test Accuracy: {test_acc:.4f}")

# Visualiser
plt.plot(history.history['accuracy'], label='Train')
plt.plot(history.history['val_accuracy'], label='Validation')
plt.xlabel('Epoch')
plt.ylabel('Accuracy')
plt.legend()
plt.savefig('training_history.png')
```

### 2.2 CNN pour Images

```python
from keras.applications import VGG16
from keras.preprocessing.image import ImageDataGenerator

# Transfer Learning
base_model = VGG16(weights='imagenet', include_top=False, input_shape=(224, 224, 3))
base_model.trainable = False

# Ajouter couches
model = keras.Sequential([
    base_model,
    layers.GlobalAveragePooling2D(),
    layers.Dense(256, activation='relu'),
    layers.Dropout(0.5),
    layers.Dense(num_classes, activation='softmax')
])

model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])

# Data Augmentation
datagen = ImageDataGenerator(
    rotation_range=20,
    width_shift_range=0.2,
    height_shift_range=0.2,
    horizontal_flip=True
)

# Entraîner
model.fit(datagen.flow(X_train, y_train, batch_size=32), epochs=20)
```

---

## 3. Natural Language Processing (NLP)

### 3.1 Text Preprocessing

```python
import nltk
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from nltk.stem import PorterStemmer, WordNetLemmatizer

# Télécharger ressources
nltk.download('punkt')
nltk.download('stopwords')
nltk.download('wordnet')

def preprocess_text(text):
    # Minuscules
    text = text.lower()
    
    # Tokenization
    tokens = word_tokenize(text)
    
    # Supprimer stopwords
    stop_words = set(stopwords.words('english'))
    tokens = [t for t in tokens if t not in stop_words]
    
    # Lemmatization
    lemmatizer = WordNetLemmatizer()
    tokens = [lemmatizer.lemmatize(t) for t in tokens]
    
    return ' '.join(tokens)

# Appliquer
df['text_clean'] = df['text'].apply(preprocess_text)
```

### 3.2 Feature Extraction

```python
from sklearn.feature_extraction.text import TfidfVectorizer, CountVectorizer

# TF-IDF
tfidf = TfidfVectorizer(max_features=1000)
X_tfidf = tfidf.fit_transform(df['text_clean'])

# Bag of Words
count_vec = CountVectorizer(max_features=1000)
X_bow = count_vec.fit_transform(df['text_clean'])
```

### 3.3 Sentiment Analysis

```python
from sklearn.naive_bayes import MultinomialNB
from sklearn.pipeline import Pipeline

# Pipeline
pipeline = Pipeline([
    ('tfidf', TfidfVectorizer(max_features=5000)),
    ('clf', MultinomialNB())
])

# Entraîner
pipeline.fit(X_train, y_train)

# Prédire
predictions = pipeline.predict(X_test)

# Évaluer
from sklearn.metrics import classification_report
print(classification_report(y_test, predictions))
```

---

## 4. Time Series Analysis

### 4.1 Préparation

```python
import pandas as pd
from statsmodels.tsa.seasonal import seasonal_decompose

# Charger
df = pd.read_csv('timeseries.csv', parse_dates=['date'], index_col='date')

# Décomposition
decomposition = seasonal_decompose(df['value'], model='multiplicative', period=12)

fig, axes = plt.subplots(4, 1, figsize=(15, 10))
decomposition.observed.plot(ax=axes[0], title='Observed')
decomposition.trend.plot(ax=axes[1], title='Trend')
decomposition.seasonal.plot(ax=axes[2], title='Seasonal')
decomposition.resid.plot(ax=axes[3], title='Residual')
plt.tight_layout()
plt.savefig('decomposition.png')
```

### 4.2 ARIMA

```python
from statsmodels.tsa.arima.model import ARIMA

# Entraîner ARIMA
model = ARIMA(df['value'], order=(5, 1, 0))
model_fit = model.fit()

# Prédire
forecast = model_fit.forecast(steps=30)

# Visualiser
plt.figure(figsize=(15, 6))
plt.plot(df.index, df['value'], label='Actual')
plt.plot(forecast.index, forecast, label='Forecast', color='red')
plt.legend()
plt.title('ARIMA Forecast')
plt.savefig('arima_forecast.png')
```

### 4.3 LSTM pour Time Series

```python
from keras.models import Sequential
from keras.layers import LSTM, Dense

# Préparer séquences
def create_sequences(data, seq_length):
    X, y = [], []
    for i in range(len(data) - seq_length):
        X.append(data[i:i+seq_length])
        y.append(data[i+seq_length])
    return np.array(X), np.array(y)

seq_length = 10
X, y = create_sequences(df['value'].values, seq_length)
X = X.reshape((X.shape[0], X.shape[1], 1))

# Modèle LSTM
model = Sequential([
    LSTM(50, activation='relu', input_shape=(seq_length, 1)),
    Dense(1)
])

model.compile(optimizer='adam', loss='mse')
model.fit(X, y, epochs=50, batch_size=32, validation_split=0.2)

# Prédire
predictions = model.predict(X_test)
```

---

## 5. Best Practices

### 5.1 Pipeline Complet

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
from sklearn.ensemble import RandomForestClassifier

# Pipeline
pipeline = Pipeline([
    ('scaler', StandardScaler()),
    ('pca', PCA(n_components=10)),
    ('classifier', RandomForestClassifier(n_estimators=100))
])

# Cross-validation
from sklearn.model_selection import cross_val_score
scores = cross_val_score(pipeline, X, y, cv=5)
print(f"CV Score: {scores.mean():.4f} (+/- {scores.std():.4f})")

# Entraîner
pipeline.fit(X_train, y_train)

# Sauvegarder
import joblib
joblib.dump(pipeline, 'model_pipeline.pkl')

# Charger
loaded_pipeline = joblib.load('model_pipeline.pkl')
predictions = loaded_pipeline.predict(X_new)
```

### 5.2 Gestion des Déséquilibres de Classes

```python
from imblearn.over_sampling import SMOTE
from imblearn.under_sampling import RandomUnderSampler

# SMOTE (oversampling)
smote = SMOTE(random_state=42)
X_resampled, y_resampled = smote.fit_resample(X_train, y_train)

# Undersampling
rus = RandomUnderSampler(random_state=42)
X_resampled, y_resampled = rus.fit_resample(X_train, y_train)

# Class weights
from sklearn.utils.class_weight import compute_class_weight
weights = compute_class_weight('balanced', classes=np.unique(y_train), y=y_train)
class_weights = dict(enumerate(weights))

model = RandomForestClassifier(class_weight=class_weights)
```

---

## 📝 Projet Final: Système de Prédiction Complet

```python
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split, GridSearchCV
import joblib

class MLPipeline:
    def __init__(self):
        self.scaler = StandardScaler()
        self.model = None
        
    def preprocess(self, df):
        # Nettoyage
        df = df.dropna()
        df = df.drop_duplicates()
        
        # Feature engineering
        # ... votre code ici
        
        return df
    
    def train(self, X, y):
        # Normaliser
        X_scaled = self.scaler.fit_transform(X)
        
        # Split
        X_train, X_test, y_train, y_test = train_test_split(
            X_scaled, y, test_size=0.2, random_state=42
        )
        
        # Grid Search
        param_grid = {
            'n_estimators': [50, 100, 200],
            'max_depth': [5, 10, 15],
            'min_samples_split': [2, 5, 10]
        }
        
        grid = GridSearchCV(
            RandomForestClassifier(random_state=42),
            param_grid,
            cv=5,
            scoring='accuracy',
            n_jobs=-1
        )
        
        grid.fit(X_train, y_train)
        
        self.model = grid.best_estimator_
        
        # Évaluer
        score = self.model.score(X_test, y_test)
        print(f"Test Accuracy: {score:.4f}")
        print(f"Best Params: {grid.best_params_}")
        
        return self.model
    
    def predict(self, X):
        X_scaled = self.scaler.transform(X)
        return self.model.predict(X_scaled)
    
    def save(self, filename='ml_pipeline.pkl'):
        joblib.dump({
            'scaler': self.scaler,
            'model': self.model
        }, filename)
        print(f"✅ Modèle sauvegardé: {filename}")
    
    def load(self, filename='ml_pipeline.pkl'):
        data = joblib.load(filename)
        self.scaler = data['scaler']
        self.model = data['model']
        print(f"✅ Modèle chargé: {filename}")

# Utilisation
pipeline = MLPipeline()
df = pd.read_csv('data.csv')
df_clean = pipeline.preprocess(df)
X = df_clean.drop('target', axis=1)
y = df_clean['target']
pipeline.train(X, y)
pipeline.save()
```

---

## 🎯 Points Clés

✅ **Ensemble Methods**: Combinaison de modèles  
✅ **Deep Learning**: Neural Networks pour problèmes complexes  
✅ **NLP**: Traitement du langage naturel  
✅ **Time Series**: ARIMA, LSTM  
✅ **Pipelines**: Automatisation et reproductibilité  
✅ **Production**: Sauvegarder et déployer modèles  

---

## ➡️ Prochaine Étape

[Module 21: Projet Capstone →](./module21-capstone.md)

---

*© 2025 - Formation Data Mining Professionnelle*
