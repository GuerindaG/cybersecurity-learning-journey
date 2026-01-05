# commentconfigurer un vlan sur un port ?

## 1. Accéder au mode de configuration globale :

    configure terminal (ou conf t).

## 2. Créer le VLAN (si nécessaire) :

    vlan [num_vlan] (ex: vlan 10)
    name [nom_vlan] (ex: name Ventes)
    exit.

## 3. Sélectionner l'interface (port) à configurer :

    interface FastEthernet 0/1 (remplacez par votre port, ex: GigabitEthernet1/0/1).

## 4. Définir le port en mode accès :

    switchport mode access.

## 5. Assigner le port au VLAN :

    switchport access vlan [num_vlan] (ex: switchport access vlan 10).

## 6. Vérifier la configuration :

    show vlan brief ou show interfaces FastEthernet 0/1 switchport pour confirmer l'assignation. 

## 7. Concepts clés

  **Port d'accès (Access Port)** : Un port dédié à un seul VLAN, utilisé pour connecter des périphériques finaux (PC, imprimantes) qui ne gèrent pas les VLAN eux-mêmes.
  **Port Trunk (Trunk Port)** : Un port qui transporte plusieurs VLAN, utilisé pour connecter des switches entre eux ou à un routeur (souvent avec le protocole 802.1Q).
  **VLAN Natif** : Le VLAN 1 par défaut, généralement non sécurisé et à éviter pour les autres VLAN. 
