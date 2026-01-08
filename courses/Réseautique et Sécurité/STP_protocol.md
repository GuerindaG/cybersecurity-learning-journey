# Spanning Tree Protocol (STP)

## 1. Définition
Le **Spanning Tree Protocol (STP)** est un protocole de **couche 2 (IEEE 802.1D)** qui permet :
- d’éviter les **boucles réseau**
- de garantir un chemin unique entre les switchs
- de protéger la stabilité du réseau Ethernet

STP désactive certains liens redondants pour empêcher les tempêtes de broadcast.

---

## 2. Pourquoi STP est indispensable ?
Dans un réseau avec redondance :
- les trames Ethernet n’ont pas de TTL
- une boucle provoque :
  - tempête de broadcast
  - saturation de la table MAC
  - réseau inutilisable

STP empêche ces situations.

---

## 3. Principe de fonctionnement
STP :
1. élit un **Root Bridge**
2. calcule le **chemin le plus court** vers le root
3. place certains ports en **Forwarding**
4. bloque les ports redondants

---

## 4. Notions fondamentales

### 4.1 Root Bridge
- Switch principal du réseau STP
- Tous les chemins sont calculés vers lui

#### Élection du Root Bridge
Basée sur le **Bridge ID (BID)** :
BID = Priorité + Adresse MAC


Le switch avec le **BID le plus faible** devient Root.

---

### 4.2 Coût STP (Path Cost)
Chaque lien a un coût selon sa vitesse :

| Vitesse | Coût |
|---|---|
| 10 Mbps | 100 |
| 100 Mbps | 19 |
| 1 Gbps | 4 |
| 10 Gbps | 2 |

STP choisit le **chemin au coût total le plus faible**.

---

## 5. Types de ports STP

| Type de port | Rôle |
|---|---|
| Root Port (RP) | Port le plus proche du Root |
| Designated Port (DP) | Port autorisé sur un segment |
| Blocking Port | Port bloqué pour éviter la boucle |

---

## 6. États STP d’un port
Blocking → Listening → Learning → Forwarding

| État | Rôle |
|---|---|
| Blocking | Bloque le trafic |
| Listening | Échange des BPDU |
| Learning | Apprend les MAC |
| Forwarding | Transmet le trafic |

⏱ Temps de convergence ≈ **30–50 secondes**

---

## 7. BPDU (Bridge Protocol Data Unit)
Messages STP échangés entre switchs contenant :
- Root Bridge ID
- coût du chemin
- timers STP

 Permettent l’élection et la convergence STP.

---

## 8. PortFast

### Définition
```bash
spanning-tree portfast
```
- Le port passe directement en Forwarding :
  technique réseau qui permet de rediriger les requêtes entrantes d'Internet
  (sur un port spécifique du routeur) vers un appareil précis (ordinateur, console, caméra)
  sur votre réseau local (LAN), en utilisant son adresse IP interne et son port dédié;
- Supprime les délais STP`;

### Utilisation

- Ports connectés à des PC
- Imprimantes
- Serveurs

**NB :** À ne jamais utiliser entre switchs

## 9. BPDU Guard

### Définition
`spanning-tree bpduguard enable`

- Désactive le port s’il reçoit un BPDU
- Protège contre les boucles et switchs non autorisés
- Souvent utilisé avec PortFast.

## 10. Configuration globale recommandée
`spanning-tree portfast default
spanning-tree bpduguard default`

## 11. Variantes de STP
| Protocole   | Standard | Convergence |
| ----------- | -------- | ----------- |
| STP         | 802.1D   | Lente       |
| RSTP        | 802.1w   | Rapide      |
| PVST+       | Cisco    | Par VLAN    |
| Rapid PVST+ | Cisco    | Très rapide |

## 12. Vérification STP
`show spanning-tree
show spanning-tree vlan 10
show spanning-tree interface fa0/1
`

## 13. Bonnes pratiques

- Forcer le Root Bridge
- Activer PortFast sur ports utilisateurs
- Activer BPDU Guard pour la sécurité
- Ne jamais désactiver STP en production

## A RETENIR 
STP empêche les boucles, protège le réseau et assure la redondance.
