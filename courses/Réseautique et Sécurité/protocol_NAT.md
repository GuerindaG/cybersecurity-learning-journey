# NAT : Network Address Translation

## 1. Définition
Le **NAT (Network Address Translation)** est un mécanisme utilisé par les routeurs pour traduire les adresses IP privées d’un réseau local en une ou plusieurs adresses IP publiques afin de permettre l’accès à Internet.

Il modifie les informations contenues dans les en-têtes IP (et parfois les ports) lors du passage des paquets entre le réseau interne et le réseau externe.

---

## 2. Objectifs du NAT
- Permettre à plusieurs machines privées d’accéder à Internet
- Économiser les adresses IPv4 publiques
- Masquer les adresses IP internes
- Centraliser le contrôle du trafic réseau

---

## 3. Types de NAT

### 3.1 Static NAT
- Association **1 IP privée ↔ 1 IP publique**
- Utilisé pour rendre un serveur interne accessible depuis Internet

### 3.2 Dynamic NAT
- Association dynamique entre IP privées et un **pool d’IP publiques**
- L’IP publique peut changer

### 3.3 PAT (NAT Overload)
- Plusieurs IP privées utilisent **une seule IP publique**
- Différenciation par les **numéros de ports**
- Type de NAT le plus utilisé (box Internet)

---

## 4. Fonctionnement général
1. Un PC interne envoie une requête vers Internet
2. Le routeur NAT remplace l’IP source privée par son IP publique
3. Le routeur enregistre la translation dans sa table NAT
4. La réponse revient vers l’IP publique
5. Le routeur redirige la réponse vers le bon PC interne

---

## 5. Étapes de configuration NAT (Cisco IOS / Packet Tracer)

### 5.1 Configuration des interfaces
```bash
enable
configure terminal

interface gigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
 no shutdown
 exit

interface gigabitEthernet0/1
 ip address 82.54.32.1 255.255.255.252
 ip nat outside
 no shutdown
 exit
```

### 5.2 Création de la liste d’accès (ACL)
```
access-list 1 permit 192.168.1.0 0.0.0.255
```

### 5.3 Configuration du NAT Overload (PAT)
```
ip nat inside source list 1 interface gigabitEthernet0/1 overload
```

### 5.4 Configuration de la route par défaut
```
ip route 0.0.0.0 0.0.0.0 82.54.32.2
```

### 5.5 Vérification du NAT
```
show ip nat translations
show ip nat statistics
```

### 5.6 Commandes de dépannage
```
clear ip nat translations *
debug ip nat
```

## 6. Exemple réel

- Un PC avec l’adresse 192.168.1.10 accède à un serveur web 203.0.113.50 :
- IP source privée → IP publique du routeur
- Le port est modifié automatiquement (PAT)
- La réponse est renvoyée vers le bon PC interne

## 7. Sécurité et limites du NAT

- Le NAT ne remplace pas un pare-feu
- Peut bloquer certaines connexions entrantes
- Problèmes possibles avec VPN, VoIP et jeux en ligne
- Nécessite parfois du Port Forwarding

## 8. Figures

![Configuration NAT Cisco Packet Tracer](../images/nat.jpeg)

![Configuration NAT Cisco Packet Tracer](../images/nat2.jpeg)

![Illustration NAT](../images/eebedaad-b371-484c-9430-845355c4c8f0.png)

## 9. Résumé

- Le NAT permet la communication entre réseaux privés et Internet
- Le PAT est la méthode la plus courante
- Une bonne configuration NAT est essentielle en réseau d’entreprise
- Indispensable en administration réseau et cybersécurité

## 10. Mots-clés

*NAT, PAT, Static NAT, Dynamic NAT, Cisco IOS, Packet Tracer, IP privée, IP publique*
