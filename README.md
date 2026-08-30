# 🍷 Analyse des ventes et des stocks d'un e-commerce de vins avec Python

## 📌 Présentation

Ce projet consiste à analyser les données commerciales de **BottleNeck**, un marchand de vins disposant de plusieurs systèmes d'information pour gérer ses produits, ses ventes et ses stocks.

L'entreprise utilise notamment :

- un **ERP** contenant les références produits, les prix, les prix d'achat et les stocks ;
- une base issue du **site e-commerce** contenant les ventes et les informations produits ;
- une **table de liaison** permettant de faire correspondre les identifiants entre les deux systèmes.

L'objectif du projet est de consolider ces différentes sources, d'améliorer la qualité des données et de produire des analyses permettant d'aider le comité de direction dans le pilotage de l'activité.

Les ventes analysées correspondent au mois d'octobre.

---

## 🎯 Objectifs

Le projet comporte deux grandes phases.

### 1. Préparation et fiabilisation des données

- explorer les différentes sources de données ;
- identifier les valeurs manquantes et les incohérences ;
- vérifier l'unicité des identifiants ;
- corriger ou supprimer les données erronées ;
- nettoyer les données issues du site web ;
- rapprocher les références ERP et Web grâce à la table de liaison ;
- construire un dataset final propre et exploitable.

### 2. Analyse commerciale

L'analyse permet notamment d'étudier :

- le chiffre d'affaires total ;
- le chiffre d'affaires par produit ;
- les références générant le plus de chiffre d'affaires ;
- les produits les plus vendus ;
- la concentration des ventes avec une analyse de type Pareto ;
- la distribution des prix ;
- les valeurs atypiques ;
- la rotation et la valorisation des stocks ;
- les taux de marge ;
- les relations entre prix, ventes et stocks.

---

## 🗂️ Sources de données

Trois sources principales ont été utilisées :

### ERP

Le fichier ERP contient notamment :

- `product_id` : identifiant produit ERP ;
- `price` : prix de vente ;
- `purchase_price` : prix d'achat ;
- `stock_quantity` : quantité en stock ;
- `stock_status` : statut du stock ;
- `onsale_web` : présence ou non du produit sur le site.

### Site Web

L'extraction du site contient notamment :

- `sku` : identifiant produit côté web ;
- `total_sales` : quantité vendue ;
- `post_title` : nom du produit ;
- `post_type` : type de contenu.

### Table de liaison

La table de liaison permet de rapprocher :

- `product_id` côté ERP ;
- `id_web` côté site web.

Cette table est nécessaire car les deux systèmes n'utilisent pas les mêmes identifiants produits.

---

## 🧹 Nettoyage et préparation des données

Une analyse exploratoire a été réalisée sur chaque source avant leur fusion.

Plusieurs problèmes de qualité ont été identifiés.

### ERP

Les contrôles ont notamment porté sur :

- les types des variables ;
- les valeurs manquantes ;
- l'unicité des identifiants ;
- les prix ;
- les quantités en stock ;
- la cohérence entre `stock_quantity` et `stock_status`.

Plusieurs anomalies ont été détectées, notamment :

- des prix négatifs ;
- des quantités de stock négatives ;
- des incohérences entre la quantité disponible et le statut du stock.

Lorsque la valeur correcte pouvait être déterminée grâce à une règle métier fiable, elle a été corrigée.

Dans le cas contraire, les observations incohérentes ont été supprimées afin de ne pas introduire de valeurs arbitraires dans l'analyse.

### Données Web

L'extraction du site contenait également des données qui ne correspondaient pas directement à des produits.

Les traitements ont notamment consisté à :

- sélectionner uniquement les colonnes nécessaires ;
- identifier les lignes sans SKU ;
- supprimer les lignes inutilisables ;
- filtrer `post_type = product` ;
- éliminer les lignes correspondant à des `attachments`.

### Table de liaison

Des contrôles ont également été réalisés sur :

- l'unicité de `product_id` ;
- l'unicité de `id_web` ;
- les valeurs manquantes ;
- les références ne possédant pas de correspondance entre les systèmes.

---

## 🔗 Fusion des données

La consolidation a été réalisée en deux étapes.

### Étape 1

Fusion entre l'ERP et la table de liaison grâce à :

`product_id`

### Étape 2

Fusion avec les données du site web grâce à la correspondance :

`id_web ↔ sku`

Après nettoyage et consolidation, le dataset final contient **713 produits exploitables**.

---

## 🛠️ Création de nouveaux indicateurs

Plusieurs variables métier ont été créées à partir des données existantes.

### Chiffre d'affaires par produit

`CA = prix × quantité vendue`

Cet indicateur permet de mesurer la contribution de chaque référence au chiffre d'affaires.

### Prix hors taxes

Le prix HT a été calculé à partir du prix TTC afin de pouvoir analyser correctement la marge.

### Taux de marge

`Taux de marge = (Prix HT - Prix d'achat) / Prix d'achat`

Cet indicateur permet d'évaluer la rentabilité des différentes références.

### Rotation / couverture du stock

La quantité disponible a été comparée aux ventes du mois afin d'identifier les références présentant un niveau de stock important par rapport à leur rythme de vente.

### Valorisation du stock

`Valorisation du stock = Prix d'achat × Quantité en stock`

Cet indicateur permet d'estimer le capital immobilisé dans les stocks.

---

## 📊 Analyse de la distribution des prix

L'étude de la distribution montre notamment :

- un prix moyen d'environ **32 €** ;
- un prix médian d'environ **23 €** ;
- 50 % des références situées approximativement entre **14 € et 42 €** ;
- quelques références beaucoup plus coûteuses que le reste du catalogue.

La moyenne supérieure à la médiane indique une distribution asymétrique vers les prix élevés.

