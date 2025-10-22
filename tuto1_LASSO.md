% =========================================================================
% TUTORIEL 1 : Régression LASSO avec UNLocBoX
% AUTEURs : 
        %  YOUBI BOPDA FREDY LOIC   20U2811
        %  DIBAKTO DEBAMI MANICH JORDAN 000000
        %  DAWAI HOSSEA 21T2436
        %  TCHANKUIMEU MEUGHELA AMAELLE  21T2548
% =========================================================================

clear; close all; clc;

% ------------------------------
% 1. Chargement des librairies
% ------------------------------
addpath(genpath('~/unlocbox'));  % Adapter le chemin si besoin
init_unlocbox();

% ------------------------------
% 2. Génération des données
% ------------------------------
n = 100;      % Nombre de variables
m = 40;       % Nombre d'observations
A = randn(m, n);    % Matrice de mesure
x_true = zeros(n, 1);
x_true(1:10) = randn(10,1);     % Signal parcimonieux
b = A * x_true + 0.01 * randn(m,1); % Ajout de bruit

% ------------------------------
% 3. Définition des fonctions convexes
% ------------------------------
lambda = 0.05;

% Fonction f1(x) = 1/2 * ||A*x - b||_2^2
f1.prox = @(x, T) (A' * ((eye(m) + T*(A*A')) \ (b + (A*x)*T)));
f1.eval = @(x) 0.5 * norm(A*x - b)^2;

% Fonction f2(x) = lambda * ||x||_1
f2.prox = @(x, T) prox_l1(x, lambda*T);
f2.eval = @(x) lambda * norm(x,1);

% ------------------------------
% 4. Paramètres du solveur
% ------------------------------
param.verbose = 1;
param.maxit = 200;
param.tol = 1e-4;
param.gamma = 1;   % Pas du gradient
param.method = 'FISTA';

% ------------------------------
% 5. Résolution du problème
% ------------------------------
[x_sol, info] = solvep(randn(n,1), {f1, f2}, param);

% ------------------------------
% 6. Visualisation
% ------------------------------
figure;
stem(x_true, 'b', 'DisplayName', 'Signal original'); hold on;
stem(x_sol, 'r', 'DisplayName', 'Signal reconstruit');
legend;
xlabel('Index'); ylabel('Amplitude');
title('Résolution du problème LASSO avec UNLocBoX');
grid on;

% ------------------------------
% 7. Nettoyage
% ------------------------------
close_unlocbox();
