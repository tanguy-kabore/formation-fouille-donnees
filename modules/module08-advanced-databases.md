# Module 8: Bases de Données Avancées

## 🎯 Objectifs

- Optimiser les performances avec les index
- Comprendre les transactions et ACID
- Découvrir les bases de données NoSQL
- Intégrer les BD avec Python

---

## 1. Index et Optimisation

### 1.1 Les Index

```sql
-- Créer un index
CREATE INDEX idx_nom ON etudiants(nom);
CREATE INDEX idx_email ON etudiants(email);

-- Index composé
CREATE INDEX idx_ville_age ON etudiants(ville, age);

-- Index unique
CREATE UNIQUE INDEX idx_unique_email ON etudiants(email);

-- Supprimer un index
DROP INDEX idx_nom ON etudiants;

-- Analyser les requêtes
EXPLAIN SELECT * FROM etudiants WHERE nom = 'Dupont';
```

**Quand utiliser les index:**
- ✅ Colonnes fréquemment dans WHERE
- ✅ Colonnes de jointure
- ✅ Colonnes de tri (ORDER BY)
- ❌ Tables très petites
- ❌ Colonnes rarement utilisées

---

## 2. Transactions et ACID

### 2.1 Les Transactions

```sql
-- Démarrer une transaction
START TRANSACTION;

-- Opérations
UPDATE comptes SET solde = solde - 100 WHERE id = 1;
UPDATE comptes SET solde = solde + 100 WHERE id = 2;

-- Valider
COMMIT;

-- Ou annuler
ROLLBACK;
```

### 2.2 ACID

**A - Atomicity (Atomicité)**
- Tout ou rien: succès complet ou échec complet

**C - Consistency (Cohérence)**
- Les données respectent toujours les contraintes

**I - Isolation**
- Les transactions sont isolées les unes des autres

**D - Durability (Durabilité)**
- Une fois validée, la transaction est permanente

---

## 3. Bases de Données NoSQL

### 3.1 MongoDB (Document Database)

```python
from pymongo import MongoClient

# Connexion
client = MongoClient('localhost', 27017)
db = client['universite']
collection = db['etudiants']

# Insérer document
etudiant = {
    "nom": "Alice",
    "age": 20,
    "email": "alice@uni.fr",
    "cours": ["Python", "Data Mining", "ML"],
    "adresse": {
        "rue": "123 Main St",
        "ville": "Paris"
    }
}
collection.insert_one(etudiant)

# Insérer plusieurs
etudiants = [
    {"nom": "Bob", "age": 22, "email": "bob@uni.fr"},
    {"nom": "Charlie", "age": 21, "email": "charlie@uni.fr"}
]
collection.insert_many(etudiants)

# Requêtes
# Trouver tous
for etud in collection.find():
    print(etud)

# Avec filtre
result = collection.find({"age": {"$gt": 20}})

# Projection (colonnes spécifiques)
result = collection.find({}, {"nom": 1, "email": 1, "_id": 0})

# Mettre à jour
collection.update_one(
    {"nom": "Alice"},
    {"$set": {"age": 21}}
)

# Supprimer
collection.delete_one({"nom": "Bob"})

# Agrégations
pipeline = [
    {"$group": {
        "_id": "$ville",
        "count": {"$sum": 1},
        "moyenne_age": {"$avg": "$age"}
    }}
]
results = collection.aggregate(pipeline)
```

### 3.2 Redis (Key-Value Store)

```python
import redis

# Connexion
r = redis.Redis(host='localhost', port=6379, db=0)

# Set/Get
r.set('nom', 'Alice')
print(r.get('nom'))  # b'Alice'

# Hash (dictionnaire)
r.hset('user:1', mapping={
    'nom': 'Alice',
    'age': '20',
    'email': 'alice@uni.fr'
})
print(r.hgetall('user:1'))

# Liste
r.rpush('fruits', 'pomme', 'banane', 'orange')
print(r.lrange('fruits', 0, -1))

# Set (ensemble)
r.sadd('tags', 'python', 'data', 'ML')
print(r.smembers('tags'))

# Expiration (cache)
r.setex('session:abc123', 3600, 'user_data')  # Expire dans 1h

# Incrémenter (compteurs)
r.incr('page_views')
```

