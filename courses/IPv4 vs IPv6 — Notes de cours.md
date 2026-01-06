# IPv4 vs IPv6 — Notes de cours

## 1. Limites du protocole IPv4

IPv4 (Internet Protocol version 4) est la première version largement utilisée du protocole IP. Malgré son succès historique, il présente aujourd’hui plusieurs **limites majeures** qui ont conduit à l’adoption d’IPv6.

---

### 1.1 Espace d’adressage insuffisant

* IPv4 utilise des **adresses de 32 bits**
* Nombre total d’adresses possibles ≈ **4,3 milliards**

 À l’époque, ce nombre semblait largement suffisant, mais aujourd’hui :

* Explosion d’Internet
* Smartphones
* Cloud
* IoT (objets connectés)

- **Les adresses IPv4 sont épuisées**.

---

### 1.2 Utilisation du NAT (Network Address Translation)

Pour pallier le manque d’adresses IPv4, on a introduit le **NAT**.

 Problèmes du NAT :

* Complexifie les réseaux
* Empêche la communication directe entre machines
* Pose des problèmes pour :

  * VoIP
  * Jeux en ligne
  * VPN
  * Applications temps réel

- Le NAT casse le principe de base d’Internet : *une adresse = une machine*.

---

### 1.3 Sécurité non intégrée

* IPv4 **n’intègre pas la sécurité par défaut**
* IPsec est optionnel

 Conséquence :

* Sécurité ajoutée après coup
* Configuration plus complexe

---

### 1.4 Configuration réseau complexe

* Configuration manuelle fréquente
* Dépendance forte à DHCP
* Peu adaptée aux grands réseaux modernes

---

### 1.5 Mauvaise prise en charge de la mobilité

* IPv4 n’a pas été conçu pour :

  * Les appareils mobiles
  * Les changements fréquents de réseau

---

## 2. Pourquoi IPv6 a été adopté

IPv6 (Internet Protocol version 6) a été conçu pour **corriger toutes les limites d’IPv4**.

---

### 2.1 Espace d’adressage immense

* IPv6 utilise des **adresses de 128 bits**
* Nombre d’adresses ≈ **3,4 × 10³⁸**

- Chaque appareil peut avoir **une adresse unique publique**.

---

### 2.2 Fin du NAT

* Grâce à l’abondance d’adresses
* Communication directe de bout en bout
* Réseaux plus simples et plus performants

---

### 2.3 Auto-configuration (SLAAC)

* Les hôtes peuvent se configurer automatiquement
* Pas besoin de serveur DHCP obligatoire

- Réseaux plus faciles à déployer et à gérer.

---

### 2.4 Sécurité intégrée

* **IPsec est intégré par défaut** dans IPv6
* Meilleure authentification et confidentialité

---

### 2.5 Meilleur support de la mobilité et de l’IoT

* Conçu pour les réseaux modernes
* Idéal pour les milliards d’objets connectés

---

## 3. Format des adresses IPv6

### 3.1 Structure générale

* Une adresse IPv6 = **128 bits**
* Découpée en **8 blocs de 16 bits**
* Représentée en **hexadécimal**
* Séparée par des deux-points `:`

 Exemple complet :

```
2001:0db8:0000:0000:0000:ff00:0042:8329
```

---

### 3.2 Règles de simplification

#### a) Suppression des zéros initiaux

```
0db8 → db8
0042 → 42
```

#### b) Utilisation de `::`

* `::` remplace **une suite de blocs 0000 consécutifs**
* Utilisable **une seule fois par adresse**

Exemple :

```
2001:db8::ff00:42:8329
```

---

## 4. Types d’adresses IPv6

### 4.1 Unicast

- Identifie **une seule interface**

| Type           | Préfixe   | Description          |
| -------------- | --------- | -------------------- |
| Global Unicast | 2000::/3  | Adresses publiques   |
| Link-local     | fe80::/10 | Communication locale |
| Loopback       | ::1/128   | Adresse locale       |

---

### 4.2 Multicast

- Communication vers **un groupe**

* Préfixe : `ff00::/8`
* Exemple :

```
ff02::1   → tous les hôtes du lien
```

---

### 4.3 Anycast

- Même adresse sur plusieurs interfaces
- Le paquet va vers **le routeur le plus proche**

---

### 4.4 Adresses spéciales

| Adresse        | Signification         |
| -------------- | --------------------- |
| ::             | Adresse non spécifiée |
| ::1            | Loopback              |
| ::ffff:x.x.x.x | IPv4 mappée IPv6      |

---

##  5. Notation CIDR en IPv6

###  Principe

* Même logique que IPv4
* Indique la **longueur du préfixe réseau**

 Format :

```
AdresseIPv6 / longueur
```

---

### Exemples

| Adresse           | Préfixe | Description   |
| ----------------- | ------- | ------------- |
| 2001:db8::/32     | /32     | Réseau global |
| 2001:db8:1:1::/64 | /64     | Réseau LAN    |
| fe80::/10         | /10     | Link-local    |

 En pratique :

* **/64 est le standard pour les LAN IPv6**
* SLAAC nécessite obligatoirement un `/64`

---

## Conclusion

IPv6 n’est pas une simple évolution d’IPv4.
C’est une **refonte complète** pensée pour :

* La croissance d’Internet
* La sécurité
* La mobilité
* Les réseaux modernes

 **IPv6 est le présent et l’avenir d’Internet.**