Ces références premium nécessitent donc une analyse spécifique avant de les considérer comme des erreurs.

---

## 🔎 Détection des valeurs atypiques

Deux méthodes statistiques ont été utilisées afin d'identifier les prix atypiques.

### Z-score

Le Z-score mesure l'éloignement d'une observation par rapport à la moyenne en nombre d'écarts-types.

`Z = (x - moyenne) / écart-type`

Les valeurs présentant un Z-score supérieur à 3 ont été étudiées comme valeurs potentiellement atypiques.

### Intervalle interquartile (IQR)

L'IQR mesure la dispersion des 50 % centraux de la distribution.

`IQR = Q3 - Q1`

Les valeurs situées au-delà de :

`Q3 + 1,5 × IQR`

ont été considérées comme des valeurs atypiques potentielles.

Cette méthode identifie **31 références**, soit environ **4,35 % du catalogue**.

### Interprétation

L'analyse métier montre que ces valeurs ne correspondent pas nécessairement à des erreurs de saisie.

Il s'agit principalement de références haut de gamme :

- champagnes ;
- grands crus ;
- cognacs premium ;
- vins rares.

Leurs prix d'achat sont également élevés, ce qui confirme la cohérence de leurs prix de vente.

Les valeurs statistiquement atypiques ne doivent donc pas être automatiquement supprimées : elles doivent d'abord être interprétées dans leur contexte métier.

---

## 💰 Analyse du chiffre d'affaires

Le chiffre d'affaires est calculé pour chaque référence avec :

`CA produit = prix × quantité vendue`

Le chiffre d'affaires total du site sur la période étudiée est d'environ :

### **143 324 €**

Un classement des références par chiffre d'affaires permet ensuite d'identifier les produits contribuant le plus à l'activité.

---

## 🏆 Analyse des meilleures références

Deux classements complémentaires ont été réalisés :

### Classement par chiffre d'affaires

Il permet d'identifier les produits générant le plus de valeur.

### Classement par quantité vendue

Il permet d'identifier les produits générant le plus de volume.

Cette distinction est importante car un produit très vendu n'est pas nécessairement celui qui génère le plus de chiffre d'affaires.

---

## 📈 Analyse de Pareto

Une analyse cumulative des ventes a été réalisée afin d'étudier la concentration du chiffre d'affaires et des volumes.

Pour chaque produit, la part du chiffre d'affaires a été calculée :

`Part du CA = CA produit / CA total`

Une somme cumulative permet ensuite d'identifier les références représentant environ 80 % du chiffre d'affaires.

La même approche a été appliquée aux quantités vendues.

Cette analyse permet d'identifier les références stratégiques du catalogue.

---

## 📦 Analyse des stocks

L'analyse des stocks permet d'identifier :

- les références disposant d'un stock important ;
- les produits présentant une faible rotation ;
- les risques de surstock ;
- le capital immobilisé.

Une attention particulière doit être portée aux produits combinant :

- un stock élevé ;
- des ventes faibles ;
- une valeur d'achat importante.

Ces références peuvent représenter un coût important pour l'entreprise.

---

## 📉 Analyse des marges

Le prix hors taxes et le prix d'achat ont été utilisés pour calculer le taux de marge des produits.

L'analyse montre une **rentabilité globalement positive**, avec un taux de marge moyen proche de **60 %**.

Les éventuelles marges négatives doivent faire l'objet d'une vérification spécifique afin de déterminer s'il s'agit :

- d'une erreur de prix ;
- d'une promotion ;
- d'une stratégie de déstockage ;
- ou d'une anomalie dans les données.

---

## 🔗 Analyse des corrélations

Une matrice de corrélation a été réalisée entre :

- le prix ;
- les quantités vendues ;
- le stock disponible.

Les principaux résultats sont :

- **stock / ventes : 0,44**
- **prix / ventes : -0,52**
- **prix / stock : -0,11**

### Interprétation

La relation négative entre prix et ventes indique que les produits les plus chers ont tendance à être vendus en plus petites quantités.

La relation positive entre stock et ventes suggère que les références les plus demandées disposent généralement de niveaux de stock plus importants.

Enfin, la relation très faible entre prix et stock indique que le niveau de stock ne semble pas directement déterminé par le prix du produit.

> Une corrélation indique une relation statistique entre deux variables, mais ne permet pas à elle seule de démontrer un lien de causalité.

---

## 💡 Recommandations

L'analyse met en évidence plusieurs pistes d'amélioration.

### Qualité des données

- renforcer les contrôles lors de la saisie ;
- empêcher les prix négatifs ;
- empêcher les stocks négatifs ;
- automatiser la cohérence entre quantité et statut de stock ;
- fiabiliser la table de correspondance entre ERP et site web ;
- mettre en place des contrôles réguliers sur les identifiants produits.

### Gestion commerciale

- suivre régulièrement les références générant le plus de chiffre d'affaires ;
- surveiller les produits à faible rotation ;
- limiter le surstock ;
- analyser les produits à marge faible ou négative ;
- suivre séparément les références premium.

### Data visualisation

Le dataset consolidé constitue une base exploitable pour la création future d'un tableau de bord permettant de suivre automatiquement :

- chiffre d'affaires ;
- ventes ;
- stocks ;
- marges ;
- rotation des produits ;
- anomalies de données.

---

## 🔐 RGPD

Les données utilisées dans ce projet concernent uniquement :

- des produits ;
- des ventes ;
- des prix ;
- des stocks.

Aucune donnée personnelle ou sensible n'est utilisée dans le périmètre analysé.

---

## 🧰 Technologies utilisées

- **Python**
- **Pandas**
- **NumPy**
- **Matplotlib**
- **Seaborn**
- **Plotly**
- **Jupyter Notebook**
- **Excel**

---

