# Module 13: Cloud Computing - Fondamentaux

## 🎯 Objectifs
- Comprendre les concepts du Cloud
- Découvrir AWS, Azure, GCP
- Utiliser les services de stockage
- Déployer des applications

---

## 1. Introduction au Cloud

### 1.1 Qu'est-ce que le Cloud?

**Définition**: Accès à des ressources informatiques (serveurs, stockage, bases de données) via Internet, à la demande.

### 1.2 Modèles de Service

<div style="display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 20px;">

<div>

**IaaS**  
Infrastructure as a Service

- Serveurs virtuels
- Stockage
- Réseau

Exemples:
- AWS EC2
- Azure VMs
- Google Compute Engine

</div>

<div>

**PaaS**  
Platform as a Service

- Environnement de dev
- Middleware
- Outils de dev

Exemples:
- Heroku
- Google App Engine
- Azure App Service

</div>

<div>

**SaaS**  
Software as a Service

- Applications complètes
- Accès web
- Maintenance incluse

Exemples:
- Gmail
- Office 365
- Salesforce

</div>

</div>

### 1.3 Avantages du Cloud

✅ **Scalabilité**: Ajuster les ressources à la demande  
✅ **Coût**: Paiement à l'usage (pay-as-you-go)  
✅ **Disponibilité**: 99.9% uptime  
✅ **Sécurité**: Infrastructure sécurisée  
✅ **Global**: Déploiement mondial  

---

## 2. Amazon Web Services (AWS)

### 2.1 Services Principaux

**Compute:**
- EC2: Serveurs virtuels
- Lambda: Serverless functions
- ECS: Containers

**Storage:**
- S3: Object storage
- EBS: Block storage
- Glacier: Archivage

**Database:**
- RDS: Bases relationnelles
- DynamoDB: NoSQL
- Redshift: Data warehouse

### 2.2 AWS S3 avec Python

```python
import boto3

# Connexion
s3 = boto3.client('s3',
    aws_access_key_id='YOUR_KEY',
    aws_secret_access_key='YOUR_SECRET'
)

# Créer un bucket
s3.create_bucket(Bucket='mon-bucket-datamining')

# Upload fichier
s3.upload_file('local_file.csv', 'mon-bucket', 'data/file.csv')

# Download fichier
s3.download_file('mon-bucket', 'data/file.csv', 'downloaded.csv')

# Lister fichiers
response = s3.list_objects_v2(Bucket='mon-bucket')
for obj in response['Contents']:
    print(obj['Key'])

# Supprimer fichier
s3.delete_object(Bucket='mon-bucket', Key='data/file.csv')
```

### 2.3 AWS EC2 - Instance

```python
import boto3

ec2 = boto3.resource('ec2')

# Lancer instance
instances = ec2.create_instances(
    ImageId='ami-0c55b159cbfafe1f0',  # Ubuntu
    MinCount=1,
    MaxCount=1,
    InstanceType='t2.micro',
    KeyName='my-key-pair'
)

instance = instances[0]
print(f'Instance ID: {instance.id}')

# Attendre que l'instance soit en cours
instance.wait_until_running()

# Obtenir IP publique
instance.reload()
print(f'Public IP: {instance.public_ip_address}')

# Arrêter instance
instance.stop()

# Terminer instance
instance.terminate()
```

---

## 3. Microsoft Azure

### 3.1 Services Clés

- Virtual Machines
- Blob Storage
- Azure SQL Database
- Azure ML Studio

### 3.2 Azure Blob Storage

```python
from azure.storage.blob import BlobServiceClient

# Connexion
connection_string = "YOUR_CONNECTION_STRING"
blob_service = BlobServiceClient.from_connection_string(connection_string)

# Créer container
container_client = blob_service.create_container("data-container")

# Upload blob
blob_client = blob_service.get_blob_client(
    container="data-container",
    blob="data.csv"
)

with open("local_data.csv", "rb") as data:
    blob_client.upload_blob(data)

# Download blob
with open("downloaded_data.csv", "wb") as download_file:
    download_file.write(blob_client.download_blob().readall())

# Lister blobs
container_client = blob_service.get_container_client("data-container")
for blob in container_client.list_blobs():
    print(blob.name)
```

---

## 4. Google Cloud Platform (GCP)

### 4.1 Services Principaux

- Compute Engine
- Cloud Storage
- BigQuery
- Cloud ML Engine

### 4.2 Google Cloud Storage

