# Jenkins

Jenkins permet de créer et d'automatiser des tâches répétitives à travers des pipelines. Sur le projet Jenkins est utilisé pour automatiser les tests, la qualité de code, la couverture de code, etc..

[Documentation Officielle](https://www.jenkins.io/doc/)

## Configuration

Une fois les conteneurs lancés, rendez-vous sur [localhost:8081](http://localhost:8081)

Au premier démarrage vous devez récupérer le mot de passe temporaire pour créer le compte admin. Il est disponible sur le conteneur dans le fichier `/var/jenkins_home/secrets/initialAdminPassword`.

Pour le récupérer sur le conteneur:

```bash
docker-compose exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Puis créez le compte.

## Pipelines

Les configurations pour exécuter les pipelines des dépôts.

### API

Le pipeline est décrit dans le fichier `Jenkinsfile` à la racine du dépôt. Mais il faut configurer jenkins pour ajouter le projet et paramétrer les variables secretes.

Comment fonctionne le pipeline, pour les tests nous recréeons l'application dans sa globalité dans un environnement Docker (Docker in Docker). Ceci nous permet de réutiliser le même environnement que pour la production. Une fois les tests effectués et après avoir récupéré les rapports, les conteneurs sont détruits.

#### Plugins

Les plugins suivants sont requis pour exécuter le pipeline:

* GitHub plugin

#### Créer le projet

Ajouter un projet de type `Pipeline`, sur la page de configuration cochez les cases suivantes:

* GitHub project
  * Project Url : https://github.com/ZDubeau/GOOD-FOOD-2.0P.git

Dans la section Pipeline sélectionnez `Pipeline script from SCM` puis dans SCM sélectionnez `Git` et dans `Repository URL` spécifiez `https://github.com/ZDubeau/GOOD-FOOD-2.0P.git`. Dans la sous section Branches to build, laissez `*/*`.

Sauvegardez et votre projet est configuré, passez maintenant à la configuration des variables.


#### Variables

Sur Jenkins rendez-vous sur la page `Accueil > Administrer Jenkins > Manage Credentials > Store Jenkins > Identifiants globaux > Ajouter des identifiants`.

Toutes les variables ci-dessous sont de type `secret text`

!!! danger "Les valeurs sont à récupérer dans le .env"

|ID|Equivalent dans le .env du dépôt|
|-|-|
|goodfood_database_host|DB_HOST|
|goodfood_database|DB_DATABASE|
|goodfood_database_user|DB_USER|
|goodfood_database_password|DB_PASSWORD|
|goodfood_database_port|DB_PORT|
|typesense_api_key|TYPESENSE_API_KEY|

### Tester le pipeline

