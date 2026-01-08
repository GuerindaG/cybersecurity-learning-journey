# Comprendre le fonctionnement du DNS

Le **DNS (Domain Name System)** est le système qui permet de traduire les **noms de domaine** compréhensibles par l'homme (ex : `www.exemple.com`) en **adresses IP** que les ordinateurs peuvent utiliser pour communiquer sur Internet.

---

## 1. Qu’est-ce que le DNS ?

Le DNS agit comme un **annuaire d’Internet**. Chaque fois que vous tapez un nom de domaine dans votre navigateur, votre ordinateur ne comprend que les **adresses IP**, donc il doit interroger le DNS pour obtenir l’adresse correspondante.

Le DNS repose sur plusieurs types de **serveurs** hiérarchiques :

- **Serveurs racine (Root DNS Servers)** : dirigent les requêtes vers le bon domaine de premier niveau (TLD).  
- **Serveurs TLD (Top Level Domain Servers)** : spécialisés par extension (.com, .org, .bj, etc.), ils dirigent vers les serveurs autoritaires du domaine.  
- **Serveurs DNS autoritaires (Authoritative DNS Servers)** : contiennent les informations exactes sur le domaine, comme l’adresse IP du site, les serveurs de messagerie, etc.

---

## 2. Étapes détaillées d’une requête DNS

1. **Saisie du nom de domaine** :  
   Vous tapez `www.exemple.com` dans votre navigateur.

2. **Vérification du cache local** :  
   Votre ordinateur vérifie s’il a déjà l’adresse IP dans son **cache DNS**.  
   - Si oui : connexion directe.  
   - Si non : la requête est envoyée au **DNS Resolver**.

3. **Interrogation du DNS Resolver** :  
   Le **DNS Resolver** (souvent fourni par votre fournisseur d’accès Internet ou Google DNS / Cloudflare) commence la recherche hiérarchique :
   
   - **Serveur racine** : « Je ne connais pas l’IP exacte, mais pour le domaine `.com`, interrogez les serveurs TLD .com ».  
   - **Serveur TLD (.com)** : « Pour `exemple.com`, interrogez les **serveurs DNS autoritaires** de ce domaine ».  
   - **Serveur DNS autoritaire** : « L’adresse IP de `www.exemple.com` est 192.168.10.20 ».

4. **Transmission de la réponse** :  
   Le **DNS Resolver** renvoie l’adresse IP à votre ordinateur.  
   Votre ordinateur peut maintenant se connecter au serveur web correspondant.

---

## 3. Les principaux enregistrements DNS

| Type | Fonction |
|------|----------|
| **A** | Associe un nom à une adresse IPv4 |
| **AAAA** | Associe un nom à une adresse IPv6 |
| **CNAME** | Alias vers un autre domaine |
| **MX** | Serveurs de messagerie |
| **NS** | Serveurs DNS autoritaires du domaine |
| **TXT** | Informations diverses (SPF, vérification de domaine) |

---

## 4. Exemple concret : scénario réel

Imaginons que vous tapez `www.monblog.bj` dans votre navigateur :

1. Votre ordinateur vérifie le **cache DNS local** → aucune réponse.  
2. Il contacte le **DNS Resolver** de votre fournisseur d’accès.  
3. Le **DNS Resolver** interroge un **serveur racine** → « Pour `.bj`, allez voir le serveur TLD `.bj` ».  
4. Le **serveur TLD .bj** répond → « Les **serveurs DNS autoritaires** pour `monblog.bj` sont ns1.monblog.bj et ns2.monblog.bj ».  
5. Le **DNS Resolver** contacte `ns1.monblog.bj` → « L’adresse IP de `www.monblog.bj` est 102.54.32.10 ».  
6. Votre navigateur reçoit l’adresse IP et se connecte au serveur web.

**En résumé** : grâce au DNS, vous pouvez accéder à un site web en utilisant un nom simple, alors que votre ordinateur utilise une adresse IP pour se connecter.

---

## 5. Commandes pratiques (Linux / Windows)

- `nslookup www.exemple.com` : interroger un serveur DNS  
- `dig www.exemple.com` : obtenir une réponse détaillée du DNS  
- `ping www.exemple.com` : tester la connexion via l’IP obtenue

---

*Le DNS est donc une infrastructure essentielle qui permet la navigation sur Internet de manière simple et efficace.*
