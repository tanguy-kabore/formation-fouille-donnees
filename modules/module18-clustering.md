# Module 18: Clustering et Segmentation

## 🎯 Objectifs
- Implémenter K-Means, DBSCAN
- Évaluer les clusters
- Applications pratiques

---

## 1. K-Means Clustering

```python
from sklearn.cluster import KMeans
import matplotlib.pyplot as plt

# K-Means
kmeans = KMeans(n_clusters=3, random_state=42)
clusters = kmeans.fit_predict(X)

# Visualiser
plt.scatter(X[:, 0], X[:, 1], c=clusters, cmap='viridis')
plt.scatter(kmeans.cluster_centers_[:, 0], 
           kmeans.cluster_centers_[:, 1],
           marker='X', s=200, c='red', label='Centroids')
plt.title('K-Means Clustering')
plt.legend()
plt.savefig('kmeans.png')

# Méthode du coude (Elbow)
inertias = []
for k in range(1, 11):
    km = KMeans(n_clusters=k, random_state=42)
    km.fit(X)
    inertias.append(km.inertia_)

plt.plot(range(1, 11), inertias, marker='o')
plt.xlabel('Number of Clusters')
plt.ylabel('Inertia')
plt.title('Elbow Method')
plt.savefig('elbow.png')
```

---

## 2. Hierarchical Clustering

```python
from scipy.cluster.hierarchy import dendrogram, linkage

# Linkage
linkage_matrix = linkage(X, method='ward')

# Dendrogram
plt.figure(figsize=(12, 8))
dendrogram(linkage_matrix)
plt.title('Hierarchical Clustering Dendrogram')
plt.savefig('dendrogram.png')
```

---

## 3. DBSCAN

```python
from sklearn.cluster import DBSCAN

# DBSCAN
dbscan = DBSCAN(eps=0.5, min_samples=5)
clusters = dbscan.fit_predict(X)

# Visualiser
plt.scatter(X[:, 0], X[:, 1], c=clusters, cmap='viridis')
plt.title('DBSCAN Clustering')
plt.savefig('dbscan.png')

# Nombre de clusters
n_clusters = len(set(clusters)) - (1 if -1 in clusters else 0)
n_noise = list(clusters).count(-1)
print(f'Clusters: {n_clusters}, Noise points: {n_noise}')
```

---

## 4. Évaluation

```python
from sklearn.metrics import silhouette_score, davies_bouldin_score

# Silhouette Score
sil_score = silhouette_score(X, clusters)
print(f'Silhouette Score: {sil_score:.3f}')

# Davies-Bouldin Index
db_score = davies_bouldin_score(X, clusters)
print(f'Davies-Bouldin: {db_score:.3f}')
```

---

## 📝 Projet: Segmentation Clients

```python
import pandas as pd
from sklearn.preprocessing import StandardScaler

# Charger données
df = pd.read_csv('customers.csv')

# Préparer
features = ['age', 'income', 'spending_score']
X = df[features]

# Normaliser
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# K-Means
kmeans = KMeans(n_clusters=4, random_state=42)
df['segment'] = kmeans.fit_predict(X_scaled)

# Analyser segments
for i in range(4):
    segment = df[df['segment'] == i]
    print(f"\n=== Segment {i} ===")
    print(f"Taille: {len(segment)}")
    print(segment[features].describe())
```

---

## ➡️ Prochaine Étape

[Module 19: Association Rules →](./module19-association-rules.md)

---

*© 2025 - Formation Data Mining Professionnelle*
