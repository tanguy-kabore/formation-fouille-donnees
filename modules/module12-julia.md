# Module 12: Julia - Computing Scientifique

## 🎯 Objectifs
- Comprendre Julia et ses avantages
- Maîtriser la syntaxe de base
- Utiliser Julia pour le Data Science
- Parallélisation et performance

---

## 1. Introduction à Julia

### Pourquoi Julia?

✅ **Performance**: Vitesse proche du C  
✅ **Syntaxe simple**: Comme Python  
✅ **Computing scientifique**: Mathématiques avancées  
✅ **Parallélisme natif**: Multi-core facilement  

### Installation
```bash
# Télécharger depuis julialang.org
# Vérifier
julia --version
```

---

## 2. Syntaxe de Base

```julia
# Variables
x = 10
y = 20.5
nom = "Julia"
actif = true

# Types
typeof(x)  # Int64
typeof(y)  # Float64

# Opérations
a = 5 + 3
b = 10 - 4
c = 2 * 3
d = 10 / 3
e = 10 ÷ 3  # Division entière
f = 2^10   # Puissance

# Affichage
println("Hello, World!")
println("x = $x")  # Interpolation

# Tableaux
arr = [1, 2, 3, 4, 5]
matrix = [1 2 3; 4 5 6; 7 8 9]

# Indexation (commence à 1!)
premier = arr[1]
dernier = arr[end]
slice = arr[2:4]

# Boucles
for i in 1:5
    println(i)
end

for element in arr
    println(element)
end

# Conditions
if x > 5
    println("Grand")
elseif x > 0
    println("Petit")
else
    println("Négatif")
end

# Fonctions
function carre(x)
    return x^2
end

# Forme courte
cube(x) = x^3

# Appel
println(carre(5))
println(cube(3))
```

---

## 3. Tableaux et Matrices

```julia
# Créer tableaux
zeros(5)           # [0, 0, 0, 0, 0]
ones(3, 3)         # Matrice 3x3 de 1
rand(5)            # 5 nombres aléatoires
randn(5)           # Distribution normale

# Opérations vectorielles
a = [1, 2, 3, 4, 5]
b = [2, 4, 6, 8, 10]

c = a .+ b         # Addition élément par élément
d = a .* b         # Multiplication
e = a .^ 2         # Élever au carré

# Broadcasting
f = a .+ 10        # Ajoute 10 à chaque élément

# Fonctions d'agrégation
sum(a)
mean(a)
std(a)
maximum(a)
minimum(a)

# Compréhension de liste
carres = [x^2 for x in 1:10]
pairs = [x for x in 1:20 if x % 2 == 0]
```

---

## 4. DataFrames en Julia

```julia
using DataFrames, CSV, Statistics

# Créer DataFrame
df = DataFrame(
    nom = ["Alice", "Bob", "Charlie"],
    age = [25, 30, 35],
    salaire = [50000, 60000, 55000]
)

# Afficher
println(df)

# Lire CSV
df = CSV.read("data.csv", DataFrame)

# Sélection
df[1, :]          # Première ligne
df[:, :nom]       # Colonne nom
df[df.age .> 28, :]  # Filtrer

# Ajouter colonne
df.bonus = df.salaire .* 0.1

# Statistiques
describe(df)
mean(df.age)
std(df.salaire)

# Grouper
using DataFramesMeta
@chain df begin
    @groupby(:ville)
    @combine(:moyenne_age = mean(:age))
end
```

---

## 5. Visualisation

```julia
using Plots

# Line plot
x = 1:10
y = x.^2
plot(x, y, label="y = x²", xlabel="X", ylabel="Y", title="Graphique")

# Scatter
scatter(rand(100), rand(100), alpha=0.5)

# Histogram
histogram(randn(1000), bins=30, label="Distribution Normale")

# Plusieurs graphiques
p1 = plot(1:10, rand(10), label="Plot 1")
p2 = scatter(rand(20), rand(20), label="Plot 2")
plot(p1, p2, layout=(1,2))
```

---

## 6. Performance et Parallélisme

### Parallélisation

```julia
using Distributed

# Ajouter processeurs
addprocs(4)

# Fonction parallèle
@everywhere function slow_function(x)
    sleep(1)
    return x^2
end

# Map parallèle
results = pmap(slow_function, 1:10)

# Boucle parallèle
@distributed for i in 1:100
    # Traitement
end

# Réduction parallèle
total = @distributed (+) for i in 1:1000
    i^2
end
```

### Optimisation

```julia
# Macro @time
@time begin
    result = sum([i^2 for i in 1:1_000_000])
end

# Type stability
function fast_sum(arr::Vector{Int64})
    total = 0
    for x in arr
        total += x
    end
    return total
end

# Inline
@inline function petit_calcul(x)
    return x * 2 + 1
end
```

---

## 📝 Projet: Analyse de Données Scientifiques

```julia
using DataFrames, CSV, Statistics, Plots

# 1. Charger données
df = CSV.read("scientific_data.csv", DataFrame)

# 2. Nettoyage
dropmissing!(df)

# 3. Statistiques descriptives
println("=== STATISTIQUES ===")
println("Moyenne: ", mean(df.valeur))
println("Médiane: ", median(df.valeur))
println("Écart-type: ", std(df.valeur))

# 4. Analyse par groupe
grouped = groupby(df, :categorie)
stats = combine(grouped, 
    :valeur => mean => :moyenne,
    :valeur => std => :ecart_type,
    nrow => :count
)
println(stats)

# 5. Visualisation
p1 = histogram(df.valeur, bins=30, title="Distribution")
p2 = boxplot(df.categorie, df.valeur, title="Par Catégorie")
plot(p1, p2, layout=(2,1), size=(800, 600))
savefig("analysis.png")

# 6. Test statistique
using HypothesisTests

# T-test
group1 = df[df.categorie .== "A", :valeur]
group2 = df[df.categorie .== "B", :valeur]
result = EqualVarianceTTest(group1, group2)
println("p-value: ", pvalue(result))
```

---

## 🎯 Points Clés

✅ **Julia**: Performance + Simplicité  
✅ **Syntaxe mathématique**: Naturelle pour scientifiques  
✅ **JIT compilation**: Rapide à l'exécution  
✅ **Parallélisme**: Multi-core facile  
✅ **Écosystème Data Science**: DataFrames, Plots, Stats  

---

## ➡️ Prochaine Étape

[Module 13: Cloud Computing - Fondamentaux →](./module13-cloud-fundamentals.md)

---

*© 2025 - Formation Data Mining Professionnelle*
