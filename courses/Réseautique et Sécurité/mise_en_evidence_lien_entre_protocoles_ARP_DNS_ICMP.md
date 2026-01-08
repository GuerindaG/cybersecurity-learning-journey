# Lien entre ARP, DNS et ICMP

Cette note explique comment les protocoles **ARP, DNS et ICMP** interagissent pour permettre la communication réseau.

---

## 1. DNS (Domain Name System)
- **Rôle** : Traduire un nom de domaine en adresse IP.
- **Exemple** : `www.google.com` → `142.250.74.100`
- **Importance** : Sans DNS, il faudrait connaître toutes les IP à la main.

---

## 2. ARP (Address Resolution Protocol)
- **Rôle** : Trouver l’adresse MAC correspondant à une IP dans le réseau local.
- **Exemple** : IP de la passerelle `192.168.1.1` → MAC `00:1A:2B:3C:4D:5E`
- **Importance** : Sans ARP, aucune communication locale au niveau Ethernet n’est possible.

---

## 3. ICMP (Internet Control Message Protocol)
- **Rôle** : Diagnostiquer et tester la communication réseau.
- **Fonctions principales** :
  - Vérifier si une machine est joignable (`ping`)
  - Suivre le chemin des paquets (`traceroute`)
  - Signaler des erreurs (destination unreachable, TTL dépassé)
- **Importance** : Permet de détecter et résoudre les problèmes réseau.

---

## 4. Interaction des protocoles dans un scénario réel

1. **DNS** : Résolution du nom de domaine → IP
2. **ARP** : Résolution de l’IP → MAC pour communication locale
3. **ICMP** : Diagnostic et retour d’erreur pendant la transmission

### Exemple
- Tape `www.google.com` dans un navigateur :
  1. DNS renvoie l’IP
  2. ARP trouve la MAC de la passerelle locale
  3. Les paquets sont envoyés et ICMP signale les erreurs ou vérifie la connectivité si besoin

---

## 5. Résumé 

| Protocole | Rôle | Moment d'intervention |
|-----------|------|---------------------|
| DNS       | Nom → IP | Avant toute communication |
| ARP       | IP → MAC | Juste avant l’envoi dans le réseau local |
| ICMP      | Test et diagnostic | Pendant la communication |

