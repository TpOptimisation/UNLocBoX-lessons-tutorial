% =========================================================================
% TUTORIEL 2 : Débruitage d'image par variation totale (TV)
%  AUTEURs : 
        %  YOUBI BOPDA FREDY LOIC   20U2811
        %  DIBAKTO DEBAMI MANICH JORDAN 000000
        %  DAWAI HOSSEA 21T2436
        %  TCHANKUIMEU MEUGHELA AMAELLE  21T2548
% =========================================================================

clear; close all; clc;

% ------------------------------
% 1. Chargement des librairies
% ------------------------------
addpath(genpath('~/unlocbox'));
init_unlocbox();

% ------------------------------
% 2. Chargement / génération d'image
% ------------------------------
n = 128;
y = phantom(n); % Image test (sinogramme)
y = y + 0.05 * randn(size(y)); % ajout de bruit

% ------------------------------
% 3. Définition des fonctions convexes
% ------------------------------
lambda = 0.02;

% f1(x) = 1/2 * ||x - y||_2^2
f1.prox = @(x, T) (x + T*y) ./ (1 + T);
f1.eval = @(x) 0.5 * norm(x(:) - y(:))^2;

% f2(x) = lambda * TV(x)
f2.prox = @(x, T) prox_tv(x, lambda*T);
f2.eval = @(x) lambda * norm_tv(x);

% ------------------------------
% 4. Paramètres du solveur
% ------------------------------
param.verbose = 1;
param.maxit = 100;
param.tol = 1e-4;

% ------------------------------
% 5. Résolution avec UNLocBoX
% ------------------------------
[x_sol, info] = solvep(y, {f1, f2}, param);

% ------------------------------
% 6. Visualisation
% ------------------------------
figure;
subplot(1,2,1); imshow(y, []); title('Image bruitée');
subplot(1,2,2); imshow(x_sol, []); title('Image débruitée (TV)');
sgtitle('Débruitage par variation totale - UNLocBoX');

% ------------------------------
% 7. Nettoyage
% ------------------------------
close_unlocbox();
