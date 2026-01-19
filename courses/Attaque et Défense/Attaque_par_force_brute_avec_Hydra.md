# TP – Attaque par force brute avec Hydra (TryHackMe)

## 1. Contexte du TP

Ce TP est réalisé dans le cadre d’un laboratoire **TryHackMe**, ayant pour objectif de comprendre et pratiquer une **attaque par force brute** sur des services réseau à l’aide de l’outil **Hydra**.

L’attaque est effectuée dans un environnement contrôlé et pédagogique, uniquement à des fins d’apprentissage en cybersécurité.

---

## 2. Objectifs pédagogiques

- Comprendre le principe d’une attaque par force brute
- Utiliser Hydra pour attaquer :
  - un formulaire web (HTTP POST)
  - un service SSH
- Identifier des identifiants valides
- Accéder au système distant via SSH
- Récupérer un flag TryHackMe
- Savoir analyser et expliquer les commandes utilisées

---

## 3. Environnement de travail

- **Machine attaquante** : Kali Linux
- **Outil principal** : Hydra
- **Wordlist** : rockyou.txt
- **Cible** : Machine TryHackMe
- **Adresse IP cible** : `10.65.167.78`
- **Utilisateur ciblé** : `molly`

---

## 4. Outil utilisé : Hydra

### 4.1 Définition

**Hydra** est un outil de craquage de mots de passe en ligne permettant d’effectuer des attaques par force brute ou dictionnaire sur de nombreux services réseau tels que :

- HTTP / HTTPS
- SSH
- FTP
- SMB
- RDP
- MySQL, etc.

---

## 5. Attaque par force brute sur un formulaire HTTP

### 5.1 Commande utilisée

```bash
hydra -l molly -P /home/kali/Downloads/rockyou.txt-main/rockyou.txt 10.65.167.78 http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V
````
### 5.2 Décomposition détaillée de la commande

| Élément           | Description                                    |
| ----------------- | ---------------------------------------------- |
| `hydra`           | Lancement de l’outil Hydra                     |
| `-l molly`        | Nom d’utilisateur fixe ciblé                   |
| `-P rockyou.txt`  | Liste de mots de passe utilisée                |
| `10.65.167.78`    | Adresse IP de la cible                         |
| `http-post-form`  | Type de service attaqué (formulaire HTTP POST) |
| `/login`          | URL du formulaire de connexion                 |
| `username=^USER^` | Champ du formulaire pour le nom d’utilisateur  |
| `password=^PASS^` | Champ du formulaire pour le mot de passe       |
| `F=incorrect`     | Message retourné en cas d’échec                |
| `-V`              | Mode verbeux (affiche chaque tentative)        |

## 6. Attaque par force brute sur le service SSH

### 6.1 Commande utilisée

````
hydra -l molly -P /home/kali/Downloads/rockyou.txt-main/rockyou.txt 10.65.167.78 -t 4 ssh
````
### 6.2 Décomposition de la commande

| Élément          | Description                                |
| ---------------- | ------------------------------------------ |
| `-l molly`       | Nom d’utilisateur ciblé                    |
| `-P rockyou.txt` | Wordlist utilisée                          |
| `10.65.167.78`   | IP de la machine cible                     |
| `-t 4`           | Nombre de threads (connexions simultanées) |
| `ssh`            | Service attaqué                            |

## 7. Connexion SSH à la machine cible

### 7.1 Commande utilisée
````
ssh user@addressMachine
````

---

## Différence entre Hydra et John The Ripper

Bien que **Hydra** et **John The Ripper** soient tous deux des outils de craquage de mots de passe, ils n’ont **pas le même rôle ni le même mode de fonctionnement**.

---

### 1. Nature de l’attaque

| Outil | Type d’attaque |
|-----|---------------|
| **Hydra** | Attaque **en ligne** |
| **John The Ripper** | Attaque **hors ligne** |

- **Hydra** teste des identifiants directement contre un **service actif** (SSH, HTTP, FTP, etc.).
- **John The Ripper** travaille sur des **hashs de mots de passe déjà récupérés**, sans interaction avec le service cible.

---

### 2. Pré-requis

| Outil | Pré-requis |
|-----|-----------|
| **Hydra** | Accès réseau au service cible |
| **John The Ripper** | Hashs des mots de passe |

Exemples :
- Hydra → nécessite une IP, un port et un service ouvert
- John → nécessite des fichiers comme `/etc/shadow`, hash NTLM, MD5, SHA, etc.

---

### 3. Vitesse d’attaque

- **Hydra** :
  - Plus lent
  - Limité par le réseau
  - Peut être bloqué par des protections (rate limit, captcha, fail2ban)

- **John The Ripper** :
  - Très rapide
  - Utilise le CPU/GPU
  - Pas détectable par la cible (attaque hors ligne)

---

### 4. Cas d’utilisation typiques

| Situation | Outil adapté |
|--------|--------------|
| Formulaire web | Hydra |
| Service SSH | Hydra |
| Hash Linux (/etc/shadow) | John The Ripper |
| Hash Windows (NTLM) | John The Ripper |
| Dump de base de données | John The Ripper |

---

### 5. Exemple de commandes

#### Hydra (attaque en ligne)
```bash
hydra -l molly -P rockyou.txt 10.65.167.78 ssh
````

#### John The Ripper (attaque hors ligne)
````
john --wordlist=rockyou.txt hash.txt
````
### 6. Résumé comparatif


| Critère           | Hydra           | John The Ripper |
| ----------------- | --------------- | --------------- |
| Mode              | En ligne        | Hors ligne      |
| Cible             | Services réseau | Hashs           |
| Discrétion        | Faible          | Élevée          |
| Détection         | Facile          | Très difficile  |
| Dépendance réseau | Oui             | Non             |
