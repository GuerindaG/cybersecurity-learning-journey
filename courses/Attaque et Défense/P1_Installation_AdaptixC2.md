# Installation et Présentation d’AdaptixC2

## 1. Présentation générale

AdaptixC2 est un **framework de type Command & Control (C2)** open-source conçu pour les **tests d’intrusion autorisés** et les opérations de **Red Team**.  
Il permet de gérer des agents déployés sur des systèmes compromis, d’exécuter des commandes à distance, de transférer des fichiers, de naviguer dans le système de fichiers, de gérer des sessions, d’extraire des informations et beaucoup d’autres opérations post-exploitation.

Le framework se compose principalement de deux parties :
- **AdaptixServer** : serveur écrit en Go (Golang) qui gère la communication avec les agents.
- **AdaptixClient** : client graphique multiplateforme (Linux, Windows, macOS) écrit en C++/Qt permettant à l’opérateur de contrôler les agents et de visualiser les données.
  
AdaptixC2 supporte une architecture **modulaire extensible** à l’aide de plugins appelés *extenders*. Il existe des extenders pour différents types de communications (HTTP/S, SMB, TCP, Gopher) et pour différents types d’agents/beacons.

> **Note de sécurité légale :** AdaptixC2 est destiné à un usage **autorisé dans des environnements contrôlés** (laboratoire, tests d’intrusion avec permission). Son usage non autorisé peut enfreindre la loi.

---

## 2. Utilité d’AdaptixC2

AdaptixC2 sert principalement à :

- **Post-exploitation avancée** : interaction avec les machines compromises après une intrusion contrôlée.
- **Simuler des attaques réelles** et tester la défense d’une infrastructure (capacité de détection et de réponse).
- **Gérer des sessions multiples** d’agents/hosts compromis.
- **Exécuter des tâches à distance** : commandes, fichiers, capture d’informations.
- **Extensibilité** via des modules (*extenders*) pour ajouter de nouvelles fonctionnalités.

Ce type de framework est comparable à d’autres outils utilisés en Red Team (par exemple Cobalt Strike), mais il est open-source.

---

## 3. Installation (selon la documentation officielle)

Les étapes principales pour installer AdaptixC2 sont les suivantes :

### 3.1. Récupération du code source

```bash
git clone https://github.com/Adaptix-Framework/AdaptixC2.git
cd AdaptixC2
````

### 3.2. Préinstallation des dépendances

Pour compiler les composants Serveur et Extenders :
Sous Debian / Ubuntu
````
sudo apt install mingw-w64 make gcc g++ g++-mingw-w64
wget https://go.dev/dl/go1.25.4.linux-amd64.tar.gz -O /tmp/go1.25.4.linux-amd64.tar.gz
sudo rm -rf /usr/local/go /usr/local/bin/go
sudo tar -C /usr/local -xzf /tmp/go1.25.4.linux-amd64.tar.gz
sudo ln -s /usr/local/go/bin/go /usr/local/bin/go
````
Une version précise de Go (1.25.4) est recommandée pour assurer la compatibilité de compilation.

### 3.3. Compilation des composants

Une fois les dépendances installées, les étapes de build suivantes sont disponibles :
**Construire le serveur et les extenders :**
````
make server-ext
````
**Construire uniquement le serveur :**
````
make build-server
````

**Construire uniquement les extenders :**
````
make build-extenders
````

**Construire le client :**
````
make client
````

Les fichiers compilés (serveur, client et extenders) sont placés dans le répertoire dist.

### 3.4. Docker (optionnel)

Il existe aussi des profils Docker Compose pour automatiser la construction et l’exécution des composants :

**Construire serveur seul :**
````
docker compose --profile build-server up
````

**Construire extenders :**
````
docker compose --profile build-extenders up
````

**Construire serveur + extenders :**
````
docker compose --profile build-server-ext up
````

**Démarrer le serveur en mode runtime :**
````
docker compose --profile runtime up -d
````

Ces profils simplifient le déploiement en environnement conteneurisé.

## 4. Démarrage et configuration basique

Après compilation, le serveur Adaptix peut être lancé en fournissant un profil JSON de configuration contenant les paramètres comme port, endpoint, mot de passe, certificats SSL et extenders à charger.

Le client graphique, une fois lancé, se connecte au serveur et permet de visualiser les sessions, exécuter des commandes, gérer les tâches, les tunnels et autres artefacts.

## 5. Conclusion

AdaptixC2 est un framework C2 moderne, modulaire et extensible utilisé pour les opérations de post-exploitation, red team et adversarial emulation. Il fournit une architecture serveur/client avec prise en charge de plugins pour écouter et interagir avec des agents.

L’installation nécessite des dépendances spécifiques (notamment Go, Qt pour le client) et suit des étapes documentées dans la documentation officielle.
