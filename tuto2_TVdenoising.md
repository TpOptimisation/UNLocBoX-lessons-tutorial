
---

````markdown
# TUTORIEL 2 : Débruitage d'image par variation totale (TV)

## AUTEURS
- YOUBI BOPDA FREDY LOIC – 20U2811  
- DIBAKTO DEBAMI MANICH JORDAN – 000000  
- DAWAI HOSSEA – 21T2436  
- TCHANKUIMEU MEUGHELA AMAELLE – 21T2548  

---

## 1. Chargement des librairies

```matlab
clear; close all; clc;

addpath(genpath('~/unlocbox'));
init_unlocbox();
````

---

## 2. Chargement ou génération d'une image

On utilise ici l’image de test **phantom**, souvent utilisée pour les simulations en imagerie médicale.
Un bruit gaussien est ajouté pour simuler une image dégradée.

```matlab
n = 128;
y = phantom(n);          % Image test (sinogramme)
y = y + 0.05 * randn(size(y)); % Ajout de bruit
```

---

## 3. Définition des fonctions convexes

L’objectif est de résoudre le problème suivant :

[
\min_x \frac{1}{2} |x - y|_2^2 + \lambda , TV(x)
]

où :

* ( TV(x) ) est la **variation totale** de l’image, pénalisant les variations abruptes pour favoriser des zones homogènes,
* ( \lambda ) contrôle le niveau de régularisation (compromis entre fidélité aux données et lissage).

```matlab
lambda = 0.02;

% f1(x) = 1/2 * ||x - y||_2^2
f1.prox = @(x, T) (x + T*y) ./ (1 + T);
f1.eval = @(x) 0.5 * norm(x(:) - y(:))^2;

% f2(x) = lambda * TV(x)
f2.prox = @(x, T) prox_tv(x, lambda*T);
f2.eval = @(x) lambda * norm_tv(x);
```

---

## 4. Paramètres du solveur

On configure les paramètres du solveur UNLocBoX pour itérer jusqu’à la convergence.

```matlab
param.verbose = 1;
param.maxit = 100;
param.tol = 1e-4;
```

---

## 5. Résolution du problème avec UNLocBoX

On utilise la fonction `solvep` de la boîte à outils UNLocBoX pour effectuer la minimisation.

```matlab
[x_sol, info] = solvep(y, {f1, f2}, param);
```

---

## 6. Visualisation des résultats

Les images bruitée et débruitée sont affichées côte à côte pour comparaison.

```matlab
figure;
subplot(1,2,1); imshow(y, []); title('Image bruitée');
subplot(1,2,2); imshow(x_sol, []); title('Image débruitée (TV)');
sgtitle('Débruitage par variation totale - UNLocBoX');
```

**Observation :**
L’image obtenue après régularisation par variation totale est plus lisse tout en conservant les contours nets, contrairement à une simple moyenne qui flouterait les détails.

---

## 7. Nettoyage

On ferme proprement la librairie UNLocBoX.

```matlab
close_unlocbox();
```

---

## Conclusion

Ce tutoriel montre comment utiliser **UNLocBoX** pour résoudre un problème de **débruitage d’image** à l’aide de la **variation totale (TV)**.
Cette méthode permet de supprimer efficacement le bruit tout en préservant les contours de l’image, ce qui la rend particulièrement adaptée aux applications de restauration et de reconstruction d’images.

---