# UNLocBoX – Cours et Tutoriels MATLAB

## Présentation

Ce dépôt contient un **cours complet sur la boîte à outils UNLocBoX** ainsi que deux **tutoriels pratiques** permettant de se familiariser avec les méthodes de régularisation et d’optimisation convexes sous MATLAB.

L’objectif est d’illustrer, à travers des exemples simples et commentés, la puissance de la boîte à outils **UNLocBoX** pour résoudre des problèmes d’optimisation non lisses et de reconstruction de signaux ou d’images.

---

## Contenu du dépôt

### 1. **Cours UNLocBoX**
Le document de cours présente :
- L’introduction à l’optimisation convexe.  
- Les principes de la décomposition proximale.  
- L’utilisation et la structure de la toolbox UNLocBoX.  
- Des exemples de scripts et d’applications typiques.

### 2. **Tutoriel 1 – Régression LASSO**
Fichier : `tutoriel1_lasso.md`

Ce tutoriel montre comment :
- Générer un **signal parcimonieux**.  
- Formuler un **problème de régression LASSO**.  
- Utiliser **UNLocBoX** pour le résoudre avec la méthode **FISTA**.  
- Comparer le signal original et le signal reconstruit.

**Objectif :** reconstruire un signal à partir de mesures bruitées en exploitant la régularisation L1.

### 3. **Tutoriel 2 – Débruitage d’image par Variation Totale (TV)**
Fichier : `tutoriel2_tv.md`

Ce tutoriel illustre :
- Le débruitage d’une image par régularisation **TV (**
