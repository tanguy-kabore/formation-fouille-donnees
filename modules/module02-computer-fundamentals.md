# Module 2: Fondamentaux de l'Informatique

## 🎯 Objectifs d'Apprentissage

À la fin de ce module, vous serez capable de:
- Comprendre l'architecture de base d'un ordinateur
- Connaître les différents systèmes d'exploitation
- Comprendre comment fonctionnent les réseaux et Internet
- Maîtriser la représentation des données en informatique

---

## 📋 Table des Matières

1. [Architecture des Ordinateurs](#1-architecture-des-ordinateurs)
2. [Systèmes d'Exploitation](#2-systèmes-dexploitation)
3. [Réseaux et Internet](#3-réseaux-et-internet)
4. [Représentation des Données](#4-représentation-des-données)
5. [Algorithmes et Logique](#5-algorithmes-et-logique)

---

## 1. Architecture des Ordinateurs

### 1.1 Les Composants Principaux

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

#### 🧠 CPU (Central Processing Unit)
Le "cerveau" de l'ordinateur

**Fonctions:**
- Exécute les instructions
- Effectue les calculs
- Coordonne les composants

**Caractéristiques:**
- Fréquence (GHz): vitesse d'exécution
- Cœurs: nombre d'unités de traitement
- Cache: mémoire ultra-rapide

**Exemples:**
- Intel Core i7, i9
- AMD Ryzen
- Apple M1, M2

</div>

<div>

#### 💾 Mémoire (RAM)
Mémoire de travail temporaire

**Caractéristiques:**
- Volatile (perdue à l'extinction)
- Très rapide
- Limitée en taille (8GB - 64GB)

**Analogie:**
```
RAM = Bureau de travail
Plus c'est grand, plus vous pouvez
travailler sur plusieurs dossiers
simultanément
```

</div>

</div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

#### 💿 Stockage
Mémoire permanente

**Types:**
1. **HDD** (Hard Disk Drive)
   - Mécanique (plateaux magnétiques)
   - Grande capacité
   - Plus lent, moins cher

2. **SSD** (Solid State Drive)
   - Électronique (flash memory)
   - Très rapide
   - Plus cher, plus fiable

</div>

<div>

#### 🎮 GPU (Graphics Processing Unit)
Processeur graphique

**Utilisations:**
- Affichage graphique
- **Calcul parallèle massif**
- **Deep Learning** ⭐
- Crypto-mining

**Important pour Data Mining:**
- Accélération des calculs
- Training de modèles ML
- Traitement d'images

</div>

</div>

### 1.2 Schéma d'Architecture

```
┌─────────────────────────────────────────────────┐
│                    CPU                          │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐       │
│  │  Core 1  │  │  Core 2  │  │  Core N  │       │
│  └──────────┘  └──────────┘  └──────────┘       │
│       │              │              │           │
│       └──────────────┴──────────────┘           │
│                    Cache                        │
└───────────────────┬─────────────────────────────┘
                    │ BUS SYSTÈME
        ┌───────────┼───────────┬─────────────┐
        │           │           │             │
    ┌───▼───┐   ┌──▼──┐    ┌───▼────┐   ┌────▼────┐
    │  RAM  │   │ GPU │    │Storage │   │ Network │
    │ 16 GB │   │     │    │SSD/HDD │   │  Card   │
    └───────┘   └─────┘    └────────┘   └─────────┘
```

### 1.3 Hiérarchie de la Mémoire

```
┌─────────────────────┐
│  CPU Registers      │  Taille: Bytes      Vitesse: ★★★★★
├─────────────────────┤
│  Cache L1, L2, L3   │  Taille: KB-MB      Vitesse: ★★★★☆
├─────────────────────┤
│  RAM                │  Taille: GB         Vitesse: ★★★☆☆
├─────────────────────┤
│  SSD                │  Taille: GB-TB      Vitesse: ★★☆☆☆
├─────────────────────┤
│  HDD                │  Taille: TB         Vitesse: ★☆☆☆☆
├─────────────────────┤
│  Network Storage    │  Taille: PB         Vitesse: ☆☆☆☆☆
└─────────────────────┘
```

---

## 2. Systèmes d'Exploitation

### 2.1 Qu'est-ce qu'un OS?

Un **Système d'Exploitation** (Operating System) est le logiciel qui gère:
- Les ressources matérielles (CPU, RAM, disque)
- Les applications et processus
- L'interface utilisateur
- La sécurité et les permissions

### 2.2 Les Principaux OS

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

#### 🪟 Windows
**Avantages:**
- Très répandu (bureautique, gaming)
- Compatible avec la plupart des logiciels
- Interface intuitive

**Pour Data Mining:**
- ✅ Python, R, Java
- ✅ Anaconda, Jupyter
- ⚠️ Certains outils Linux nécessitent WSL

**Version recommandée:** Windows 10/11 Pro

</div>

<div>

#### 🐧 Linux
**Avantages:**
- Open source et gratuit
- Très stable et sécurisé
- Préféré pour serveurs et cloud

**Distributions populaires:**
- Ubuntu (débutants)
- CentOS/RedHat (entreprise)
- Debian (serveurs)

**Pour Data Mining:**
- ✅ Excellent support Python/R
- ✅ Outils Big Data natifs
- ✅ Serveurs de production

</div>

</div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

#### 🍎 macOS
**Avantages:**
- Basé sur Unix (comme Linux)
- Excellent pour développeurs
- Interface élégante

**Pour Data Mining:**
- ✅ Terminal puissant
- ✅ Compatibilité Python/R excellente
- ⚠️ Prix élevé

</div>

<div>

#### ☁️ Cloud OS
**Exemples:**
- AWS Linux
- Google Cloud
- Azure

**Avantages:**
- Scalabilité infinie
- Paiement à l'usage
- Pas de maintenance matérielle

</div>

</div>

### 2.3 Interface en Ligne de Commande (CLI)

La **CLI** (Command Line Interface) est essentielle pour le Data Mining.

#### Windows PowerShell / CMD
```powershell
# Naviguer dans les dossiers
cd C:\Users\Documents
dir                    # Lister les fichiers
mkdir nouveau_dossier  # Créer un dossier
del fichier.txt        # Supprimer un fichier
```

#### Linux/Mac Terminal
```bash
# Naviguer
cd /home/user/documents
ls -la                 # Lister détaillé
mkdir new_folder       # Créer dossier
rm file.txt            # Supprimer
```

---

## 3. Réseaux et Internet

### 3.1 Concepts de Base

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

#### 🌐 Adresse IP
Identifiant unique d'un appareil sur le réseau

**IPv4:**
```
192.168.1.1
```
- 4 nombres de 0 à 255
- ≈ 4 milliards d'adresses

**IPv6:**
```
2001:0db8:85a3::8a2e:0370:7334
```
- Format hexadécimal
- Nombre quasi-infini

</div>

<div>

#### 🌍 DNS (Domain Name System)
Traduit les noms en adresses IP

```
google.com → 142.250.185.46
github.com → 140.82.121.4
```

**Analogie:**
DNS = Annuaire téléphonique d'Internet

</div>

</div>

### 3.2 Le Modèle OSI (7 Couches)

```
7. APPLICATION    ← HTTP, FTP, DNS, SMTP
   ├─────────────────────────────────────┐
6. PRÉSENTATION   ← Encryption, Compression
   ├─────────────────────────────────────┤
5. SESSION        ← Établir/maintenir connexions
   ├─────────────────────────────────────┤
4. TRANSPORT      ← TCP, UDP
   ├─────────────────────────────────────┤
3. RÉSEAU         ← IP, Routing
   ├─────────────────────────────────────┤
2. LIAISON        ← MAC, Ethernet
   ├─────────────────────────────────────┤
1. PHYSIQUE       ← Câbles, Ondes radio
   └─────────────────────────────────────┘
```

### 3.3 Protocoles Importants pour Data Mining

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

#### HTTP/HTTPS
```
Protocole du Web

HTTP  → Non sécurisé
HTTPS → Sécurisé (SSL/TLS)

Usage Data Mining:
- Web Scraping
- APIs REST
- Téléchargement de datasets
```

#### FTP/SFTP
```
Transfert de fichiers

FTP  → File Transfer Protocol
SFTP → Secure FTP

Usage:
- Transfert de gros datasets
- Backup de données
```

</div>

<div>

#### SSH
```
Secure Shell
Connexion sécurisée à distance

Usage:
- Accès serveurs cloud
- Exécution de scripts distants
- Tunneling sécurisé
```

#### APIs REST
```
Communication entre applications

Format: JSON, XML
Méthodes: GET, POST, PUT, DELETE

Usage Data Mining:
- Récupération de données
- Twitter API, Google API, etc.
```

</div>

</div>

---

## 4. Représentation des Données

### 4.1 Systèmes de Numération

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

#### Binaire (Base 2)
```
Symboles: 0, 1
Exemple: 1010 (binaire) = 10 (décimal)

Calcul:
1×2³ + 0×2² + 1×2¹ + 0×2⁰
= 8 + 0 + 2 + 0
= 10
```

#### Décimal (Base 10)
```
Symboles: 0-9
Notre système habituel
Exemple: 42
```

</div>

<div>

#### Hexadécimal (Base 16)
```
Symboles: 0-9, A-F
Exemple: FF (hexa) = 255 (décimal)

Usage:
- Couleurs: #FF5733
- Adresses mémoire
- Hashing
```

#### Octal (Base 8)
```
Symboles: 0-7
Moins utilisé aujourd'hui
Exemple: 77 (octal) = 63 (décimal)
```

</div>

</div>

### 4.2 Unités de Données

```
Bit (b)     = Unité de base (0 ou 1)
Byte (B)    = 8 bits
Kilobyte    = 1,024 bytes (2¹⁰)
Megabyte    = 1,024 KB    (2²⁰)
Gigabyte    = 1,024 MB    (2³⁰)
Terabyte    = 1,024 GB    (2⁴⁰)
Petabyte    = 1,024 TB    (2⁵⁰)
```

**Exemples concrets:**
- **MP3 (3 min):** ~3 MB
- **Photo HD:** ~5 MB
- **Film HD:** ~4 GB
- **Dataset Kaggle typique:** 100 MB - 10 GB
- **Big Data:** TB - PB

### 4.3 Encodage de Caractères

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

#### ASCII
```
American Standard Code
128 caractères (7 bits)

Exemples:
'A' = 65  (0100 0001)
'a' = 97  (0110 0001)
'0' = 48  (0011 0000)
' ' = 32  (0010 0000)
```

</div>

<div>

#### Unicode (UTF-8)
```
Standard universel
Support de toutes les langues

Exemples:
'A' = U+0041
'é' = U+00E9
'中' = U+4E2D
'😊' = U+1F60A

UTF-8: Encodage variable
1 à 4 bytes par caractère
```

</div>

</div>

### 4.4 Types de Fichiers pour Data Mining

| Type | Extension | Usage | Taille Typique |
|------|-----------|-------|----------------|
| **CSV** | .csv | Données tabulaires | KB - GB |
| **JSON** | .json | Données structurées, APIs | KB - MB |
| **XML** | .xml | Données hiérarchiques | KB - MB |
| **Parquet** | .parquet | Big Data, Spark | MB - TB |
| **HDF5** | .h5 | Données scientifiques | GB - TB |
| **Excel** | .xlsx | Tableurs | KB - MB |
| **SQL Dump** | .sql | Bases de données | MB - GB |
| **Images** | .jpg, .png | Vision par ordinateur | KB - MB |
| **Texte** | .txt | Documents | KB - MB |

---

## 5. Algorithmes et Logique

### 5.1 Qu'est-ce qu'un Algorithme?

**Définition:** Suite d'instructions pour résoudre un problème

**Analogie:** Une recette de cuisine
1. Ingrédients (données d'entrée)
2. Étapes (instructions)
3. Plat final (résultat)

### 5.2 Exemple: Algorithme de Recherche

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

<div>

#### Recherche Linéaire
```
Problème: Trouver un nombre dans une liste

Liste: [3, 7, 1, 9, 4]
Chercher: 9

Algorithme:
1. Commencer au début
2. Comparer chaque élément avec 9
3. Si trouvé → retourner position
4. Sinon → continuer

Résultat: Trouvé à la position 3
Complexité: O(n) - Linéaire
```

</div>

<div>

#### Recherche Binaire
```
(Liste doit être triée)

Liste: [1, 3, 4, 7, 9]
Chercher: 7

Algorithme:
1. Prendre l'élément du milieu (4)
2. 7 > 4 → chercher à droite
3. Nouveau milieu (7)
4. 7 == 7 → Trouvé!

Complexité: O(log n) - Logarithmique
BEAUCOUP plus rapide pour grandes listes
```

</div>

</div>

### 5.3 Structures de Contrôle

#### Séquence
```python
# Exécution ligne par ligne
instruction_1
instruction_2
instruction_3
```

#### Condition (If-Else)
```python
if temperature > 30:
    print("Il fait chaud")
elif temperature > 20:
    print("Il fait bon")
else:
    print("Il fait froid")
```

#### Boucles
```python
# Boucle For (nombre d'itérations connu)
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4

# Boucle While (condition)
compteur = 0
while compteur < 5:
    print(compteur)
    compteur += 1
```

---

## 📝 Exercices Pratiques

### Exercice 1: Conversions Numériques

Convertissez les nombres suivants:
1. `10110101` (binaire) → décimal
2. `FF` (hexadécimal) → décimal
3. `42` (décimal) → binaire
4. `255` (décimal) → hexadécimal

<details>
<summary>Solutions</summary>

1. 10110101 (binaire) = 181 (décimal)
   - 1×128 + 0×64 + 1×32 + 1×16 + 0×8 + 1×4 + 0×2 + 1×1 = 181

2. FF (hex) = 255 (décimal)
   - F×16 + F×1 = 15×16 + 15 = 255

3. 42 (décimal) = 101010 (binaire)
   - 42 ÷ 2 = 21 reste 0
   - 21 ÷ 2 = 10 reste 1
   - 10 ÷ 2 = 5 reste 0
   - 5 ÷ 2 = 2 reste 1
   - 2 ÷ 2 = 1 reste 0
   - 1 ÷ 2 = 0 reste 1
   - Lire de bas en haut: 101010

4. 255 (décimal) = FF (hexadécimal)
   - 255 ÷ 16 = 15 (F) reste 15 (F)
</details>

### Exercice 2: Calculs de Taille

1. Un fichier CSV contient 1 million de lignes × 50 colonnes. Chaque cellule contient en moyenne 10 caractères. Estimez la taille du fichier.

2. Vous avez un disque de 500 GB. Combien de photos de 5 MB pouvez-vous stocker?

<details>
<summary>Solutions</summary>

1. Calcul:
   - 1,000,000 lignes × 50 colonnes = 50,000,000 cellules
   - 50,000,000 × 10 caractères = 500,000,000 caractères
   - ≈ 500 MB (en assumant 1 byte par caractère)

2. Calcul:
   - 500 GB = 500,000 MB
   - 500,000 MB ÷ 5 MB = 100,000 photos
</details>

### Exercice 3: Ligne de Commande

Pratiquez les commandes suivantes (Windows PowerShell ou Linux Terminal):

```bash
# 1. Créer une structure de dossiers
mkdir -p datamining/projets/projet1
mkdir -p datamining/datasets
mkdir -p datamining/scripts

# 2. Créer des fichiers
cd datamining/scripts
echo "print('Hello Data Mining')" > hello.py

# 3. Lister et naviguer
ls
cd ..
pwd  # (Get-Location sur Windows)

# 4. Copier et renommer
cp scripts/hello.py scripts/hello_backup.py
mv scripts/hello_backup.py scripts/backup.py
```

### Exercice 4: Algorithme Simple

Écrivez sur papier l'algorithme pour:
1. Trouver le maximum dans une liste de nombres
2. Calculer la moyenne d'une liste de nombres
3. Compter combien de nombres pairs dans une liste

<details>
<summary>Exemple de solution (Maximum)</summary>

```
ALGORITHME: Trouver le maximum

ENTRÉE: liste de nombres
SORTIE: le nombre maximum

1. max ← premier élément de la liste
2. POUR chaque élément dans la liste:
3.     SI élément > max ALORS
4.         max ← élément
5.     FIN SI
6. FIN POUR
7. RETOURNER max
```
</details>

---

## 🎯 Quiz de Compréhension

1. **Quel composant est le "cerveau" de l'ordinateur?**
   - a) RAM
   - b) CPU
   - c) SSD
   - d) GPU

2. **Combien de bits dans un byte?**
   - a) 4
   - b) 8
   - c) 16
   - d) 32

