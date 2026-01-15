# John the Ripper – TP

## 1. Objectif du lab
Ce lab a pour objectif de :
- Identifier différents types de hash
- Utiliser John the Ripper pour casser des mots de passe
- Comprendre les erreurs courantes liées aux formats et aux options
- Afficher correctement les mots de passe crackés

---

## 2. Environnement de travail
- Système d’exploitation : Kali Linux
- Outil : John the Ripper (Jumbo)
- Wordlist : rockyou.txt
- Fichiers de hash : hash1.txt, hash2.txt, hash3.txt, hash4.txt

---

## 3. Commandes utilisées et explications (Difficultés et solutions)

### 3.1 Détection automatique du type de hash
```bash
john hash1.txt
```

**Description :**
- John tente de détecter automatiquement le type de hash
- Lance une attaque par défaut

**Résultat :**
- Hashes correctement chargés
- Aucun mot de passe trouvé avec cette méthode

---

### 3.2 Erreur de casse dans l’option wordlist

Commande incorrecte :
```bash
john --format=Raw-MD5 --Wordlist=/home/kali/Downloads/rockyou.txt-main/rockyou.txt hash1.txt
```

**Erreur :**
```
Unknown option: "--Wordlist=..."
```

**Cause :**
- John the Ripper est sensible à la casse
- L’option correcte est `--wordlist` en minuscules

---

### 3.3 Commande corrigée pour un hash MD5
```bash
john --format=Raw-MD5 --wordlist=/home/kali/Downloads/rockyou.txt-main/rockyou.txt hash1.txt
```

**Résultat :**
```
biscuit (?)
```

- Le hash MD5 a été cassé avec succès
- Mot de passe trouvé : **biscuit**

---

### 3.4 Problème lors de l’affichage avec --show
```bash
john --show hash1.txt
```

**Résultat :**
```
0 password hashes cracked, 2 left
```

**Explication :**
- John n’utilise pas le même format lors de l’affichage
- Le format doit être précisé manuellement

---

### 3.5 Affichage correct des mots de passe MD5
```bash
john --show --format=Raw-MD5 hash1.txt
```

**Résultat attendu :**
```
hash: biscuit
```

---

### 3.6 Erreur sur le format SHA-1

 Commande incorrecte :
```bash
john --format=SHA-1 --wordlist=/home/kali/Downloads/rockyou.txt-main/rockyou.txt hash2.txt
```

**Erreur :**
```
Unknown ciphertext format name requested
```

**Cause :**
- Le format `SHA-1` n’existe pas sous ce nom dans John

---

### 3.7 Format SHA-1 correct
```bash
john --format=Raw-SHA1 --wordlist=/home/kali/Downloads/rockyou.txt-main/rockyou.txt hash2.txt
```

**Explication :**
- `Raw-` indique un hash simple sans sel
- `SHA1` correspond à l’algorithme utilisé

---

### 3.8 Affichage des résultats SHA-1
```bash
john --show --format=Raw-SHA1 hash2.txt
```

---

## 4. Bonnes pratiques à retenir
- John est sensible à la casse (options et formats)
- Le format utilisé pour `--show` doit correspondre au format utilisé pour le crack
- Toujours identifier le type de hash avant de forcer un format
- Les messages d’erreur de John sont des indications utiles

---

## 5. Conclusion
Ce lab met en évidence l’importance du bon choix de format et de la rigueur dans l’utilisation de John the Ripper. Même lorsqu’un mot de passe est cracké avec succès, une mauvaise commande d’affichage peut donner l’impression d’un échec.

Les hashes ont été correctement analysés et cassés à l’aide des formats appropriés, démontrant l’efficacité de John the Ripper lorsqu’il est utilisé correctement.
