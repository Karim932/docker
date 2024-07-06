# Projet Docker Full Stack

## Instructions pour construire et démarrer l'application

### Pré-requis

- Docker
- Docker Compose

### Étapes pour construire et démarrer l'application

1. **Clonez ce dépôt :**

    ```bash
    git clone <url-du-depot>
    cd projet
    ```

2. **Construisez et démarrez les conteneurs :**

    ```bash
    docker-compose up --build
    ```

    Cette commande va :
    - Construire les images Docker pour les services frontend et backend à partir des Dockerfiles situés dans les répertoires respectifs `front` et `back`.
    - Démarrer les conteneurs pour chaque service (frontend, backend, db, et phpmyadmin).
    - Créer et initialiser la base de données MySQL avec le script SQL fourni.

3. **Accédez à l'application :**
    - Front-end : [http://localhost:8080](http://localhost:8080)
    - Back-end : [http://localhost:8081](http://localhost:8081)
    - phpMyAdmin : [http://localhost:8082](http://localhost:8082)

## Architecture Docker de l'application

L'application est composée de quatre services principaux définis dans le fichier `docker-compose.yml` :

### Services

- **frontend** : Gère l'interface utilisateur.
    - **Dockerfile** : Situé dans le répertoire `front`.
    - **Ports** : `8080` (externe) mappé vers `80` (interne).
    - **Volumes** :
        - `./back/config:/var/www/html/config`
        - `./back/admin:/var/www/html/admin`
    - **Dépendances** : Dépend du service `backend`.

- **backend** : Gère la logique de l'application et les API.
    - **Dockerfile** : Situé dans le répertoire `back`.
    - **Ports** : `8081` (externe) mappé vers `80` (interne).
    - **Volumes** :
        - `./back/config:/var/www/html/config`
        - `./back/admin:/var/www/html/admin`
    - **Environnement** :
        - `DATABASE_HOST=db`
        - `DATABASE_USER=user`
        - `DATABASE_PASSWORD=root`
        - `DATABASE_NAME=FootKit`
    - **Dépendances** : Dépend du service `db`.

- **db** : Conteneur MySQL pour la base de données.
    - **Image** : `mysql:5.7`
    - **Environnement** :
        - `MYSQL_ROOT_PASSWORD=root`
        - `MYSQL_DATABASE=FootKit`
        - `MYSQL_USER=user`
        - `MYSQL_PASSWORD=root`
    - **Volumes** :
        - `db_data:/var/lib/mysql`
        - `./db/bdd.sql:/docker-entrypoint-initdb.d/bdd.sql`
    - **Ports** : `3306` (externe) mappé vers `3306` (interne).

- **phpmyadmin** : Interface graphique pour gérer la base de données MySQL.
    - **Image** : `phpmyadmin/phpmyadmin`
    - **Environnement** :
        - `PMA_HOST=db`
        - `MYSQL_ROOT_PASSWORD=root`
    - **Ports** : `8082` (externe) mappé vers `80` (interne).
    - **Dépendances** : Dépend du service `db`.

### Volumes

- **db_data** : Volume nommé pour la persistance des données MySQL.

### Réseaux

- **app-network** : Réseau Docker personnalisé pour assurer une communication sécurisée entre les conteneurs.

## Guide de test de la communication entre les conteneurs et la persistance des données

### Test de la communication entre les conteneurs

- **Tester la connexion front-end vers back-end :**
    1. Ouvrez votre navigateur et accédez à [http://localhost:8080](http://localhost:8080).
    2. Naviguez dans l'application pour vous assurer que le front-end peut interagir avec les API du back-end.

- **Tester la connexion back-end vers base de données :**
    1. Ouvrez phpMyAdmin à l'adresse [http://localhost:8082](http://localhost:8082).
    2. Connectez-vous avec les identifiants MySQL (user / root).
    3. Vérifiez que les tables définies dans `bdd.sql` sont présentes et que vous pouvez exécuter des requêtes.

### Test de la persistance des données

1. **Ajouter des données via l'application :**
    - Utilisez l'interface front-end pour ajouter des données à la base de données (par exemple, ajouter un nouveau maillot).

2. **Redémarrer les conteneurs :**
    - Arrêtez les conteneurs :
        ```bash
        docker-compose down
        ```
    - Démarrez à nouveau les conteneurs :
        ```bash
        docker-compose up
        ```

3. **Vérifier la persistance des données :**
    - Ouvrez phpMyAdmin à [http://localhost:8082](http://localhost:8082).
    - Connectez-vous et vérifiez que les données ajoutées précédemment sont toujours présentes dans la base de données.

En suivant ces instructions, vous devriez être en mesure de construire, démarrer, et tester votre application Docker avec succès.
message.txt
5 Ko
