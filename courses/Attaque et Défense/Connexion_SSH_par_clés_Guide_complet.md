# Connexion SSH par clés – Guide complet

Ce document récapitule **toutes les étapes pour générer une clé privée / clé publique SSH** et établir une connexion sécurisée à un serveur distant.

---

## 1. Prérequis

* Un client Linux / macOS / Kali (ou Windows avec OpenSSH)
* Un serveur SSH accessible
* Un compte utilisateur sur la machine distante

---

## 2. Génération de la paire de clés SSH (machine cliente)

### Méthode recommandée (ED25519)

```bash
ssh-keygen -t ed25519 -C "user@machine"
```

### Alternative (RSA 4096 bits)

```bash
ssh-keygen -t rsa -b 4096 -C "user@machine"
```

### Déroulement

1. Emplacement du fichier : appuyer sur **Entrée** pour le chemin par défaut
2. Passphrase : recommandée pour plus de sécurité

### Fichiers générés

* **Clé privée** : `~/.ssh/id_ed25519`
* **Clé publique** : `~/.ssh/id_ed25519.pub`

> ⚠️ La clé privée ne doit **jamais** être partagée.

---

## 3. Installation de la clé publique sur le serveur

### Méthode 1 – Automatique (recommandée)

```bash
ssh-copy-id user@IP_CIBLE
```

Ou avec une clé spécifique :

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@IP_CIBLE
```

La clé est ajoutée dans :

```
~/.ssh/authorized_keys
```

---

### Méthode 2 – Manuelle

#### Sur le client

```bash
cat ~/.ssh/id_ed25519.pub
```

Copier la clé publique.

#### Sur le serveur

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
```

Coller la clé publique puis :

```bash
chmod 600 ~/.ssh/authorized_keys
```

---

## 4. Permissions recommandées

### Côté client

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

---

## 5. Connexion SSH avec clé

### Connexion standard

```bash
ssh user@IP_CIBLE
```

### Forcer une clé spécifique

```bash
ssh -i ~/.ssh/id_ed25519 user@IP_CIBLE
```

---

## 6. Utilisation de ssh-agent (optionnel mais recommandé)

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

La passphrase est saisie une seule fois.

---

## 7. Dépannage (debug)

Connexion en mode verbeux :

```bash
ssh -v user@IP_CIBLE
```

Messages attendus :

* `Offering public key`
* `Authentication succeeded`

---

## 8. Sécurisation côté serveur (recommandée)

Fichier `/etc/ssh/sshd_config` :

```conf
PubkeyAuthentication yes
PasswordAuthentication no
PermitRootLogin no
```

Puis redémarrer SSH :

```bash
sudo systemctl restart ssh
```

---

## 9. Schéma de fonctionnement

```
CLIENT                         SERVEUR
ssh-keygen
  ├── clé privée  --------X--> (jamais envoyée)
  └── clé publique -----> ~/.ssh/authorized_keys

ssh user@IP  --> vérification clé --> accès accordé
```

---

## 10. Résumé

* `ssh-keygen` : génère la paire de clés
* `ssh-copy-id` : installe la clé publique
* `ssh -i` : force l'utilisation d'une clé
* Clé privée = secrète, clé publique = partageable


