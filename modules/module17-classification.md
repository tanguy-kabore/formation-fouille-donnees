# Module 17: Algorithms de Classification

## 🎯 Objectifs
- Implémenter Decision Trees, KNN, SVM
- Évaluer les modèles de classification
- Optimiser les hyperparamètres

---

## 1. Decision Trees (Arbres de Décision)

```python
from sklearn.tree import DecisionTreeClassifier, plot_tree
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, classification_report
import matplotlib.pyplot as plt

# Préparer données
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# Entraîner
dt = DecisionTreeClassifier(max_depth=5, random_state=42)
dt.fit(X_train, y_train)

# Prédire
y_pred = dt.predict(X_test)

# Évaluer
print(f"Accuracy: {accuracy_score(y_test, y_pred):.3f}")
print(classification_report(y_test, y_pred))

# Visualiser arbre
plt.figure(figsize=(20,10))
plot_tree(dt, filled=True, feature_names=X.columns, class_names=['0','1'])
plt.savefig('decision_tree.png', dpi=300)
```

---

## 2. K-Nearest Neighbors (KNN)

```python
from sklearn.neighbors import KNeighborsClassifier

# Entraîner
knn = KNeighborsClassifier(n_neighbors=5)
knn.fit(X_train, y_train)

# Prédire
y_pred = knn.predict(X_test)

# Trouver meilleur K
scores = []
for k in range(1, 31):
    knn = KNeighborsClassifier(n_neighbors=k)
    knn.fit(X_train, y_train)
    scores.append(knn.score(X_test, y_test))

plt.plot(range(1, 31), scores)
plt.xlabel('K')
plt.ylabel('Accuracy')
plt.title('KNN - Optimal K')
plt.savefig('knn_k_selection.png')
```

---

## 3. Support Vector Machines (SVM)

```python
from sklearn.svm import SVC

# Linear SVM
svm_linear = SVC(kernel='linear')
svm_linear.fit(X_train, y_train)

# RBF kernel
svm_rbf = SVC(kernel='rbf', gamma='scale')
svm_rbf.fit(X_train, y_train)

# Évaluer
print("Linear SVM:", svm_linear.score(X_test, y_test))
print("RBF SVM:", svm_rbf.score(X_test, y_test))
```

---

## 4. Évaluation des Modèles

```python
from sklearn.metrics import confusion_matrix, roc_curve, auc
import seaborn as sns

# Confusion Matrix
cm = confusion_matrix(y_test, y_pred)
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues')
plt.title('Confusion Matrix')
plt.ylabel('True')
plt.xlabel('Predicted')
plt.savefig('confusion_matrix.png')

# ROC Curve
y_proba = dt.predict_proba(X_test)[:, 1]
fpr, tpr, _ = roc_curve(y_test, y_proba)
roc_auc = auc(fpr, tpr)

plt.figure()
plt.plot(fpr, tpr, label=f'AUC = {roc_auc:.2f}')
plt.plot([0, 1], [0, 1], 'k--')
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('ROC Curve')
plt.legend()
plt.savefig('roc_curve.png')
```

---

## 5. Cross-Validation

```python
from sklearn.model_selection import cross_val_score, GridSearchCV

# Cross-validation
scores = cross_val_score(dt, X, y, cv=5)
print(f"CV Scores: {scores}")
print(f"Mean: {scores.mean():.3f} (+/- {scores.std():.3f})")

# Grid Search
param_grid = {
    'max_depth': [3, 5, 7, 10],
    'min_samples_split': [2, 5, 10],
    'min_samples_leaf': [1, 2, 4]
}

grid = GridSearchCV(DecisionTreeClassifier(), param_grid, cv=5)
grid.fit(X_train, y_train)

print("Best params:", grid.best_params_)
print("Best score:", grid.best_score_)
```

---

## 📝 Projet: Prédiction de Churn

```python
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report

# Charger données
df = pd.read_csv('customer_churn.csv')

# Préparer
X = df.drop('churn', axis=1)
y = df['churn']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# Modèle
rf = RandomForestClassifier(n_estimators=100, random_state=42)
rf.fit(X_train, y_train)

# Évaluer
y_pred = rf.predict(X_test)
print(classification_report(y_test, y_pred))

# Feature importance
importances = pd.DataFrame({
    'feature': X.columns,
    'importance': rf.feature_importances_
}).sort_values('importance', ascending=False)

print(importances.head(10))
```

---

## ➡️ Prochaine Étape

[Module 18: Clustering →](./module18-clustering.md)

---

*© 2025 - Formation Data Mining Professionnelle*
