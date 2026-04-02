# Projet de fin d'études — Climate Stress Testing du risque de crédit

## Présentation du projet

Ce projet a pour objectif de construire une chaîne de modélisation permettant d’évaluer l’impact de scénarios climatiques et macroéconomiques sur le risque de crédit d’un portefeuille d’entreprises.

L’approche repose sur quatre blocs principaux :

1. **Extraction d’un facteur systémique sectoriel** à partir des matrices de migration de notation, dans un cadre inspiré de **CreditMetrics** ;
2. **Modélisation satellite** reliant ce facteur à des variables macroéconomiques via une approche de **model averaging** ;
3. **Prise en compte des interdépendances sectorielles et géographiques** au moyen d’un **GVAR** ;
4. **Projection sous scénarios NGFS** pour produire des trajectoires de risque cohérentes à horizon long terme.

---

## Structure du dépôt

Le dépôt est organisé autour de plusieurs notebooks correspondant aux grandes étapes du projet.

### Dossiers

- **`data/`**  
  Contient les jeux de données utilisés dans le projet : données de notation, données macroéconomiques, données sectorielles, matrices d’input-output, etc. Vous y trouverez lz dossier facteur_systemique
comportant pour chaque zone geographique un sous dossier contenant les information extraite: barrières, matrices PIT, rho, MSE et les Zt par secteurs.

- **`documentation/`**  
  Contient les documents méthodologiques, notes de travail, éléments de cadrage ou de documentation intermédiaire.

- **`images/`**  
  Contient les figures produites au cours du projet : graphiques de validation, trajectoires, réponses impulsionnelles, comparaisons de scénarios, etc.

---

## Description des notebooks

### 1. `extraction.ipynb`
Notebook dédié à l’extraction du **facteur systémique sectoriel**.

Ce notebook permet notamment de :
- nettoyer et préparer les données de ratings ;
- construire des matrices de transition **PIT** ;
- calculer des matrices **TTC** de référence ;
- estimer les **barrières de migration** dans un cadre gaussien latent ;
- reconstruire le facteur systémique \( Z_t^s \) par secteur ;
- comparer les transitions observées et reconstruites.

---
### 2. `preprocessing_macro.ipynb`
Notebook de préparation des données macroéconomiques.

Il sert à :
- importer les séries macro ;
- préparation des variables macro ;
- stationnarisation et création des retards ;
- préparer les inputs utilisés dans les modèles satellites et le GVAR.

### 3. `model_averaging.ipynb`
Notebook consacré à la **modélisation satellite**.

Objectif :
expliquer et prévoir le facteur systémique sectoriel à partir de variables macroéconomiques.

Contenu principal :
-  préparation de la base de données macro x facteur systémique a partir de la base de données macro traité
- génération d’un grand ensemble de modèles candidats ;
- filtrage économétrique des modèles admissibles ;
- estimation des poids par **Jackknife Model Averaging (JMA)** ;
- comparaison avec une logique de sélection par modèle unique.


---


### 4. `gvar_model.ipynb`
Notebook principal d’estimation du **GVAR**.

Il permet de :
- construire les variables domestiques et étrangères ;
- intégrer les matrices de pondération (par exemple issues des relations input-output) ;
- estimer les équations sectorielles ;
- analyser les interdépendances entre secteurs / pays.

Ce notebook constitue le cœur de la partie propagation.

---

### 5. `gvar_ajout_choc.ipynb`
Notebook consacré à l’analyse des **chocs** dans le GVAR.

Contenu typique :
- définition d’un choc macroéconomique ou sectoriel ;
- propagation dynamique du choc ;
- étude de l’hétérogénéité des réponses sectorielles ;
- mesure des effets d’amplification liés aux interdépendances.

---

### 6. `gvar_global.ipynb`
Notebook de synthèse ou d’extension du GVAR à une dimension plus globale.

Il sert à analyser les interactions entre plusieurs blocs géographiques / sectoriels et à produire une vision agrégée des transmissions internationales.

---

### 7. `analyse_tes.ipynb`
Notebook d’analyse des tableaux entrées-sorties.

---

### 8. `ngfs.ipynb`
Notebook de projection sous **scénarios NGFS**.

Il permet de :
- importer et préparer les scénarios climatiques ;
- projeter les variables macroéconomiques ;
- alimenter les modèles satellites et/ou le GVAR ;
- obtenir des trajectoires futures du facteur systémique ;
- traduire ces trajectoires en mesures de risque de crédit.

---


## Prérequis techniques

Le projet est développé principalement en **Python** sous forme de notebooks Jupyter.

### Environnement recommandé
- Python 3.10+
- Jupyter Notebook / JupyterLab

### Bibliothèques principales
Selon les notebooks, les packages suivants peuvent être nécessaires :

- `pandas`
- `numpy`
- `matplotlib`
- `seaborn`
- `scipy`
- `statsmodels`
- `scikit-learn`
- `itertools`
- `os`
- `pathlib`

Selon les versions du projet, d’autres dépendances peuvent être ajoutées en fonction des traitements spécifiques.

---

## Installation

Cloner le dépôt :

```bash
git clone https://github.com/Celeste2405/PFE.git
cd PFE
