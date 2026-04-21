# Data Mining - Analyse des Émissions Mondiales de Gaz à Effet de Serre (2013-2022)

**Projet SAE** (Situation d'Apprentissage et d'Évaluation) - BUT SD (ex DUT STID) S4

**Auteurs :** Ibrahim BENKHERFELLAH, Axel COULET, Nathan RUELLE

---

## Vue d'ensemble

Ce projet propose une analyse complète et multidimensionnelle des **émissions mondiales de gaz à effet de serre** sur la période 2013-2022, combinant deux sources institutionnelles majeures :

- **Our World in Data (OWID)** : données consolidées par l'Université d'Oxford
- **EDGAR 2025** : base de données détaillée par secteur de la Commission Européenne

### Objectifs

1. **Nettoyer et fusionner** deux sources de données hétérogènes
2. **Analyser explorativement** les distributions et tendances mondiales
3. **Identifier des profils de pays** via clustering multidimensionnel
4. **Visualiser** les relations entre émissions, PIB, population et énergie

---

## Structure du projet

```
.
├── README.md                          # Ce fichier
├── notebook.ipynb                     # Notebook Jupyter complet
├── requirements.txt                   # Bibliothèques nécessaires
├── data/
│   ├── initial_owid_dataset.csv       # Source OWID brute
|   ├── ...                            # Autres données OWID pour consolidation
│   ├── EDGAR_2025_GHGs...xlsx         # Source EDGAR 2025 (4 feuilles)
│   └── donnees_nettoyees.csv          # Dataset fusionné après ETL
└── .gitignore
```

---

## Phases du projet

### Phase 1 - Analyse Exploratoire (EDA)

**Dataset :** 2 180 observations (218 pays × 10 ans, de 2013 à 2022), 60 variables couvrant les émissions de GES (CO₂, CH₄, N₂O, F-gaz), le PIB, la population et la consommation d'énergie primaire.

**Nettoyage :** fusion de 11 sources par jointure sur `(country, year, iso_code)`, filtrage temporel 2013-2022, suppression des agrégats géographiques, harmonisation des unités (Mt et kt → tonnes), imputation des valeurs manquantes à 0. Trois variables dérivées créées : `co2_per_capita`, `gdp_per_capita` et `ghg_non_co2`.

**Résultats clés :** asymétrie extrême des distributions (quelques pays dominent les émissions absolues), variables per capita plus comparables, forte concentration du CO₂ charbon.

---

### Phase 2
#### Analyses Bivariées et Multivariées

**Corrélations :** matrice de corrélation et heatmap avec dendrogramme pour identifier les liens entre émissions, richesse et énergie.

**Régressions :** en dimension 2 d'abord (CO₂/hab vs PIB/hab), puis en dimension supérieure via une régression multiple intégrant l'ensemble des variables énergétiques et économiques pour mieux capturer les relations entre indicateurs.

**ACP :** analyse en composantes principales sur variables standardisées pour réduire la dimensionnalité, identifier les axes structurants du jeu de données et projeter les pays dans un espace interprétable.

#### Clustering

**En dimension 2 :** premier clustering sur le plan (PIB/hab × CO₂/hab) via K-Means et CAH Ward, permettant une lecture directe des groupes dans le scatter plot.

**En dimension supérieure :** clustering sur l'ensemble des variables standardisées (15 indicateurs), avec projection des résultats sur les composantes de l'ACP (2D puis 3D) pour visualiser des clusters qui ne sont plus représentables directement.

**Méthodes et validation :** choix de K via elbow plot et score de silhouette, comparaison K-Means vs CAH Ward sur les deux espaces (dim 2 et dim 15).

**Résultats :** identification de profils de pays (économies émergentes, pays riches bas-carbone, pétromonarchies), analyse des trajectoires temporelles entre clusters, et visualisation par projections ACP, heatmaps de profils et graphiques radar.

---

## Sources et références

### Données
- **OWID** : https://github.com/owid/co2-data
- **EDGAR 2025** : https://edgar.jrc.ec.europa.eu/

### Documentation méthodologique
- [IQR Method for Outlier Detection](https://procogia.com/interquartile-range-method-for-reliable-data-analysis/)
- [DataCamp - R² ajusté](https://www.datacamp.com/fr/tutorial/adjusted-r-squared)
- [K-Means & Silhouette Analysis](https://scikit-learn.org/stable/modules/clustering.html)

---

## Licence

Ce projet est proposé à des fins pédagogiques uniquement. Reproduction interdite sans autorisation.

**Dernière mise à jour :** Avril 2026