---

## 4. Python et Bases de Données Avancées

### 4.1 SQLAlchemy (ORM)

```python
from sqlalchemy import create_engine, Column, Integer, String, Float
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# Configuration
engine = create_engine('sqlite:///universite.db')
Base = declarative_base()

# Définir modèle
class Etudiant(Base):
    __tablename__ = 'etudiants'
    
    id = Column(Integer, primary_key=True)
    nom = Column(String(100), nullable=False)
    age = Column(Integer)
    email = Column(String(255), unique=True)

# Créer tables
Base.metadata.create_all(engine)

# Session
Session = sessionmaker(bind=engine)
session = Session()

# Insérer
alice = Etudiant(nom="Alice", age=20, email="alice@uni.fr")
session.add(alice)
session.commit()

# Requêtes
# Tous
etudiants = session.query(Etudiant).all()

# Avec filtre
results = session.query(Etudiant).filter(Etudiant.age > 20).all()

# Mettre à jour
etudiant = session.query(Etudiant).filter_by(nom="Alice").first()
etudiant.age = 21
session.commit()

# Supprimer
session.delete(etudiant)
session.commit()
```

### 4.2 Connection Pooling

```python
from sqlalchemy import create_engine
from sqlalchemy.pool import QueuePool

# Avec pool de connexions
engine = create_engine(
    'postgresql://user:password@localhost/db',
    poolclass=QueuePool,
    pool_size=10,
    max_overflow=20
)
```

---

## 📝 Projet: Système de Cache avec Redis

```python
import redis
import json
import time
from functools import wraps

class CacheManager:
    def __init__(self, host='localhost', port=6379):
        self.redis = redis.Redis(host=host, port=port, decode_responses=True)
    
    def get(self, key):
        """Récupérer une valeur du cache"""
        value = self.redis.get(key)
        if value:
            return json.loads(value)
        return None
    
    def set(self, key, value, expire=3600):
        """Stocker une valeur dans le cache"""
        self.redis.setex(key, expire, json.dumps(value))
    
    def delete(self, key):
        """Supprimer une clé du cache"""
        self.redis.delete(key)
    
    def cache_result(self, expire=3600):
        """Décorateur pour cacher les résultats de fonction"""
        def decorator(func):
            @wraps(func)
            def wrapper(*args, **kwargs):
                # Créer clé de cache
                cache_key = f"{func.__name__}:{str(args)}:{str(kwargs)}"
                
                # Vérifier cache
                cached = self.get(cache_key)
                if cached is not None:
                    print(f"Cache hit for {cache_key}")
                    return cached
                
                # Calculer et cacher
                result = func(*args, **kwargs)
                self.set(cache_key, result, expire)
                print(f"Cache miss for {cache_key}")
                return result
            return wrapper
        return decorator

# Utilisation
cache = CacheManager()

@cache.cache_result(expire=300)
def expensive_query(user_id):
    """Simulation d'une requête coûteuse"""
    print("Exécution de la requête...")
    time.sleep(2)  # Simule délai
    return {"user_id": user_id, "data": "Important data"}

# Test
result1 = expensive_query(123)  # Cache miss, lent
result2 = expensive_query(123)  # Cache hit, rapide!
```

---

## 🎯 Points Clés

✅ **Index**: Accélèrent les requêtes mais ralentissent les insertions  
✅ **Transactions**: ACID garantit l'intégrité  
✅ **NoSQL**: Flexible, scalable pour Big Data  
✅ **MongoDB**: Documents JSON  
✅ **Redis**: Cache ultra-rapide  
✅ **SQLAlchemy**: ORM Python puissant  

---

## ➡️ Prochaine Étape

[Module 9: Java - Fondamentaux →](./module09-java-fundamentals.md)

---

*© 2025 - Formation Data Mining Professionnelle*
