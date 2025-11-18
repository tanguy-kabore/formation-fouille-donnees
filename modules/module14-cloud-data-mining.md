# Module 14: Cloud pour le Data Mining

## 🎯 Objectifs
- Utiliser les services ML cloud
- Déployer des modèles en production
- Optimiser les coûts

---

## 1. AWS SageMaker

```python
import boto3
import sagemaker
from sagemaker import get_execution_role

# Setup
role = get_execution_role()
sess = sagemaker.Session()
bucket = sess.default_bucket()

# Entraîner modèle
from sagemaker.sklearn.estimator import SKLearn

sklearn_estimator = SKLearn(
    entry_point='train.py',
    role=role,
    instance_type='ml.m5.xlarge',
    framework_version='0.23-1',
    py_version='py3'
)

sklearn_estimator.fit({'train': 's3://bucket/train/'})

# Déployer
predictor = sklearn_estimator.deploy(
    initial_instance_count=1,
    instance_type='ml.t2.medium'
)

# Prédire
predictions = predictor.predict(data)
```

---

## 2. Azure ML

```python
from azureml.core import Workspace, Experiment, Dataset
from azureml.train.sklearn import SKLearn

# Connexion
ws = Workspace.from_config()

# Créer expérience
experiment = Experiment(workspace=ws, name='my-experiment')

# Configuration
estimator = SKLearn(
    source_directory='./src',
    entry_script='train.py',
    compute_target='cpu-cluster'
)

# Lancer
run = experiment.submit(estimator)
run.wait_for_completion(show_output=True)
```

---

## 3. Google Cloud AI Platform

```python
from google.cloud import aiplatform

# Entraîner modèle
aiplatform.init(project='my-project')

job = aiplatform.CustomTrainingJob(
    display_name='training-job',
    script_path='train.py',
    container_uri='gcr.io/cloud-aiplatform/training/sklearn-cpu.0-23'
)

job.run(replica_count=1, machine_type='n1-standard-4')

# Déployer
endpoint = aiplatform.Endpoint.create(display_name='my-endpoint')
model.deploy(endpoint=endpoint, machine_type='n1-standard-2')
```

---

## ➡️ Prochaine Étape

[Module 15: Data Mining - Préparation →](./module15-data-preparation.md)

---

*© 2024 - Formation Data Mining Professionnelle*
