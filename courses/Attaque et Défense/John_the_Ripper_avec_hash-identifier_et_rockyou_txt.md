# Utilisation de John the Ripper avec hash-identifier et rockyou.txt

## 1. Contexte et objectif

Cette note décrit, de manière pédagogique et structurée, l’utilisation conjointe de **hash-identifier**, **John the Ripper** et de la wordlist **rockyou.txt** dans un cadre **strictement éducatif** (formation, laboratoire, CTF).

**Objectif principal :**

* Identifier le type d’un hash
* Tenter de retrouver le mot de passe correspondant à l’aide d’une attaque par dictionnaire

---

## 2. Présentation des outils

### 2.1 John the Ripper

John the Ripper (souvent appelé *John*) est un outil de cassage de mots de passe fonctionnant en ligne de commande.

**Fonctions principales :**

* Comparer des hashes à des mots de passe potentiels
* Support de nombreux formats (MD5, SHA, hashes Linux, Windows, etc.)

**Limites :**

* Ne garantit pas le succès
* Dépend fortement du dictionnaire utilisé

---

### 2.2 hash-identifier

hash-identifier est un outil permettant d’identifier le **type probable d’un hash**.

**Rôle clé :**

* Orienter le choix de l’outil ou du format à utiliser
* Éviter les erreurs de type de hash lors de l’analyse

---

### 2.3 rockyou.txt

rockyou.txt est une **wordlist** (liste de mots de passe courants).

**Utilité :**

* Utilisée dans les attaques par dictionnaire
* Contient des millions de mots de passe réels issus de fuites publiques

---

## 3. Installation des outils sur Kali Linux

### 3.1 Installation de John the Ripper

```bash
sudo apt update
sudo apt install john -y
```

Vérification :

```bash
john --version
```

---

### 3.2 Installation de hash-identifier

```bash
cd ~
git clone https://gitlab.com/kalilinux/packages/hash-identifier
```

---

### 3.3 Installation de rockyou.txt

```bash
cd /usr/share/wordlists
sudo git clone https://github.com/RykerWilder/rockyou.txt
```

---

## 4. Méthodologie d’utilisation (workflow)

L’utilisation des outils suit un ordre logique précis :

1. Identification du hash
2. Préparation du fichier de hash
3. Attaque par dictionnaire avec John
4. Analyse des résultats

---

## 5. Étape 1 – Identifier le type de hash

Lancement de hash-identifier :

```bash
cd ~/hash-identifier
python3 hash-id.py
```

Entrer le hash lorsqu’il est demandé :

```text
HASH: 5f4dcc3b5aa765d61d8327deb882cf99
```

Résultat possible :

```text
Possible Hashes:
[+] MD5
```

**Conclusion :** le hash est probablement de type MD5.

---

## 6. Étape 2 – Préparer le fichier de hash

John the Ripper lit les hashes depuis un fichier texte.

Création du fichier :

```bash
nano hash.txt
```

Contenu du fichier :

```text
5f4dcc3b5aa765d61d8327deb882cf99
```

Sauvegarde :

* CTRL + O
* Entrée
* CTRL + X

---

## 7. Étape 3 – Attaque par dictionnaire avec John

Commande standard :

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt/rockyou.txt hash.txt
```

**Explication :**

* `--wordlist` : dictionnaire utilisé
* `hash.txt` : fichier contenant le hash à analyser

---

## 8. Étape 4 – Affichage des résultats

Si un mot de passe est trouvé :

```bash
john --show hash.txt
```

Exemple de sortie :

```text
5f4dcc3b5aa765d61d8327deb882cf99:password
```

---

## 9. Problèmes fréquents et solutions

### 9.1 Aucun mot de passe trouvé

Causes possibles :

* Le mot de passe n’est pas dans rockyou.txt
* Hash incorrect ou incomplet
* Hash salé (salt)

---

### 9.2 Erreur liée à rockyou.txt

Vérifier la présence du fichier :

```bash
ls /usr/share/wordlists/rockyou.txt
```

Si le fichier est compressé :

```bash
sudo gunzip rockyou.txt.gz
```

---

## 10. Aspects légaux et éthiques

L’utilisation de John the Ripper est **strictement encadrée** :

Utilisation autorisée :

* Formation académique
* Laboratoires (TryHackMe, Hack The Box)
* CTF

Utilisation interdite :

* Comptes réels
* Systèmes sans autorisation

---

## 11. Résumé synthétique

```text
Hash → hash-identifier → Type de hash
Type de hash → John + rockyou.txt → Mot de passe (si existant)
```

---

## 12. Conclusion

L’utilisation combinée de **hash-identifier**, **John the Ripper** et **rockyou.txt** permet de comprendre concrètement :

* Le fonctionnement des hashes
* Les attaques par dictionnaire
* Les limites de la sécurité basée uniquement sur des mots de passe faibles

Cette méthodologie constitue une base essentielle en cybersécurité offensive et défensive.
