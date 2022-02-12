# SonarQube

Comme Jenkins SonarQube est lancé avec Docker pour tourner localement. SonarQube permet d'analyser le code pour produire des rapports sur la qualité du code.

## Démarrage rapide

Au minimum vous devez avoir le service `db` du fichier `./docker-compose.yml`, le service `db` héberge une base de données PostgreSQL pour la persistence des données du service.

Pour lancer SonarQube

```bash
docker-compose up --build -d db sonarqube
```

!!! danger "Si SonarQube ne démarre pas checkez les logs du container"
    Si vous avez l'erreur "vm.max_map_count is too low" et que vous êtes sur windows, ouvrez votre terminal wsl et rentrez la commande suivante `sudo sysctl -w vm.max_map_count=262144`

## Configuration

Quand vous lancez SonarQube pour la première fois, identifiez vous avec le login `admin` et le mot de passe `admin`. Puis créez votre compte.

### Configurez Jenkins

Le serveur Jenkins à besoin d'accèder à sonarqube pour exécuter des appels API pour cela rendez-vous sur le serveur [Jenkins](http://localhost:8081).

#### Installer le plugin SonarQube

Sur Jenkins rende-vous dans `Administrer Jenkins > Gestion des plugins`, allez dans l'onglet `Disponibles` et tapez dans la barre de recherche `SonarQube Scanner for Jenkins`. Puis sélectionnez le et installez le.

Une fois le plugin installé, nous allons créer un token sur SonarQube pour que Jenkins puisse contacter le serveur. Donc sur SonarQube allez sur votre profil puis dans l'onglet `Security` générez un nouveau token, une fois votre token créé, copiez le et retournez sur Jenkins dans `Administrer Jenkins > Manage Credentials > Store Jenkins > Identifiants globaux > Ajouter des identifiants`, sélectionnez le type `Secret Text` dans ID rentrez `sonarqube_token` et collez votre token dans le `Secret`. 

Maintenant que jenkins peut contacter SonarQube rendez-vous dans `Administrer Jenkins > Configurer le système > SonarQube servers`, cliquez sur `Ajouter une installation SonarQube` et rentrez les valeurs suivantes:

* Nom: SonarQube
* URL du Serveur: http://sonarqube:9000
* Server Authentification Token: sonarqube_token

Une fois la configuration de l'application Jenkins effectuée, il reste encore à installer le scanner de SonarQube sur Jenkins pour cela rendez-vous dans `Administrer Jenkins > Configuration globale des outils > SonarQube Scanner`, puis cliquez sur `Installations SonarQube Scanner` puis sur `Ajouter un installateur > Installer depuis Maven Central`. Dans `Name` rentrez `sonar_scanner`, cochez la case `Install automatically`. Enregistrez et c'est tout bon pour l'intégration de SonarQube dans Jenkins.


!!! info "Vous pouvez maintenant utiliser SonarQube dans vos pipelines Jenkins"
    Exemple de configuration `Jenkinsfile`
    
    ```
        environment {
            scannerHome = tool 'sonar_scanner'
        }
    ```

    Puis 

    ```
        withSonarQubeEnv('SonarQube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
    ```



