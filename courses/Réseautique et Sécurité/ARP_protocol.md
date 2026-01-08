# Protocole ARP (Address Resolution Protocol)

## 1. Définition
Le protocole **ARP** permet de faire la correspondance entre :
- **Adresse IP** (logique, couche 3)
- **Adresse MAC** (physique, couche 2)

> ARP permet de trouver l’adresse MAC associée à une adresse IP sur un réseau local (LAN).

---

## 2. Fonctionnement

### Étapes
1. **Vérification de la table ARP**  
   - La machine cherche l’IP cible dans sa table ARP.  
   - Si elle existe → communication directe.  
   - Sinon → déclenchement d’une requête ARP.

2. **ARP Request (requête ARP)**  
   - Envoi en **broadcast** sur le réseau :  
     ```
     Qui a l’IP 192.168.1.20 ? Donne-moi ton adresse MAC.
     ```
   - Destination MAC : `FF:FF:FF:FF:FF:FF`.

3. **ARP Reply (réponse ARP)**  
   - Le propriétaire de l’IP répond en **unicast** avec son adresse MAC :
     ```
     192.168.1.20 → MAC xx:xx:xx:xx:xx:xx
     ```

4. **Mise à jour du cache ARP**  
   - La machine ajoute l’entrée IP ↔ MAC dans sa table ARP.  
   - La communication peut alors se faire directement.

---

## 3. Table ARP (exemple Linux)
```bash
arp -a
? (192.168.1.20) at 00:1A:92:4F:2B:11 [ether] on eth0
```

## 4. Limites de l’ARP

- Ne vérifie pas l’authenticité des réponses.
- Vulnérable à :
  - ARP Spoofing / Poisoning
  - Man-in-the-middle (MITM)
  - Empoisonnement de cache ARP

## 5. Contre-mesures

| Méthode                          | Description                                           |
| -------------------------------- | ----------------------------------------------------- |
| **ARP statique**                 | Définir manuellement IP ↔ MAC.                        |
| **Dynamic ARP Inspection (DAI)** | Vérifie ARP sur switches via DHCP Snooping.           |
| **DHCP Snooping**                | Construit une table IP ↔ MAC ↔ port pour valider ARP. |
| **Segmentation VLAN**            | Réduit la portée des broadcasts ARP.                  |
| **Outils de détection**          | arpwatch, XArp, IDS (Snort, Suricata).                |
| **IPv6 / NDP + SeND**            | Remplace ARP par un protocole sécurisé.               |

## 6. Durée de conservation dans le cache ARP

| Système       | Durée approximative                   |
| ------------- | ------------------------------------- |
| **Linux**     | ~60 secondes (réévaluation dynamique) |
| **Windows**   | 2 à 10 minutes selon usage            |
| **Cisco IOS** | 4 heures par défaut (modifiable)      |

Les entrées peuvent être renouvelées si la machine communique régulièrement avec l’IP.

## 7. Résumé

- ARP permet de résoudre les adresses IP en MAC sur un LAN.
- Fonctionnement : Vérification → ARP Request → ARP Reply → mise à jour du cache.
- Limites : vulnérable aux attaques ARP spoofing.
- Contre-mesures : ARP statique, DAI, DHCP Snooping, VLAN, outils de détection, IPv6/NDP.
- Le cache ARP conserve les entrées quelques minutes selon le système.
