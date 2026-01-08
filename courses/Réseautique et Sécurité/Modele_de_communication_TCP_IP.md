# Modèle TCP/IP : Acheminement des données et encapsulation

Le **modèle TCP/IP** (ou Internet) est le modèle pratique utilisé pour la plupart des réseaux modernes. Il est plus simple que le modèle OSI et comporte **4 couches principales**. Comme pour OSI, les données descendent côté émetteur et remontent côté récepteur, chaque couche ajoutant son **en-tête spécifique**.

---

## 1. Couche Application

**Rôle :**
- Fournir des services réseau aux applications.
- Interface entre le logiciel utilisateur et le réseau.

**Protocoles principaux :**
- HTTP / HTTPS
- FTP / SFTP
- SMTP / POP / IMAP
- DNS, DHCP
- Telnet, SNMP

**Unité de données :** Données applicatives

**En-tête ajouté :**
- Informations de protocole applicatif (ex : type de message, codage)

**Ce qui se passe :**
- Préparation du message ou requête pour le réseau.
- Optionnel : chiffrement ou compression avant transport.

---

## 2. Couche Transport

**Rôle :**
- Transport fiable ou non des données entre hôtes.
- Segmentation et réassemblage.
- Contrôle des flux et des erreurs.
- Gestion des **ports** pour distinguer les applications.

**Protocoles :**
- TCP (Transmission Control Protocol) → fiable
- UDP (User Datagram Protocol) → non fiable

**Unité de données :** Segment (TCP) / Datagramme (UDP)

**En-tête ajouté :**
- Ports source et destination
- Numéro de séquence / d’acquittement (TCP)
- Checksum pour contrôle d’erreurs
- Indicateurs de contrôle (flags TCP : SYN, ACK, FIN…)

**Ce qui se passe :**
- Les données applicatives sont découpées en segments.
- TCP assure la livraison fiable, UDP envoie simplement les datagrammes.

---

## 3. Couche Internet (Réseau)

**Rôle :**
- Acheminer les paquets entre hôtes (routage).
- Adressage logique (IP).
- Fragmentation et réassemblage.

**Protocoles :**
- IP (IPv4 / IPv6)
- ICMP (messages d’erreur et diagnostic, ex : ping)
- IGMP (gestion de groupes multicast)
- ARP / RARP (résolution d’adresse pour IPv4)

**Unité de données :** Paquet

**En-tête ajouté :**
- Adresse IP source et destination
- TTL (Time to Live)
- Protocole transport (TCP ou UDP)
- Champs de fragmentation si nécessaire

**Ce qui se passe :**
- Les segments TCP/UDP sont encapsulés dans des paquets IP.
- Les routeurs utilisent l’adresse IP pour acheminer chaque paquet vers la destination.

---

## 4. Couche Accès réseau (ou Liaison / Physique)

**Rôle :**
- Transmission locale des paquets sur le support physique.
- Adressage physique (MAC).
- Détection et correction d’erreurs locales.
- Gestion des signaux électriques, optiques ou radio.

**Protocoles / technologies :**
- Ethernet, Wi-Fi (802.11), PPP
- VLAN, ARP (pour la correspondance IP → MAC)

**Unité de données :** Trame (Frame)

**En-tête ajouté :**
- Adresse MAC source et destination
- Type de protocole (IP, ARP)
- Contrôle d’erreur (CRC)

**Ce qui se passe :**
- Les paquets IP sont encapsulés dans des trames et envoyés sur le réseau local.
- Les bits sont transmis sur le support physique.

---

## Récapitulatif de l’encapsulation TCP/IP

| Couche TCP/IP | Unité de données | En-tête ajouté | Protocoles principaux |
|---------------|-----------------|----------------|---------------------|
| Application   | Données         | En-tête applicatif | HTTP, FTP, SMTP, DNS |
| Transport     | Segment / Datagramme | Ports, séquence, checksum, flags | TCP, UDP |
| Internet      | Paquet          | IP source/destination, TTL, protocole transport | IP, ICMP, ARP |
| Accès réseau  | Trame           | MAC source/destination, type, CRC | Ethernet, Wi-Fi, PPP |

---

## 🔹 Flux des données TCP/IP

1. L’utilisateur crée un message ou requête (Application)  
2. La couche transport segmente le message et ajoute les ports et contrôle d’erreur  
3. La couche Internet encapsule le segment dans un paquet avec adresses IP  
4. La couche Accès réseau transforme le paquet en trame avec adresses MAC et contrôle d’erreurs  
5. Transmission physique des bits sur le support  
6. À l’arrivée, la désencapsulation inverse le processus jusqu’à l’application destinataire.

---

## 🔹 Comparaison avec OSI

| Modèle OSI | Modèle TCP/IP |
|------------|---------------|
| Application, Présentation, Session | Application |
| Transport | Transport |
| Réseau | Internet |
| Liaison, Physique | Accès réseau |

**Astuce mnémotechnique pour TCP/IP (haut → bas) :**  
> **A**pplication → **T**ransport → **I**nternet → **N**etwork Access  
> ("Application Transporte Internet sur le Network Access")

