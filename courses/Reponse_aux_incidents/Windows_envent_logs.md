## 1- Windows event logs
[Microsoft](https://learn.microsoft.com/fr-fr/windows/win32/msi/event-logging)
Le journal des événements Windows (Windows Event Log) est une fonctionnalité système intégrée qui enregistre automatiquement et
chronologiquement les activités, erreurs, avertissements et informations survenus sur l'ordinateur. Il permet aux administrateurs 
de diagnostiquer les pannes, surveiller la sécurité et auditer les comportements applicatifs via un outil appelé l'Observateur d'événements.

### Fonctionnalités principales :

- Journaux Windows (System, Application, Sécurité) : Ils enregistrent **les pannes de pilotes**, **les erreurs de logiciels**, et **les tentatives de connexion**.
- Niveaux de gravité : Les journaux classent les événements par "**Information**", "**Avertissement**" ou "**Erreur**".
- Contenu détaillé : Chaque entrée contient la date, l'heure, la source de l'événement et un ID spécifique pour le dépannage.

- **Utilité :** Crucial pour résoudre les problèmes techniques, analyser les plantages et assurer la sécurité du système.
- **Accès :** Accessible en faisant un clic droit sur le bouton Démarrer et en choisissant « Observateur d'événements ».
- **Source :** Les données proviennent du système d'exploitation, des services et des applications installées.

## 2- Event id
[Microsoft](https://learn.microsoft.com/fr-fr/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor)
Un ID d'événement (Event ID) est un numéro unique attribué par le système d'exploitation Windows ou des applications pour identifier précisément le type d'événement survenu (erreur, avertissement, information) dans les journaux système. 
Ils permettent aux administrateurs de diagnostiquer rapidement des problèmes de sécurité, des pannes ou des changements de configuration.

### IDs d'événements Windows Courants et Critiques

- 4624 : Connexion réussie à un compte.
- 4625 : Échec de connexion à un compte.
- 4648 : Tentative de connexion avec des informations d'identification explicites.
- 4672 : Assignation de privilèges spéciaux (connexion administrateur).
- 4688 : Création d'un nouveau processus (utile pour détecter les malwares, nécessite une configuration).
- 4720 : Création d'un compte utilisateur.
- 4740 : Compte utilisateur verrouillé.
- 41 : Arrêt inattendu ou redémarrage du système (Kernel-Power).
- 1102 : Le journal d'audit de sécurité a été effacé.

### Comment accéder aux événements ?

Pour consulter ces codes, ouvrez l'Observateur d'événements en tapant **eventvwr.msc** dans la boîte de dialogue Exécuter **(Touche Windows + R)**. 

### Outils d'analyse

- SIEM : Les ID d'événement sont essentiels pour les systèmes de gestion des événements et des informations de sécurité.
- Sysmon : Outil Microsoft pour une surveillance avancée des processus.

## 3- Sysmon
[Microsoft](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
Sysmon (System Monitor) est un service système Windows et un pilote de périphérique avancé, développé par Sysinternals, qui surveille et enregistre l'activité détaillée du système dans le journal des événements Windows. 
Il est essentiel pour la détection des menaces, la sécurité et la criminalistique (forensics).

### Fonctionnalités Clés et Avantages :

- Surveillance Détaillée : Enregistre la création de processus, les connexions réseau, les modifications de l'heure de création des fichiers, et les changements de registre.
- Analyse de Sécurité : Permet d'identifier des activités malveillantes ou anormales (ex: hollowing de processus, persistance).
- Intégration Windows : S'exécute comme un processus protégé et persiste après les redémarrages.
- Intégration Native : Désormais disponible en tant que fonctionnalité optionnelle dans Windows 11 (24H2 et 25H2). 

### Installation et Configuration :

- Installation : S'installe via la ligne de commande (cmd.exe en administrateur) avec la commande **sysmon -i**.
- Configuration : Utilise des fichiers de configuration XML pour définir quels événements surveiller (filtres) afin d'optimiser le rapport signal/bruit. 
