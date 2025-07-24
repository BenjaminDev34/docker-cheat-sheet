# Guide sur Docker Compose

Docker Compose est un outil permettant de définir et de gérer des applications multi-conteneurs Docker. Grâce à un fichier YAML, vous pouvez configurer les services, réseaux et volumes nécessaires à votre application.

## Pourquoi utiliser Docker Compose ?

- Simplifie la gestion des conteneurs multiples.
- Permet de définir des environnements reproductibles.
- Facilite le partage de configurations entre développeurs.

## Syntaxes de base

Voici les éléments principaux d'un fichier `docker-compose.yml` :

### 1. Services
Définit les conteneurs à exécuter.
```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
```

### 2. Volumes
Permet de partager des données entre le conteneur et l'hôte.
```yaml
volumes:
  - ./data:/var/www/html
```


### Exemple complet
Voici un exemple d'un fichier `docker-compose.yml` pour une application web simple :
```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    networks:
      - webnet

  app:
    image: node
    volumes:
      - ./app:/usr/src/app
```

## Commandes utiles

- `docker-compose up`: Lance les services définis dans le fichier.
- `docker-compose down`: Arrête et supprime les conteneurs, réseaux et volumes créés.
- `docker-compose ps`: Affiche l'état des conteneurs.
- `docker compose down --rmi all` : supprimes les images

Pour plus d'informations, consultez la [documentation officielle](https://docs.docker.com/compose/).
