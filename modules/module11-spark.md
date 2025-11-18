# Module 11: Apache Spark - Big Data Processing

## 🎯 Objectifs
- Comprendre l'architecture Spark
- Manipuler RDDs et DataFrames
- Utiliser Spark SQL
- Introduction à MLlib

---

## 1. Introduction à Spark

### Pourquoi Spark?

✅ **100x plus rapide** que Hadoop MapReduce  
✅ **In-memory computing**  
✅ **APIs Python, Scala, Java, R**  
✅ **Écosystème complet**: SQL, ML, Streaming, Graph  

---

## 2. PySpark - Démarrage

### Installation
```bash
pip install pyspark
```

### Premier Programme

```python
from pyspark.sql import SparkSession

# Créer SparkSession
spark = SparkSession.builder \
    .appName("MonApp") \
    .getOrCreate()

# Créer un DataFrame
data = [("Alice", 20), ("Bob", 22), ("Charlie", 21)]
df = spark.createDataFrame(data, ["nom", "age"])

# Afficher
df.show()
"""
+-------+---+
|   nom |age|
+-------+---+
|  Alice| 20|
|    Bob| 22|
|Charlie| 21|
+-------+---+
"""

# Opérations
df.filter(df.age > 20).show()
df.select("nom").show()
df.groupBy("age").count().show()

# Fermer
spark.stop()
```

---

## 3. RDDs (Resilient Distributed Datasets)

```python
from pyspark import SparkContext

sc = SparkContext("local", "RDD Demo")

# Créer RDD
rdd = sc.parallelize([1, 2, 3, 4, 5])

# Transformations (lazy)
rdd_carre = rdd.map(lambda x: x ** 2)
rdd_filtre = rdd.filter(lambda x: x % 2 == 0)

# Actions (exécution)
print(rdd_carre.collect())  # [1, 4, 9, 16, 25]
print(rdd.reduce(lambda a, b: a + b))  # 15

# Depuis fichier
lines = sc.textFile("data.txt")
word_counts = lines.flatMap(lambda line: line.split(" ")) \
                   .map(lambda word: (word, 1)) \
                   .reduceByKey(lambda a, b: a + b)

word_counts.saveAsTextFile("output")
```

---

## 4. DataFrames et Spark SQL

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, avg, sum, count

spark = SparkSession.builder.appName("SQL Demo").getOrCreate()

# Lire CSV
df = spark.read.csv("data.csv", header=True, inferSchema=True)

# Afficher schéma
df.printSchema()

# Opérations DataFrame
df.select("nom", "age").show()
df.filter(col("age") > 20).show()
df.groupBy("ville").agg(avg("age").alias("age_moyen")).show()

# SQL natif
df.createOrReplaceTempView("etudiants")
result = spark.sql("""
    SELECT ville, AVG(age) as age_moyen
    FROM etudiants
    GROUP BY ville
""")
result.show()

# Jointures
df1 = spark.createDataFrame([(1, "Alice"), (2, "Bob")], ["id", "nom"])
df2 = spark.createDataFrame([(1, 85), (2, 78)], ["id", "note"])

df_join = df1.join(df2, "id")
df_join.show()
```

---

## 5. MLlib - Machine Learning

```python
from pyspark.ml.feature import VectorAssembler
from pyspark.ml.regression import LinearRegression
from pyspark.ml.classification import LogisticRegression
from pyspark.ml.evaluation import RegressionEvaluator

# Préparer données
data = spark.read.csv("data.csv", header=True, inferSchema=True)

# Feature engineering
assembler = VectorAssembler(
    inputCols=["feature1", "feature2", "feature3"],
    outputCol="features"
)
data_transformed = assembler.transform(data)

# Split train/test
train, test = data_transformed.randomSplit([0.8, 0.2], seed=42)

# Régression linéaire
lr = LinearRegression(featuresCol="features", labelCol="target")
model = lr.fit(train)

# Prédictions
predictions = model.transform(test)
predictions.select("features", "target", "prediction").show()

# Évaluation
evaluator = RegressionEvaluator(labelCol="target")
rmse = evaluator.evaluate(predictions)
print(f"RMSE: {rmse}")
```

---

## 6. Streaming avec Spark

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import window, col

spark = SparkSession.builder.appName("Streaming").getOrCreate()

# Lire stream depuis un socket
lines = spark.readStream \
    .format("socket") \
    .option("host", "localhost") \
    .option("port", 9999) \
    .load()

# Traitement
words = lines.select(
    explode(split(col("value"), " ")).alias("word")
)

word_counts = words.groupBy("word").count()

# Écrire stream
query = word_counts.writeStream \
    .outputMode("complete") \
    .format("console") \
    .start()

query.awaitTermination()
```

---

## 📝 Projet: Analyse de Logs

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import *

spark = SparkSession.builder.appName("LogAnalysis").getOrCreate()

# Lire logs
logs = spark.read.text("access.log")

# Parser logs (format Apache)
log_pattern = r'(\S+) - - \[(.*?)\] "(\S+) (\S+) (\S+)" (\d+) (\d+)'

logs_parsed = logs.select(
    regexp_extract(col("value"), log_pattern, 1).alias("ip"),
    regexp_extract(col("value"), log_pattern, 2).alias("timestamp"),
    regexp_extract(col("value"), log_pattern, 3).alias("method"),
    regexp_extract(col("value"), log_pattern, 4).alias("endpoint"),
    regexp_extract(col("value"), log_pattern, 6).alias("status").cast("int"),
    regexp_extract(col("value"), log_pattern, 7).alias("size").cast("int")
)

# Analyses
print("=== TOP 10 IPs ===")
logs_parsed.groupBy("ip").count() \
    .orderBy(col("count").desc()) \
    .show(10)

print("=== Codes de statut ===")
logs_parsed.groupBy("status").count() \
    .orderBy("status") \
    .show()

print("=== Endpoints populaires ===")
logs_parsed.groupBy("endpoint").count() \
    .orderBy(col("count").desc()) \
    .show(10)

# Taille moyenne des réponses par status
logs_parsed.groupBy("status") \
    .agg(avg("size").alias("avg_size")) \
    .show()
```

---

## 🎯 Points Clés

✅ **Spark**: Framework Big Data ultra-rapide  
✅ **RDDs**: Collections distribuées immuables  
✅ **DataFrames**: API structurée, optimisée  
✅ **Spark SQL**: Requêtes SQL sur Big Data  
✅ **MLlib**: Machine Learning distribué  
✅ **Streaming**: Traitement en temps réel  

---

## ➡️ Prochaine Étape

[Module 12: Julia →](./module12-julia.md)

---

*© 2024 - Formation Data Mining Professionnelle*
