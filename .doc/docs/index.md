# GOODFOOD DEV ZONE

Ce dépôt à pour objectif de fournir un environnement unifié aux développeurs de l'application GOODFOOD. Cette documentation reprendra les services fournis pour travailler sur le projet. 


## Contexte

Le projet GOODFOOD contient 3 dépôts de code qui doivent tous être testés pour garantir la stabilité de l'application et la non régression. Les avantages de séparer les outils de développement sont de pouvoir utiliser le même environnement et centraliser l'ensemble des pipelines.

## Démarrage rapide 

L'ensemble des conteneurs sont gérés par le fichier `docker-compose.yml`

### Pré-requis

* Docker installé et fonctionnel

### Installation & lancement

```bash
docker-compose up --build -d
```

## Services

Les services disponibles

|Service|Lien|Description|
|-|-|-|
|Jenkins|[localhost:8081](http://localhost:8081)|Gestionnaire de pipelines, utilisé pour l'intégration continue du dépôt|
|SonarQube|[localhost:9000](http://localhost:9000)|Analyseur de qualité de code utilisé dans les pipelines Jenkins|
|PgAdmin|[localhost:5050](http://localhost:5050)|Application web pour consulté et travailler sur la base de données|