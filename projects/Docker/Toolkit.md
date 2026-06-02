Commandes Docker — Guide Complet

1. Gestion des Images (Le Plan)

Avant de lancer un conteneur, il faut manipuler l’image qui sert de
modèle.

-   docker pull <image>:<tag> : Télécharge une image depuis le Docker
    Hub
    Exemple : docker pull ubuntu:22.04

-   docker images ou docker image ls : Liste toutes les images stockées
    localement sur ta machine.

-   docker rmi <image_id> : Supprime une image locale (si elle n’est pas
    utilisée par un conteneur).

-   docker image prune : Nettoie les images intermédiaires ou
    inutilisées (“dangling images”).

-   docker inspect <image_id> : Affiche les détails techniques d’une
    image (couches, variables d’environnement, configuration par
    défaut).

------------------------------------------------------------------------

2. Création et Build (L’Application)

Ces commandes interviennent lorsque tu crées ta propre application via
un Dockerfile.

-   docker build -t <nom_image>:<tag> . : Construit une image
    personnalisée à partir du Dockerfile présent dans le dossier actuel
    (.).

-   docker build --no-cache -t <nom_image> . : Force la reconstruction
    complète de l’image sans utiliser le cache.

-   docker commit <conteneur_id> <nouvelle_image> : Crée une image à
    partir des modifications actuelles d’un conteneur en cours
    d’exécution.

------------------------------------------------------------------------

3. Cycle de Vie des Conteneurs (L’Exécution)

Une fois l’image prête, voici comment donner vie et gérer tes
conteneurs.

docker run [options] <image>

Crée et démarre un conteneur.

Options courantes :

-   -d : Mode arrière-plan (detached).
-   -it : Mode interactif avec terminal.
-   --name <nom> : Donne un nom personnalisé.
-   -p <port_hote>:<port_conteneur> : Redirige les ports.
    Exemple : -p 8080:80
-   -v <chemin_hote>:<chemin_conteneur> : Monte un dossier local.
-   --restart=always : Redémarre automatiquement le conteneur.

Autres commandes

-   docker ps : Liste les conteneurs en cours d’exécution.
-   docker ps -a : Liste tous les conteneurs.
-   docker start <nom_ou_id> : Démarre un conteneur arrêté.
-   docker stop <nom_ou_id> : Arrête un conteneur proprement (SIGTERM).
-   docker kill <nom_ou_id> : Arrêt brutal (SIGKILL).
-   docker restart <nom_ou_id> : Redémarre le conteneur.
-   docker rm <nom_ou_id> : Supprime un conteneur arrêté.
-   docker rm -f <nom_ou_id> : Force la suppression.

------------------------------------------------------------------------

4. Debugging et Administration (Le Troubleshooting)

Pour inspecter ce qui se passe à l’intérieur des conteneurs.

-   docker exec -it <nom_ou_id> /bin/bash : Ouvre un terminal dans un
    conteneur actif.
    Ou /bin/sh si bash n’est pas installé.

-   docker logs <nom_ou_id> : Affiche les logs.

-   docker logs -f <nom_ou_id> : Suit les logs en temps réel.

-   docker top <nom_ou_id> : Liste les processus actifs.

-   docker stats : Affiche la consommation CPU, mémoire et réseau.

-   docker diff <nom_ou_id> : Montre les fichiers modifiés ou créés.

-   docker cp <chemin_hote> <nom_conteneur>:<chemin_conteneur> : Copie
    des fichiers entre l’hôte et le conteneur.

------------------------------------------------------------------------

5. Stockage et Réseau (Volumes & Networks)

Les Volumes

-   docker volume create <nom_volume> : Crée un volume.
-   docker volume ls : Liste les volumes.
-   docker volume rm <nom_volume> : Supprime un volume.
-   docker volume prune : Supprime les volumes inutilisés.

Les Réseaux

-   docker network create --driver <type> <nom_reseau> : Crée un réseau.

-   docker network ls : Liste les réseaux disponibles.

-   docker network connect <nom_reseau> <nom_conteneur> : Connecte un
    conteneur à un réseau.

-   docker network disconnect <nom_reseau> <nom_conteneur> : Déconnecte
    un conteneur.

-   docker network inspect <nom_reseau> : Affiche les détails d’un
    réseau.

------------------------------------------------------------------------

6. Automatisation : Docker Compose

  Selon ta version installée, la commande peut être docker-compose ou
  docker compose.

-   docker compose up : Lance l’infrastructure définie dans
    docker-compose.yml.

-   docker compose up -d : Lance en arrière-plan.

-   docker compose down : Arrête et supprime les ressources créées.

-   docker compose ps : Affiche le statut des services.

-   docker compose logs -f : Suit les logs de tous les services.

-   docker compose exec <nom_service> <commande> : Exécute une commande
    dans un service.

Exemple :

    docker compose exec db mysql -u root

------------------------------------------------------------------------

7. Avancé : Le Docker Socket (/var/run/docker.sock)

Le fichier /var/run/docker.sock est l’interface (socket UNIX) permettant
de communiquer directement avec le démon Docker.

Pourquoi l’utiliser ?

Il est souvent monté dans un conteneur pour lui permettre de contrôler
Docker sur la machine hôte (Docker-in-Docker ou gestion de conteneurs).

Exemple :

    docker run -d -p 9000:9000 --name portainer \
      -v /var/run/docker.sock:/var/run/docker.sock \
      portainer/portainer-ce

Alerte Cybersécurité

⚠️ Partager le docker.sock avec un conteneur équivaut à lui donner des
privilèges root sur la machine hôte.

Si un attaquant compromet ce conteneur, il peut potentiellement :

-   S’échapper du conteneur (container escape)
-   Contrôler entièrement la machine hôte
-   Réaliser une élévation de privilèges

Vecteur classique rencontré en CTF et en audit de sécurité.

------------------------------------------------------------------------

8. Commande Bonus : Nettoyage Général

Idéal pour repartir de zéro :

    docker system prune -a --volumes

Cette commande supprime :

-   Tous les conteneurs arrêtés
-   Les réseaux inutilisés
-   Les images non utilisées
-   Les volumes inutilisés

⚠️ À utiliser avec précaution.
