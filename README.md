
# Bottleneck — E-commerce Profitability & Inventory Optimisation

## Executive Summary

Analyse de la performance commerciale et financière d'un
site e-commerce spécialisé dans la vente de vins et spiritueux.

Cette étude de cas vise à accompagner la direction dans
le pilotage du chiffre d'affaires, la maîtrise de la
rentabilité, l'optimisation des stocks et la fiabilisation
des données commerciales.

L'analyse repose sur la consolidation de trois sources
de données : ERP, site e-commerce et table de liaison.

## Key Business Findings

| Indicateur | Résultat |
|---|---:|
| Produits analysés | 713 |
| Sources de données consolidées | 3 |
| Chiffre d'affaires TTC calculé | 143 324,10 € |
| Quantités vendues | 5 743 |
| Valorisation du stock au coût d'achat | 277 328,07 € |
| Quantités en stock | 16 740 |
| Taux de marge moyen par produit | 61,01 % |
| Prix atypiques identifiés | 31 |
| Produits à marge négative | 1 |
| Taux de marge minimum | -86,39 % |
| Références stockées sans vente sur la période | 3 |
| Valeur de ces stocks sans vente | 14 959,40 € |

Périmètre temporel :
- Ventes du 1er au 31 octobre.
- Situation des stocks au 31 octobre.

## Business Challenge

Bottleneck dispose de données commerciales réparties
entre plusieurs systèmes d'information.

L'absence d'identifiants communs et les incohérences
présentes dans les fichiers limitent la fiabilité
des analyses commerciales et financières.

Les principaux enjeux sont :

- Fiabiliser les données commerciales.
- Mesurer la performance des ventes.
- Identifier les anomalies de rentabilité.
- Optimiser la gestion des stocks.
- Analyser le positionnement tarifaire.
- Accompagner les décisions d'approvisionnement.

## Data Sources

### ERP

825 observations initiales contenant les informations
relatives aux produits, aux prix d'achat, aux prix
de vente et aux quantités en stock.

### E-commerce

1 513 observations initiales contenant les données
de ventes et les informations produits.

### Mapping Table

825 observations permettant de rapprocher les
identifiants ERP et e-commerce.

## Analytical Approach

### 1. Data Quality & Preparation

- Analyse exploratoire des trois sources.
- Contrôle des valeurs manquantes.
- Vérification de l'unicité des identifiants.
- Identification des doublons techniques.
- Suppression des lignes non exploitables.
- Traitement des prix et stocks négatifs.
- Correction des incohérences de statut de stock.

### 2. Data Integration

- Jointure externe gauche entre l'ERP et la table
  de liaison.
- Jointure interne avec les données e-commerce.
- Contrôle des correspondances entre les systèmes.
- Construction d'un dataset final de 713 produits.

### 3. Business KPI Development

Création des indicateurs suivants :

- Chiffre d'affaires par produit.
- Quantités vendues.
- Prix de vente hors taxes.
- Taux de marge commerciale.
- Valorisation des stocks au prix d'achat.
- Couverture de stock estimée en mois.

### 4. Statistical Analysis

- Analyse descriptive des prix.
- Détection des valeurs atypiques avec Z-score et IQR.
- Analyse de la concentration du chiffre d'affaires.
- Analyse des ventes et des stocks.
- Analyse des marges commerciales.
- Matrice de corrélation.

## Business Findings

### Commercial Performance

Le chiffre d'affaires TTC calculé atteint 143 324,10 €
pour 5 743 unités vendues sur la période étudiée.

L'analyse des produits les plus contributifs permet
d'identifier les références à surveiller en priorité
dans le cadre de la stratégie commerciale.

### Financial Performance

Le taux de marge moyen par produit atteint 61,01 %.

Une référence présente cependant une anomalie majeure :

- Prix de vente TTC : 12,65 €
- Prix d'achat : 77,48 €
- Prix de vente HT : environ 10,54 €
- Taux de marge : -86,39 %
- Perte brute théorique par unité : 66,94 €

Cette situation justifie une vérification prioritaire
des données tarifaires et des conditions d'achat.

### Inventory Management

La valorisation totale des stocks au coût d'achat
atteint 277 328,07 €, pour 16 740 unités.

Une analyse complémentaire du dataset final identifie
trois références disposant de stock mais n'ayant
enregistré aucune vente pendant le mois étudié.

Ces références représentent une valeur de stock
de 14 959,40 €.

Ce constat constitue une piste d'investigation pour
optimiser les approvisionnements et le besoin
en fonds de roulement.

### Pricing Strategy

L'analyse statistique des prix révèle :

- Prix moyen : 32,32 €
- Prix médian : 23,40 €
- Prix maximum : 225 €
- 31 prix atypiques identifiés dans la soutenance.

Les prix atypiques correspondent notamment à des
produits premium.

Un prix statistiquement atypique ne constitue donc
pas nécessairement une erreur de tarification.

### Correlation Analysis

| Variables | Corrélation |
|---|---:|
| Stock / Ventes | +0,44 |
| Prix / Ventes | -0,52 |
| Prix / Stock | -0,11 |

Les résultats mettent en évidence une association
négative entre le prix et les volumes vendus.

Ils constituent des pistes d'analyse commerciale,
sans permettre d'établir une relation causale.

## Strategic Recommendations

1. Vérifier immédiatement les références présentant
   une marge négative.

2. Analyser les produits sans vente et les références
   présentant une couverture de stock élevée.

3. Adapter les approvisionnements aux performances
   commerciales et aux historiques de demande.

4. Identifier les références combinant contribution
   au chiffre d'affaires et rentabilité élevée.

5. Approfondir l'analyse des performances par
   segment tarifaire.

6. Standardiser les identifiants produits entre
   l'ERP et le site e-commerce.

7. Automatiser les contrôles de qualité portant
   sur les prix, les stocks et les marges.

8. Mettre en place un reporting mensuel de suivi
   de la performance commerciale et financière.

## Business Value

Cette analyse fournit à la direction une base
décisionnelle permettant de :

- Identifier les risques de perte de marge.
- Orienter les décisions d'approvisionnement.
- Prioriser les investigations sur les stocks.
- Mieux comprendre la performance commerciale.
- Fiabiliser les données utilisées pour le reporting.
- Accompagner les décisions tarifaires.

Les bénéfices présentés correspondent à des usages
potentiels et non à des économies effectivement
mesurées après déploiement.

## Analytical Limitations

- L'analyse porte sur un seul mois de ventes.
- L'absence de vente ne signifie pas nécessairement
  qu'un produit constitue un stock dormant.
- Le taux de marge moyen est une moyenne par référence,
  et non un taux de marge global pondéré par les ventes.
- La couverture de stock doit être traitée séparément
  pour les produits sans vente.
- Les corrélations ne démontrent pas de causalité.
- Le calcul du chiffre d'affaires repose sur les prix
  disponibles et les quantités vendues.

## Technical Stack

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Plotly
- Jupyter Notebook
- Data Cleaning
- Statistical Analysis
- Financial Analytics
- Inventory Analytics

## Project Context

Étude de cas professionnalisante réalisée dans le cadre
du projet 6 de la certification Data Analyst OpenClassrooms.

## Author

Ines Belhoussine

Business Data Analyst | Business Intelligence
& Strategic Decision Support
