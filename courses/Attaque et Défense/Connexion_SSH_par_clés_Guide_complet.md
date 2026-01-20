# Connexion SSH par clé (Windows client → Kali serveur)

Ce document résume **ce qui se passe à chaque étape**, le **rôle de chaque commande**, et **pourquoi SSH peut demander encore un mot de passe**.

---
## Qu'est ce que SSH ?

Secure Socket Shell (SSH) est un protocole réseau qui permet d'accéder et de communiquer de manière sécurisée avec des machines distantes, principalement des serveurs distants. Cela signifie que vous pouvez vous connecter à un autre ordinateur directement depuis votre terminal. Une fois la connexion établie, toutes les commandes que vous saisissez dans votre terminal local sont exécutées sur le serveur distant.

## 1. Architecture du scénario

- **Machine cliente** : Windows  
- **Machine serveur** : Kali Linux  
- **Service** : OpenSSH  
- **Méthode d’authentification** : clé asymétrique (ed25519)

---

## 2. Démarrage du serveur SSH (Kali)

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
sudo systemctl status ssh
```

### Ce qui se passe :
- Le service SSH écoute sur le port 22
- Kali est prêt à accepter des connexions distantes

---

## 3. Génération de la clé SSH (Windows)

```powershell
ssh-keygen -t ed25519 -C "windows@client"
```

### Ce qui se passe :
- Une **clé privée** est créée (reste sur Windows)
- Une **clé publique** est créée (sera copiée sur Kali)

Emplacement :
```
C:\Users\USER\.ssh\
├── id_ed25519      (clé privée)
└── id_ed25519.pub  (clé publique)
```

---

## 4. Copie de la clé publique vers Kali

### Affichage de la clé publique (Windows)

```powershell
type $env:USERPROFILE\.ssh\id_ed25519.pub
```

### Installation sur Kali

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
```

- Coller **UNE seule ligne** (clé publique)

```bash
chmod 600 ~/.ssh/authorized_keys
chown -R kali:kali ~/.ssh
```

### Ce qui se passe :
- Kali enregistre la clé autorisée pour l’utilisateur `kali`
- SSH saura reconnaître le client par sa clé privée

---

## 5. Tentative de connexion SSH

```bash
ssh kali@192.168.56.101
```

### Cas 1 — Succès par clé
- SSH trouve la clé
- Authentification automatique
- Pas de mot de passe système

### Cas 2 — Mot de passe demandé
- La clé est **proposée mais refusée**
- SSH bascule vers l’authentification par mot de passe

---

## 6. Analyse avec le mode debug

```bash
ssh -v kali@192.168.56.101
```

### Ligne clé à analyser

```text
Offering public key ...
Server accepts key
Authentication succeeded (publickey)
```

- Si au lieu de ça :

```text
Authentications that can continue: publickey,password
```

- La clé est refusée (permissions, clé incorrecte, mauvais utilisateur)

---

## 7. Causes fréquentes du refus de clé

- Mauvaise clé dans `authorized_keys`
- Permissions incorrectes
- Mauvais propriétaire du fichier
- Mauvais utilisateur SSH
- SSHD mal configuré

---

## 8. Vérification de la configuration SSH (Kali)

```bash
sudo nano /etc/ssh/sshd_config
```

Lignes requises :

```ini
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication yes
```

Redémarrage :

```bash
sudo systemctl restart ssh
```

---

## 9. Sécurisation finale (après test réussi)

```ini
PasswordAuthentication no
```

- Empêche toute connexion par mot de passe

---

## 10. Résumé sécurité (niveau professionnel)

- SSH utilise la cryptographie asymétrique
- Le serveur ne stocke **jamais la clé privée**
- Une seule permission incorrecte = clé refusée
- `ssh -v` est l’outil standard de diagnostic

---

## 11. Conclusion

Si SSH demande encore un mot de passe :
> La clé n’est pas acceptée par le serveur.

La correction passe **toujours** par :
1. Clé correcte
2. Permissions strictes
3. Bon utilisateur
4. Debug SSH
