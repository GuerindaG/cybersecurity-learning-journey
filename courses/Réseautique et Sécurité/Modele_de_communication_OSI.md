# Modèle OSI : Acheminement des données et encapsulation

Le modèle **OSI (Open Systems Interconnection)** est un cadre théorique qui décrit comment 
les données circulent d’un ordinateur à un autre sur un réseau. Il est divisé en **7 couches**, chacune ayant des fonctions, protocoles et unités de données spécifiques. 
Lorsqu’un message est envoyé, il descend les couches côté émetteur et remonte côté récepteur. 
À chaque couche, un **en-tête spécifique** est ajouté pour assurer le bon acheminement et le traitement des données.

---

## 1. Couche Application (C7)

**Rôle :**
- Fournir des services réseau aux applications utilisateurs.
- Interface directe avec les logiciels (navigateur, messagerie, FTP, etc.).

**Protocoles principaux :**
- HTTP / HTTPS
- FTP / SFTP
- SMTP / POP / IMAP
- DNS, DHCP
- Telnet, SNMP

**Unité de données :** Données applicatives

**En-tête ajouté :**
- En-tête applicatif (ex : type de message, codage, métadonnées)

**Ce qui se passe :**
- L’utilisateur crée une requête ou un message.
- La couche application prépare les données pour la présentation.

---

## 2. Couche Présentation (C6)

**Rôle :**
- Formatage et traduction des données (ex : ASCII, UTF-8).
- Compression et décompression.
- Chiffrement et déchiffrement des données.

**Protocoles / standards :**
- TLS / SSL
- JPEG, PNG, MP3, MPEG
- ASCII, EBCDIC

**Unité de données :** Données formatées

**En-tête ajouté :**
- Informations de formatage ou chiffrement (ex : type de codage, clé de chiffrement)

**Ce qui se passe :**
- Les données applicatives sont transformées dans un format réseau standard.
- Les données peuvent être chiffrées pour la sécurité.

---

## 3. Couche Session (C5)

**Rôle :**
- Établir, gérer et fermer des sessions de communication.
- Synchronisation des échanges.

**Protocoles :**
- NetBIOS Session Service
- RPC (Remote Procedure Call)

**Unité de données :** Données de session

**En-tête ajouté :**
- Identifiants de session
- Points de reprise (checkpoint) pour reprise en cas d’erreur

**Ce qui se passe :**
- Une session est ouverte entre deux applications pour maintenir la communication.
- La couche session gère la synchronisation et la reprise.

---

## 4. Couche Transport (C4)

**Rôle :**
- Transport fiable ou non des données entre hôtes.
- Segmentation et réassemblage des données.
- Contrôle d’erreurs et de flux.
- Gestion des **ports** pour distinguer les applications.

**Protocoles :**
- TCP (Transmission Control Protocol) → fiable
- UDP (User Datagram Protocol) → non fiable

**Unité de données :** Segment (TCP) / Datagramme (UDP)

**En-tête ajouté :**
- Ports source et destination
- Numéro de séquence et d’acquittement (TCP)
- Contrôle d’erreurs (checksum)

**Ce qui se passe :**
- Les données sont découpées en segments.
- Chaque segment reçoit des informations pour permettre la livraison fiable et le contrôle des flux.

---

## 5. Couche Réseau (C3)

**Rôle :**
- Acheminement des données entre réseaux (routage).
- Adressage logique (IP).
- Fragmentation et réassemblage si nécessaire.

**Protocoles :**
- IP (IPv4, IPv6)
- ICMP (ping, erreurs réseau)
- IPsec (sécurité réseau)
- OSPF, RIP, BGP (protocoles de routage)

**Unité de données :** Paquet

**En-tête ajouté :**
- Adresse IP source et destination
- TTL (Time to Live)
- Protocole transport (TCP/UDP)
- Identification pour fragmentation

**Ce qui se passe :**
- Les segments sont encapsulés dans des paquets IP.
- Chaque paquet contient les informations nécessaires pour atteindre l’hôte destinataire.

---

## 6. Couche Liaison de données (C2)

**Rôle :**
- Transmission locale de paquets sur le même réseau.
- Adressage physique (MAC).
- Détection et correction d’erreurs locales.
- Contrôle de flux entre machines directement connectées.

**Protocoles / technologies :**
- Ethernet, Wi-Fi (802.11)
- ARP (Address Resolution Protocol)
- PPP, VLAN

**Unité de données :** Trame

**En-tête ajouté :**
- Adresse MAC source et destination
- Type de protocole réseau (IP, ARP…)
- Contrôle d’erreur (CRC)

**Ce qui se passe :**
- Les paquets IP sont encapsulés dans des trames.
- Les trames circulent sur le réseau local jusqu’au routeur ou hôte suivant.

---

## 7. Couche Physique (C1)

**Rôle :**
- Transmission des **bits** sur le support physique.
- Conversion en signaux électriques, optiques ou radio.
- Détermination du type de câble ou fréquence.

**Supports :**
- Câbles Ethernet cuivre, fibre optique
- Wi-Fi / radio
- Bluetooth

**Unité de données :** Bits

**En-tête ajouté :**
- Aucun (les bits sont transmis bruts)

**Ce qui se passe :**
- La trame est convertie en signaux pour être transmise sur le support physique.
- À l’arrivée, les signaux sont convertis en trames pour la couche liaison.

---

## Récapitulatif : Encapsulation des données

| Couche | Nom de l’unité | En-tête ajouté | Exemples de protocoles |
|--------|----------------|----------------|-----------------------|
| Application (C7) | Données | Application header | HTTP, FTP, SMTP |
| Présentation (C6) | Données formatées | Format / chiffrement | TLS, JPEG, ASCII |
| Session (C5) | Données de session | Identifiant session | RPC, NetBIOS |
| Transport (C4) | Segment / Datagramme | Ports, séquence, checksum | TCP, UDP |
| Réseau (C3) | Paquet | IP source/destination, TTL | IP, ICMP |
| Liaison (C2) | Trame | MAC source/destination, CRC | Ethernet, Wi-Fi |
| Physique (C1) | Bits | Aucun | Câble, fibre, radio |

---

## 🔹 Résumé du flux

1. L’utilisateur crée un message (C7)  
2. Les données sont formatées, chiffrées, segmentées (C6 → C4)  
3. Les segments sont encapsulés dans des paquets IP (C3)  
4. Les paquets sont mis dans des trames avec adresses MAC (C2)  
5. Les trames sont converties en signaux électriques/optique/radio (C1)  
6. À la réception, le processus est inversé (**désencapsulation**) jusqu’à l’application destinataire.

---

## 🔹 Astuce mnémotechnique

**Pour se souvenir des couches (de haut en bas) :**  
> **A**nalyse **P**rofessionnelle **S**upervise **T**ous **R**outages **L**ocaux **P**arfaitement  
(Application – Présentation – Session – Transport – Réseau – Liaison – Physique)

