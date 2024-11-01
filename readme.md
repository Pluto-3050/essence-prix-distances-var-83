# Visualisation des Prix de l'Essence et Distances des Stations dans le Var (83)
![image](https://github.com/user-attachments/assets/1f42f172-f84d-40b2-9c1d-06c0462984d5)
lien : [Visualisation des Prix de l'Essence et Distances des Stations dans le Var (83)](https://lookerstudio.google.com/reporting/50dbca1b-bc75-47e3-9b2a-81f24a34586d/page/7u2FE).

## Description
Je vous présente un dashboard interactif développé avec Looker Studio et connecté à une base de données BigQuery. Ce tableau de bord permet de visualiser les prix du carburant SP95 et la distance des stations par rapport à une position fixe dans le département 83 (Var).

## Fonctionnalités
- **Intégration avec BigQuery** pour des données en temps réel.
- **Visualisations interactives** avec avec des graphiques, tableaux, et cartes géographiques.
- **Possibilité d'appliquer** des filtres pour affiner l'affichage en fonction des besoins.].

## Prérequis
- **Compte Google Cloud Platform** avec accès à BigQuery.
- **Looker Studio** pour la création du dashboard.

## Source de données
1. **Connecter BigQuery à Looker Studio** :
   - **prendre les données sur ce site/telechager sous forme de csv**](https://www.data.economie.gouv.fr/explore/dataset/prix-des-carburants-en-france-flux-instantane-v2/export/?refine.carburants_indisponibles=SP95 ).
   - **nettoyer les données** enlever les colonnes inutiles
   -  Ouvrez Looker Studio et choisissez **Ajouter une source de données**.
   - importer le fichier sous forme de csv

2. **requetes sql utilisé** :
   - SELECT
  adresse,
  ville,
  CONCAT(adresse,' ',postal_code,' ',ville) as adresse_complete,
  ROUND(prix_sp95,2) as prix_sp95,
  latitude,
  longitude,
  ROUND(ST_DISTANCE(
    ST_GEOGPOINT(longitude, latitude),  -- Coordonnées de chaque station
    ST_GEOGPOINT(6.017279,43.099752)  -- Coordonnées fixes de ta maison
  ) / 1000,1) AS distance_from_home_km  -- Distance en kilomètres
FROM `fr_carburant.fr_carburant`
WHERE code_departement = "83"
AND prix_sp95 is not null 
ORDER BY distance_from_home_km asc

### Explication de la Requête

- **Sélection des Colonnes**
  - **adresse et ville** : Colonnes contenant respectivement l’adresse et la ville de chaque station-service.
  - **CONCAT(adresse, ' ', postal_code, ' ', ville) AS adresse_complete** : Combine l’adresse, le code postal et la ville en une seule chaîne de caractères pour créer une adresse complète.
  - **ROUND(prix_sp95, 2) AS prix_sp95** : Affiche le prix du carburant SP95, arrondi à deux décimales.
  - **latitude et longitude** : Coordonnées géographiques de chaque station, nécessaires pour calculer la distance.

- **Calcul de la Distance avec ST_DISTANCE**
  - **ST_DISTANCE(ST_GEOGPOINT(longitude, latitude), ST_GEOGPOINT(6.017279, 43.099752))** : Calcule la distance en mètres entre chaque station et un point fixe (coordonnées `6.017279` et `43.099752` correspondant à ta position).
  - **Conversion en kilomètres** : Le résultat est divisé par 1000 pour le convertir de mètres en kilomètres.
  - **ROUND(..., 1) AS distance_from_home_km** : La distance est arrondie à une décimale pour une présentation plus claire.

- **Filtrage avec WHERE**
  - **WHERE code_departement = "83"** : Sélectionne uniquement les stations situées dans le département 83 (Var).
  - **AND prix_sp95 IS NOT NULL** : Exclut les enregistrements sans prix pour le SP95.

- **Tri des Résultats**
  - **ORDER BY distance_from_home_km ASC** : Trie les résultats en ordre croissant par distance par rapport au point de référence, affichant les stations les plus proches en premier.

### Création d'un Template dans Looker Studio
- Je crée un template dans Looker Studio et j'importe ma requête pour personnaliser les filtres, les plages de dates et d’autres paramètres interactifs.

## Utilisation
- Accédez au dashboard en cliquant sur ce lien : [Visualisation des Prix de l'Essence et Distances des Stations dans le Var (83)](https://lookerstudio.google.com/reporting/50dbca1b-bc75-47e3-9b2a-81f24a34586d/page/7u2FE).
- Utilisez les filtres et les paramètres pour explorer les données.

## Auteur
– **Patrice-Gabriel Dary-Nereus**




