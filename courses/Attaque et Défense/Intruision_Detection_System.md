## 1. C'est quoi un IDS ?

Un IDS (Système de Détection d'Intrusion) est un outil de cybersécurité qui surveille le trafic réseau et les activités système pour identifier les activités suspectes, les menaces connues ou les tentatives d'accès non autorisées, et alerter les administrateurs en cas d'incident potentiel,
agissant comme un second niveau de défense après un pare-feu pour détecter ce qui pourrait le contourner.

## 2. À quoi sert un IDS ?

- Un IDS permet notamment de :
- Détecter des tentatives d’intrusion
- Identifier des attaques connues (scan de ports, brute force, malware, etc.)
- Repérer des comportements anormaux
- Alerter l’équipe sécurité (SOC) en temps réel
- Fournir des logs pour l’analyse et l’investigation

## 3. Quels sont les types d'IDS ?

Il existe deux grands types d'IDS : des **IDS réseau (NIDS)** pour le trafic global et des 
**IDS hôte (HIDS)** pour des appareils individuels, utilisant souvent des signatures d'attaques
connues ou la détection d'anomalies par rapport à un comportement normal. 

###  3.1 NIDS : Network Intrusion Detection System

Un système de détection des intrusions dans le réseau (NIDS) est une **application logicielle** qui permet de détecter et de signaler les problèmes de sécurité du réseau 
en surveillant les activités du réseau ou du système à la recherche de comportements malveillants ou anormaux.

**Exemples d'outils NIDS :** Snort, Suricata, Zeek (Bro)

### 3.1.a Comment fonctionne un NIDS ?

Un NIDS fonctionne comme un observateur passif du réseau : il est placé à un point stratégique pour copier le trafic réseau sans l’interrompre,
puis capture les paquets qui circulent. Il reconstitue les communications complètes à partir des paquets, décode les protocoles (HTTP, DNS, SSH, 
etc.) et analyse le contenu et le comportement du trafic. Cette analyse repose soit sur des **signatures d’attaques connues**( *Une signature 
d’attaque est un modèle reconnaissable ou une “empreinte” qui permet à un outil de sécurité d’identifier une attaque connue en comparant l’activité
observée à ce modèle.* ), soit sur la détection d’anomalies par rapport à un comportement normal. Lorsqu’un schéma suspect est identifié, le NIDS génère
des alertes et des logs qui peuvent être envoyés à un SIEM pour permettre au SOC d’analyser l’incident et de décider des actions à prendre.

Trafic réseau
      ↓
Capture des paquets
      ↓
Reconstitution des flux
      ↓
Décodage protocoles
      ↓
Moteur de règles / Anomalies
      ↓
Alertes + Logs
      ↓
SIEM / SOC


## 3.2 HIPS: Host-based Intrusion Detection System

Un HIDS (Host-based Intrusion Detection System) est un système de détection d'intrusion installé directement sur un appareil spécifique
(serveur, ordinateur) pour surveiller son activité interne (journaux système, intégrité des fichiers, processus) et détecter les anomalies ou les accès
non autorisés, agissant comme une sentinelle locale pour alerter sur des menaces comme les virus ou les tentatives d'élévation de privilèges.

**Exemples d'outils NIDS :** OSSEC, Wazuh, Tripwire

## 3.2.a Comment fonctionne un HIDS ?

Un HIDS (Host Intrusion Detection System) fonctionne en étant installé directement sur une machine (serveur ou poste) afin de surveiller en continu l’activité du système. Il analyse les fichiers critiques, les logs, les processus, les comptes utilisateurs et les configurations, puis compare les événements observés à des signatures d’attaques connues ou à un comportement normal attendu. Lorsqu’il détecte une modification suspecte ou une activité anormale (par exemple une tentative d’escalade de privilèges ou une altération de fichiers système),
il génère des alertes et des journaux, qui peuvent être envoyés à un SIEM afin de permettre au SOC d’identifier, analyser et traiter l’incident.

## 4. Quelle est la différence entre un IDS et un IPS?
 
Un IDS Passif, détecte et alerte (surveillance) tandis que l'IPS (Système de Prévention d'Intrusion) est actif.
Il détecte et bloque automatiquement les menaces et est souvent intégré avec l'IDS. 

## 5. Quel est l'utilité des IDS dans un SOC ?

Dans un SOC, l’IDS :

- Alimente le SIEM en alertes
- Sert de première ligne de détection
- Complète les pare-feu et EDR
- Participe à la corrélation d’événements
