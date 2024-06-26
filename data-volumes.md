# Data & Volumes

Les images sont en lecture seule - une fois créées, elles ne peuvent pas être modifiées (vous devez les reconstruire pour les mettre à jour). 

Les conteneurs, en revanche, peuvent lire et écrire - ils ajoutent une fine "couche de lecture-écriture" au-dessus de l'image. Cela signifie qu'ils peuvent apporter des modifications aux fichiers et dossiers de l'image sans réellement changer l'image.

Mais même avec des conteneurs en lecture-écriture, deux grands problèmes surviennent dans de nombreuses applications utilisant Docker :

1. **Les données écrites dans un conteneur ne persistent pas** : Si le conteneur est arrêté et supprimé, toutes les données écrites dans le conteneur sont perdues.
2. **Le conteneur ne peut pas interagir avec le système de fichiers de l'hôte** : Si vous modifiez quelque chose dans le dossier de projet de l'hôte, ces changements ne sont pas reflétés dans le conteneur en cours d'exécution. Vous devez reconstruire l'image (qui copie les dossiers) et démarrer un nouveau conteneur.

Le problème 1 peut être résolu avec une fonctionnalité Docker appelée "Volumes". Le problème 2 peut être résolu en utilisant des "Bind Mounts".

## Volumes

Les volumes sont des dossiers (et fichiers) gérés sur votre machine hôte qui sont connectés à des dossiers/fichiers à l'intérieur d'un conteneur.

Il existe deux types de volumes :

- **Volumes anonymes** : Créés via `-v /some/path/in/container` et supprimés automatiquement lorsqu'un conteneur est supprimé en raison de l'option `--rm` ajoutée à la commande `docker run`.
- **Volumes nommés** : Créés via `-v some-name:/some/path/in/container` et NON supprimés automatiquement.

Avec les volumes, les données peuvent être passées dans un conteneur (si le dossier sur la machine hôte n'est pas vide) et peuvent être sauvegardées lorsqu'elles sont écrites par un conteneur (les modifications apportées par le conteneur sont reflétées sur votre machine hôte).

Les volumes sont créés et gérés par Docker - en tant que développeur, vous ne savez pas nécessairement où exactement les dossiers sont stockés sur votre machine hôte. Parce que les données qui y sont stockées ne sont pas destinées à être visualisées ou éditées par vous - utilisez les "Bind Mounts" si vous avez besoin de le faire !

Au lieu de cela, surtout les volumes nommés peuvent vous aider à persister les données. 

Étant donné que les données ne sont pas seulement écrites dans le conteneur mais aussi sur votre machine hôte, les données survivent même si un conteneur est supprimé (car le volume nommé n'est pas supprimé dans ce cas). Vous pouvez donc utiliser des volumes nommés pour persister les données des conteneurs (par exemple, fichiers journaux, fichiers téléchargés, fichiers de base de données, etc.).

Les volumes anonymes peuvent être utiles pour s'assurer qu'un dossier interne au conteneur n'est pas écrasé par un "Bind Mount" par exemple.

Par défaut, les volumes anonymes sont supprimés si le conteneur a été démarré avec l'option `--rm` et a ensuite été arrêté. Ils ne sont pas supprimés si un conteneur a été démarré (et ensuite supprimé) sans cette option.

Les volumes nommés ne sont jamais supprimés, vous devez le faire manuellement (via `docker volume rm VOL_NAME`, voir la référence ci-dessous).

## Bind Mounts

Les Bind Mounts sont très similaires aux volumes - la différence clé est que vous, le développeur, définissez le chemin sur votre machine hôte qui doit être connecté à un chemin à l'intérieur d'un conteneur.

Vous le faites via `-v /absolute/path/on/your/host/machine:/some/path/inside/of/container`. Le chemin devant le `:` (c'est-à-dire le chemin sur votre machine hôte, vers le dossier qui doit être partagé avec le conteneur) doit être un chemin absolu lors de l'utilisation de `-v` sur la commande `docker run`.

Les Bind Mounts sont très utiles pour partager des données avec un conteneur qui pourrait changer pendant que le conteneur fonctionne - par exemple, votre code source que vous souhaitez partager avec le conteneur exécutant votre environnement de développement.

N'utilisez pas les Bind Mounts si vous voulez simplement persister les données - les volumes nommés devraient être utilisés pour cela (exception : vous voulez pouvoir inspecter les données écrites pendant le développement).

En général, les Bind Mounts sont un excellent outil pendant le développement - ils ne sont pas destinés à être utilisés en production (puisque votre conteneur doit fonctionner isolé de sa machine hôte).

## Commandes Docker clés

- `docker run -v /path/in/container IMAGE` : Crée un volume anonyme à l'intérieur d'un conteneur.
- `docker run -v some-name:/path/in/container IMAGE` : Crée un volume nommé (nommé `some-name`) à l'intérieur d'un conteneur.
- `docker run -v /path/on/your/host/machine:path/in/container IMAGE` : Crée un Bind Mount et connecte un chemin local sur votre machine hôte à un chemin dans le conteneur.
- `docker volume ls` : Liste tous les volumes actuellement actifs / stockés (par tous les conteneurs).
- `docker volume create VOL_NAME` : Crée un nouveau volume nommé `VOL_NAME`. Vous n'avez généralement pas besoin de le faire, car Docker les crée automatiquement pour vous s'ils n'existent pas lors de l'exécution d'un conteneur.
- `docker volume rm VOL_NAME` : Supprime un volume par son nom (ou ID).
- `docker volume prune` : Supprime tous les volumes non utilisés (c'est-à-dire non connectés à un conteneur en cours d'exécution ou arrêté).
