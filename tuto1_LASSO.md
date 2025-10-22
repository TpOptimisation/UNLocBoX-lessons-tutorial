# Tutoriel 1 : Régression LASSO avec UNLocBoX

##  AUTEURS
- **YOUBI BOPDA FREDY LOIC** – 20U2811  
- **DIBAKTO DEBAMI MANICH JORDAN** – 000000  
- **DAWAI HOSSEA** – 21T2436  
- **TCHANKUIMEU MEUGHELA AMAELLE** – 21T2548  

---

## Objectif
Ce tutoriel illustre la mise en œuvre d’une **régression LASSO** en utilisant la boîte à outils **UNLocBoX** sous **MATLAB**.  
L’objectif est de reconstruire un **signal parcimonieux** (sparse) à partir d’un petit nombre de mesures bruitées.

---

## Étapes du tutoriel

### 1️ Chargement des librairies

```matlab
clear; close all; clc;

% Ajout du chemin vers UNLocBoX (adapter selon votre installation)
addpath(genpath('~/unlocbox'));  
init_unlocbox();
````

---

### 2️ Génération des données

On crée un signal parcimonieux `x_true` comportant 100 coefficients dont seulement 10 non nuls.
Les mesures `b` sont obtenues via la matrice `A` et un bruit gaussien.

```matlab
n = 100;      % Nombre de variables
m = 40;       % Nombre d'observations
A = randn(m, n);    % Matrice de mesure
x_true = zeros(n, 1);
x_true(1:10) = randn(10,1);     % Signal parcimonieux
b = A * x_true + 0.01 * randn(m,1); % Ajout de bruit
```

---

### 3️ Définition des fonctions convexes

Le problème LASSO s’écrit :

[
\min_x \frac{1}{2} |A x - b|_2^2 + \lambda |x|_1
]

On définit alors les deux fonctions convexes :

* ( f_1(x) = \frac{1}{2} |A x - b|_2^2 )
* ( f_2(x) = \lambda |x|_1 )

```matlab
lambda = 0.05;

% Fonction f1(x) = 1/2 * ||A*x - b||_2^2
f1.prox = @(x, T) (A' * ((eye(m) + T*(A*A')) \ (b + (A*x)*T)));
f1.eval = @(x) 0.5 * norm(A*x - b)^2;

% Fonction f2(x) = lambda * ||x||_1
f2.prox = @(x, T) prox_l1(x, lambda*T);
f2.eval = @(x) lambda * norm(x,1);
```

---

### 4 Paramètres du solveur

On configure la méthode de résolution avec **FISTA** (Fast Iterative Shrinkage-Thresholding Algorithm).

```matlab
param.verbose = 1;
param.maxit = 200;
param.tol = 1e-4;
param.gamma = 1;   % Pas du gradient
param.method = 'FISTA';
```

---

### 5 Résolution du problème

On résout le problème d’optimisation à l’aide de la fonction `solvep` de UNLocBoX :

```matlab
[x_sol, info] = solvep(randn(n,1), {f1, f2}, param);
```

---

### 6️ Visualisation des résultats

On compare le **signal original** et le **signal reconstruit**.

```matlab
figure;
stem(x_true, 'b', 'DisplayName', 'Signal original'); hold on;
stem(x_sol, 'r', 'DisplayName', 'Signal reconstruit');
legend;
xlabel('Index'); ylabel('Amplitude');
title('Résolution du problème LASSO avec UNLocBoX');
grid on;
```

**Résultat attendu :**
Le signal reconstruit (en rouge) suit étroitement le signal original (en bleu), montrant que la régularisation LASSO identifie correctement les coefficients non nuls.

---

### 7 Nettoyage

On libère les ressources utilisées par UNLocBoX.

```matlab
close_unlocbox();
```

---

##  Conclusion

Ce tutoriel montre comment :

* Générer un signal parcimonieux simulé,
* Formuler un problème LASSO,
* Utiliser **UNLocBoX** pour le résoudre efficacement,
* Visualiser la qualité de la reconstruction.

Le LASSO est un outil essentiel pour les problèmes de **sélection de variables** et de **reconstruction parcimonieuse**, largement utilisé en traitement du signal, apprentissage automatique et statistiques.

---
