# TP – Configuration du DHCP Snooping sur un switch Cisco

## 1. Contexte
Ce TP porte sur la mise en place du **DHCP Snooping**, un mécanisme de sécurité de couche 2 permettant de protéger le réseau contre les **serveurs DHCP frauduleux (Rogue DHCP)**.  
L’objectif est d’autoriser uniquement un serveur DHCP légitime à distribuer des adresses IP aux clients.

---

## 2. Objectifs du TP
- Créer et configurer un VLAN utilisateur
- Configurer un serveur DHCP légitime
- Activer et paramétrer le DHCP Snooping
- Définir les ports *Trusted* et *Untrusted*
- Tester le comportement du réseau après sécurisation

---

## 3. Environnement
- Outil : Cisco Packet Tracer
- Équipements :
  - 1 Switch Cisco
  - 1 Routeur (DHCP Server)
  - 2 PC clients
- VLAN utilisé : VLAN 10 (USERS)
- Plan d’adressage : 192.168.10.0 /24

---

## 4. Création du VLAN

```bash
Switch> enable
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name USERS
Switch(config-vlan)# exit
```
## 5. Affectation des ports au VLAN

**Ports utilisateurs (PC)**

`Switch(config)# interface range fa0/2 - 3
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit
`

**Port vers le routeur DHCP**

`Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# exit
`

## 6. Activation du DHCP Snooping

`Switch(config)# ip dhcp snooping
Switch(config)# ip dhcp snooping vlan 10`

À ce stade, tous les ports sont UNTRUSTED par défaut.

## 7. Définition des ports Trusted et Untrusted

**Port Trusted (vers le routeur DHCP)**

`Switch(config)# interface fa0/1
Switch(config-if)# ip dhcp snooping trust
Switch(config-if)# exit`

Les ports Trusted sont généralement :

- les routeurs
- les serveurs DHCP légitimes
- les uplinks vers d’autres switches

**Ports Untrusted (clients)**

Les ports utilisateurs restent Untrusted par défaut.
Optionnel : limitation du taux de paquets DHCP

`Switch(config)# interface range fa0/2 - 3
Switch(config-if-range)# ip dhcp snooping limit rate 10
Switch(config-if-range)# exit
`

## 8. Configuration du serveur DHCP (Routeur)

`Router> enable
Router# configure terminal
Router(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10
Router(config)# ip dhcp pool VLAN10-POOL
Router(dhcp-config)# network 192.168.10.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.10.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit
`

**Interface du routeur**

`Router(config)# interface g0/0
Router(config-if)# ip address 192.168.10.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit
`

## 9. Option 82 (DHCP Relay Information)

L’état affiché “Insertion of option 82 is disabled” indique que le switch n’ajoute pas l’Option 82 aux paquets DHCP.
Dans ce TP, cela n’impacte pas le fonctionnement du DHCP Snooping et l’option n’était pas requise.

## 10. Tests de fonctionnement

Sur les PC :
`ipconfig /release
ipconfig /renew
`
**Résultat attendu**

- Attribution correcte d’une adresse IP
- Masque, passerelle et DNS valides
- Le DHCP fonctionne uniquement via le port trusted

## 11. Difficultés rencontrées

- Oubli de l’activation globale du DHCP Snooping
- Mauvais VLAN associé au DHCP Snooping
- Port vers le routeur non configuré en trusted
- Échec de ipconfig /renew dû au blocage DHCP par le switch

Ces difficultés m'ont permis de mieux comprendre le rôle du DHCP Snooping dans la sécurisation du réseau.

## 12. Conclusion

Le DHCP Snooping est une mesure de sécurité essentielle permettant de contrôler la distribution des adresses IP et de se protéger contre les attaques de type Rogue DHCP.
Ce TP m'a permis de comprendre son fonctionnement, sa configuration et son impact direct sur le trafic DHCP.
