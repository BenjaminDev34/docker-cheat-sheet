```markdown
## Images & Conteneurs

### Images

Les images sont l'un des deux blocs de construction principaux de Docker (l'autre étant les "Conteneurs"). Les images sont des plans/modèles pour les conteneurs.
Elles sont en lecture seule et contiennent l'application ainsi que l'environnement nécessaire à l'application (système d'exploitation, runtimes, outils, etc.).
Les images ne s'exécutent pas elles-mêmes, mais peuvent être exécutées sous forme de conteneurs.
Les images sont soit pré-construites (par exemple, des images officielles que vous trouvez sur DockerHub) soit vous construisez vos propres images en définissant un Dockerfile. 

Les Dockerfiles contiennent des instructions qui sont exécutées lors de la construction d'une image (`docker build .`), chaque instruction crée alors une couche dans l'image.
Les couches sont utilisées pour reconstruire et partager efficacement les images.
L'instruction CMD est spéciale : elle n'est pas exécutée lors de la construction de l'image mais lorsqu'un conteneur est créé et démarré à partir de cette image.

### Conteneurs

Les conteneurs sont l'autre élément clé de Docker. Les conteneurs sont des instances en cours d'exécution des images. Lorsque vous créez un conteneur (via `docker run`), une fine couche en lecture-écriture est ajoutée au-dessus de l'image. Plusieurs conteneurs peuvent donc être démarrés à partir d'une seule et même image. Tous les conteneurs s'exécutent en isolation, c'est-à-dire qu'ils ne partagent aucun état d'application ni données écrites. Vous devez créer et démarrer un conteneur pour démarrer l'application qui se trouve à l'intérieur d'un conteneur. Ce sont donc les conteneurs qui sont finalement exécutés - tant en développement qu'en production.

### Commandes Docker Clés

Pour une liste complète de toutes les commandes, ajoutez `--help` après une commande - par exemple, `docker --help`, `docker run --help` etc. Consultez également la documentation officielle pour une documentation complète et détaillée de TOUTES les commandes et fonctionnalités : [https://docs.docker.com/engine/reference/run/](https://docs.docker.com/engine/reference/run/)

Important : Cela peut être accablant ! Vous n'aurez besoin que d'une fraction de ces fonctionnalités et commandes en réalité !

- `docker build .` : Construire un Dockerfile et créer votre propre image basée sur le fichier
  - `-t NAME:TAG` : Attribuer un NOM et une ÉTIQUETTE à une image
- `docker run IMAGE_NAME` : Créer et démarrer un nouveau conteneur basé sur l'image IMAGE_NAME (ou utiliser l'id de l'image)
  - `--name NAME` : Attribuer un NOM au conteneur. Le nom peut être utilisé pour arrêter et supprimer, etc.
  - `-d` : Exécuter le conteneur en mode détaché - c'est-à-dire que la sortie imprimée par le conteneur n'est pas visible, l'invite de commande / terminal N'ATTEND PAS que le conteneur s'arrête
  - `-it` : Exécuter le conteneur en mode "interactif" - le conteneur / l'application est alors préparé pour recevoir des entrées via l'invite de commande / terminal. Vous pouvez arrêter le conteneur avec CTRL + C lorsque vous utilisez le flag `-it`
  - `--rm` : Supprimer automatiquement le conteneur lorsqu'il est arrêté
- `docker ps` : Lister tous les conteneurs en cours d'exécution
  - `-a` : Lister tous les conteneurs - y compris ceux qui sont arrêtés
- `docker images` : Lister toutes les images stockées localement
- `docker rm CONTAINER` : Supprimer un conteneur avec le nom CONTAINER (vous pouvez également utiliser l'id du conteneur)
- `docker rmi IMAGE` : Supprimer une image par nom / id
- `docker container prune` : Supprimer tous les conteneurs arrêtés
- `docker image prune` : Supprimer toutes les images pendantes (images non marquées)
  - `-a` : Supprimer toutes les images stockées localement
- `docker push IMAGE` : Pousser une image vers DockerHub (ou un autre registre) - le nom/l'étiquette de l'image doit inclure le nom de dépôt/url
- `docker pull IMAGE` : Tirer (télécharger) une image de DockerHub (ou un autre registre) - cela se fait automatiquement si vous exécutez simplement `docker run IMAGE` et que l'image n'a pas été tirée auparavant
```
