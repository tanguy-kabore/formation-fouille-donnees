# Module 19: Association Rules et Pattern Mining

## 🎯 Objectifs
- Comprendre les règles d'association
- Implémenter Apriori et FP-Growth
- Market Basket Analysis

---

## 1. Concepts

**Support**: Fréquence d'un itemset  
**Confidence**: Probabilité conditionnelle  
**Lift**: Force de l'association

```
Support(A) = Count(A) / Total Transactions
Confidence(A→B) = Support(A,B) / Support(A)
Lift(A→B) = Confidence(A→B) / Support(B)
```

---

## 2. Apriori Algorithm

```python
from mlxtend.frequent_patterns import apriori, association_rules
import pandas as pd

# Préparer données (one-hot encoding)
basket = pd.get_dummies(df['items'].str.split(', ').apply(pd.Series).stack()).groupby(level=0).sum()

# Apriori
frequent_itemsets = apriori(basket, min_support=0.01, use_colnames=True)
print(frequent_itemsets)

# Règles
rules = association_rules(frequent_itemsets, metric="lift", min_threshold=1.0)
rules = rules.sort_values('lift', ascending=False)

print(rules[['antecedents', 'consequents', 'support', 'confidence', 'lift']].head(10))
```

---

## 3. Market Basket Analysis

```python
# Transactions
transactions = [
    ['pain', 'lait', 'beurre'],
    ['pain', 'lait'],
    ['pain', 'beurre'],
    ['lait', 'beurre', 'fromage'],
    ['pain', 'lait', 'fromage']
]

# Convertir en format approprié
from mlxtend.preprocessing import TransactionEncoder

te = TransactionEncoder()
te_array = te.fit(transactions).transform(transactions)
df = pd.DataFrame(te_array, columns=te.columns_)

# Apriori
frequent_itemsets = apriori(df, min_support=0.4, use_colnames=True)
rules = association_rules(frequent_itemsets, metric="confidence", min_threshold=0.6)

print(rules)
```

---

## 📝 Projet: Recommandation Produits

```python
# Analyser transactions e-commerce
transactions = pd.read_csv('transactions.csv')

# Créer matrice items
basket = transactions.groupby(['order_id', 'product'])['product'].count().unstack().fillna(0)
basket = basket.applymap(lambda x: 1 if x > 0 else 0)

# Apriori
frequent_items = apriori(basket, min_support=0.02, use_colnames=True)
rules = association_rules(frequent_items, metric="lift", min_threshold=1.2)

# Top recommandations
print("=== TOP RECOMMANDATIONS ===")
print(rules.sort_values('lift', ascending=False).head(10))
```

---

## ➡️ Prochaine Étape

[Module 20: Machine Learning Avancé →](./module20-advanced-ml.md)

---

*© 2025 - Formation Data Mining Professionnelle*