```python
from google.cloud import storage

# Connexion
client = storage.Client()

# Créer bucket
bucket = client.create_bucket("mon-bucket-gcp")

# Upload fichier
blob = bucket.blob("data/file.csv")
blob.upload_from_filename("local_file.csv")

# Download
blob.download_to_filename("downloaded.csv")

# Lister fichiers
blobs = client.list_blobs("mon-bucket-gcp")
for blob in blobs:
    print(blob.name)

# Supprimer
blob.delete()
```

### 4.3 BigQuery

```python
from google.cloud import bigquery

# Client
client = bigquery.Client()

# Requête SQL
query = """
    SELECT name, COUNT(*) as count
    FROM `project.dataset.table`
    GROUP BY name
    ORDER BY count DESC
    LIMIT 10
"""

query_job = client.query(query)
results = query_job.result()

for row in results:
    print(f"{row.name}: {row.count}")

# Charger données depuis DataFrame
import pandas as pd

df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie'],
    'age': [25, 30, 35]
})

table_id = "project.dataset.table"
job = client.load_table_from_dataframe(df, table_id)
job.result()
```

---

## 5. Comparaison des Plateformes

| Feature | AWS | Azure | GCP |
|---------|-----|-------|-----|
| **Market Share** | ~33% | ~22% | ~10% |
| **Compute** | EC2 | VMs | Compute Engine |
| **Storage** | S3 | Blob | Cloud Storage |
| **Database** | RDS | SQL DB | Cloud SQL |
| **Data Warehouse** | Redshift | Synapse | BigQuery |
| **ML** | SageMaker | ML Studio | AI Platform |
| **Pricing** | Complex | Medium | Simple |
| **Best For** | Général | Enterprise | Big Data/ML |

---

## 6. Conteneurs et Kubernetes

### 6.1 Docker Basics

```dockerfile
# Dockerfile
FROM python:3.10

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

```bash
# Build
docker build -t mon-app .

# Run
docker run -p 5000:5000 mon-app

# Push vers registry
docker tag mon-app username/mon-app:v1
docker push username/mon-app:v1
```

### 6.2 Kubernetes Basics

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mon-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mon-app
  template:
    metadata:
      labels:
        app: mon-app
    spec:
      containers:
      - name: mon-app
        image: username/mon-app:v1
        ports:
        - containerPort: 5000
```

```bash
# Déployer
kubectl apply -f deployment.yaml

# Scaler
kubectl scale deployment mon-app --replicas=5

# Vérifier
kubectl get pods
kubectl get services
```

---

## 📝 Projet: Pipeline de Données Cloud

```python
# pipeline_cloud.py
import boto3
import pandas as pd
from io import StringIO

class CloudDataPipeline:
    def __init__(self):
        self.s3 = boto3.client('s3')
        self.bucket = 'datamining-pipeline'
    
    def extract_from_source(self, source_url):
        """Extraire données depuis source"""
        df = pd.read_csv(source_url)
        return df
    
    def transform(self, df):
        """Transformer les données"""
        # Nettoyage
        df = df.dropna()
        
        # Feature engineering
        df['total'] = df['quantity'] * df['price']
        
        # Agrégation
        summary = df.groupby('category').agg({
            'total': 'sum',
            'quantity': 'sum'
        }).reset_index()
        
        return df, summary
    
    def load_to_s3(self, df, key):
        """Charger vers S3"""
        csv_buffer = StringIO()
        df.to_csv(csv_buffer, index=False)
        
        self.s3.put_object(
            Bucket=self.bucket,
            Key=key,
            Body=csv_buffer.getvalue()
        )
        print(f"✅ Uploaded to s3://{self.bucket}/{key}")
    
    def run(self):
        """Exécuter le pipeline"""
        print("1. Extraction...")
        df = self.extract_from_source("https://example.com/data.csv")
        
        print("2. Transformation...")
        df_clean, df_summary = self.transform(df)
        
        print("3. Chargement...")
        self.load_to_s3(df_clean, "processed/data_clean.csv")
        self.load_to_s3(df_summary, "analytics/summary.csv")
        
        print("✅ Pipeline terminé!")

# Exécution
if __name__ == "__main__":
    pipeline = CloudDataPipeline()
    pipeline.run()
```

---

## 🎯 Points Clés

✅ **Cloud**: IaaS, PaaS, SaaS  
✅ **AWS**: Leader du marché, services complets  
✅ **Azure**: Excellent pour entreprises Microsoft  
✅ **GCP**: Fort en Big Data et ML  
✅ **Containers**: Docker pour portabilité  
✅ **K8s**: Orchestration à grande échelle  

---

## ➡️ Prochaine Étape

[Module 14: Cloud pour le Data Mining →](./module14-cloud-data-mining.md)

---

*© 2024 - Formation Data Mining Professionnelle*