3. **Quel protocole est utilisé pour les pages web sécurisées?**
   - a) HTTP
   - b) FTP
   - c) HTTPS
   - d) SSH

4. **Quelle est la base du système hexadécimal?**
   - a) 2
   - b) 8
   - c) 10
   - d) 16

5. **Quel OS est open source?**
   - a) Windows
   - b) macOS
   - c) Linux
   - d) iOS

<details>
<summary>Réponses</summary>

1. b) CPU
2. b) 8
3. c) HTTPS
4. d) 16
5. c) Linux
</details>

---

## 🎯 Points Clés à Retenir

✅ **Architecture:** CPU, RAM, Stockage, GPU sont les composants clés  
✅ **OS:** Windows, Linux, macOS - chacun a ses avantages  
✅ **Réseaux:** IP, DNS, HTTP/HTTPS essentiels pour le web  
✅ **Données:** Binaire est la base, Unicode pour les caractères  
✅ **Algorithmes:** Séquences d'instructions pour résoudre des problèmes  
✅ **CLI:** Interface en ligne de commande cruciale pour Data Mining  

---

## 📚 Ressources Supplémentaires

### Livres
- "Code: The Hidden Language of Computer Hardware and Software" - Charles Petzold
- "Computer Networks" - Andrew Tanenbaum

### Vidéos
- Crash Course Computer Science (YouTube)
- Khan Academy: Computers and Internet

### Pratique
- [Codecademy: Command Line](https://www.codecademy.com/learn/learn-the-command-line)
- [Over The Wire: Bandit](https://overthewire.org/wargames/bandit/) (pratique Linux)

---

## ➡️ Prochaine Étape

[Module 3: Python - Les Bases →](./module03-python-basics.md)

Commencez votre voyage en programmation avec Python, le langage le plus populaire pour le Data Mining!

---

*© 2025 - Formation Data Mining Professionnelle*
