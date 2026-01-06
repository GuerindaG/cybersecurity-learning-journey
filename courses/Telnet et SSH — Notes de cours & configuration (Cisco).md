# Telnet et SSH — Notes de cours & configuration (Cisco)

## Objectifs du cours

ÊTRE capable de :

* Comprendre le rôle de Telnet et SSH
* Expliquer les différences de sécurité entre Telnet et SSH
* Configurer Telnet sur un équipement Cisco
* Observer les failles de sécurité de Telnet
* Configurer SSH sécurisé (SSH v2)
* Comparer Telnet et SSH en situation réelle

---

## 1. Notions de base

### Accès distant à un équipement réseau

Un **accès distant** permet d’administrer un routeur ou un switch sans être physiquement connecté via un câble console.

Deux protocoles principaux existent :

* **Telnet** (ancien, non sécurisé)
* **SSH (Secure Shell)** (sécurisé, standard actuel)

---

## 2. Telnet

### Définition

Telnet est un protocole permettant une connexion distante en ligne de commande.

### Problème majeur

* Aucune sécurité
* Identifiants envoyés **en clair**
* Facilement interceptable (sniffing)

### Port utilisé

* TCP **23**

### Statut en entreprise

 **Interdit : considéré comme une vulnérabilité**

---

## 3. SSH (Secure Shell)

### Définition

SSH est un protocole d’accès distant **sécurisé**, basé sur le chiffrement.

### Sécurité

* Chiffrement des données
* Authentification sécurisée
* Protection contre interception et MITM

### Port utilisé

* TCP **22**

### Statut en entreprise

**Obligatoire : norme professionnelle**

---

## 4. Comparaison Telnet vs SSH

| Critère      | Telnet   | SSH     |
| ------------ | -------- | ------- |
| Port         | 23       | 22      |
| Chiffrement  | Non    | Oui   |
| Mot de passe | En clair | Chiffré |
| Sécurité     | Faible   | Élevée  |
| Usage pro    | Non    | Oui   |

---

## 5. Notion clé : les lignes VTY

### VTY (Virtual Teletype)

Les **lignes VTY** sont des lignes virtuelles permettant les connexions distantes (Telnet ou SSH).

* Chaque connexion utilise une ligne VTY
* Exemple : `line vty 0 4` → 5 connexions simultanées

---

## 6. Configuration IP (pré-requis)

```bash
interface g0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
```

- Sans IP active, aucun accès distant n’est possible.

---

## 7- Configuration TELNET (non sécurisé)

### Étape 1 : mot de passe enable

```bash
enable secret telnet123
```

### Étape 2 : utilisateur local

```bash
username admin password telnet123
```

### Étape 3 : configuration VTY

```bash
line vty 0 4
 login local
 transport input telnet
```

### 7. Test Telnet (PC)

```bash
telnet 192.168.1.1
```

### Observation sécurité

* Le mot de passe circule en clair
* Visible dans Wireshark / Packet Tracer

Telnet = risque majeur de sécurité

---

## 8. Désactivation de Telnet

```bash
line vty 0 4
 transport input none
```

 **Coupe tout accès distant**

---

## 9. Configuration SSH sécurisée

### Étape 1 : définir l’identité du routeur

```bash
hostname R1
ip domain-name lab.local
```

### Étape 2 : génération de la clé RSA

```bash
crypto key generate rsa
```

* Taille recommandée : **2048 bits**
* Active automatiquement SSH

### Étape 3 : utilisateur sécurisé

```bash
username admin secret ssh123
```

### Étape 4 : forcer SSH version 2

```bash
ip ssh version 2
```

### Étape 5 : configuration VTY pour SSH

```bash
line vty 0 4
 login local
 transport input ssh
```

---

## 🔎 Vérifications

```bash
show ip ssh
show users
show running-config | section vty
```

---

## 10. Test SSH (PC / Linux)

```bash
ssh admin@192.168.1.1
```

### Observation sécurité

* Données chiffrées
* Mot de passe non visible
* Connexion sécurisée

---

## Conclusion générale

* **Telnet** :

  * Facile à configurer
  * Totalement non sécurisé
  * À éviter absolument

* **SSH** :

  * Configuration plus complète
  * Sécurité forte
  * Standard en entreprise

> **Toujours utiliser SSH en production**


---

📌 *Document prêt pour GitHub / formation / TP CCNA / cybersécurité*
