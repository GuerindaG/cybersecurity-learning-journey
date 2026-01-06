# Configuration d’un serveur DHCP sur un routeur Cisco (Packet Tracer)

## Environnement
- Outil : Cisco Packet Tracer
- Équipement : Routeur Cisco, PC clients
- Réseau utilisé : `192.168.1.0 /24`

---

## Étapes de configuration

### 1. Accès au routeur
Connexion au routeur via le CLI (localement ou à distance par Telnet/SSH).

```
Router> enable
Router# configure terminal
```
### 2. Exclusion des adresses IP
Les adresses réservées (routeur, serveurs, etc.) sont exclues du pool DHCP.

```Router(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10```

### 3. Création du pool DHCP
Définition du réseau, de la passerelle et du DNS.
```Router(config)# ip dhcp pool LAN-POOL
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.1.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# lease 7
```

Explication des paramètres :
- network : réseau concerné par le DHCP
- default-router : passerelle distribuée aux clients
- dns-server : serveur DNS
- lease : durée du bail DHCP

## 4. Configuration de l’interface réseau

L’interface reliée au réseau des clients doit être correctement configurée et active.

`Router(config)# interface g0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
`

## 5. Activation du service DHCP

Le service DHCP est activé (généralement actif par défaut).

`Router(config)# service dhcp
`

## 6 Vérification de la configuration

Commandes utilisées pour vérifier le bon fonctionnement du DHCP :

`Router# show ip dhcp pool
Router# show ip dhcp binding
Router# show ip dhcp conflict
`

## 7 Configuration des clients

**Sur les PC clients :**

- Desktop → IP Configuration
- Sélection de l’option DHCP

**Les clients reçoivent automatiquement :**

- Adresse IP
- Masque de sous-réseau
- Passerelle par défaut
- Serveur DNS

**Résultat:**

- Le routeur distribue correctement les adresses IP aux hôtes du réseau.
- Les clients peuvent communiquer entre eux et accéder à la passerelle sans configuration IP manuelle.

