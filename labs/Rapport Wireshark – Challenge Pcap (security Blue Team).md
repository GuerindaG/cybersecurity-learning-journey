# Rapport d’analyse de trafic réseau – Wireshark

- **Challenge : PCAP 1 & PCAP 2**
- **Plateforme : Security Blue Team**
- **Outil principal : Wireshark**

---

## 1. Introduction

Dans le cadre d’un challenge pratique proposé par la plateforme **Security Blue Team**, j’ai été amenée à analyser deux fichiers de capture réseau (PCAP 1 et PCAP 2) à l’aide de Wireshark.

L’objectif de cet exercice était d’identifier des informations clés à partir du trafic réseau, notamment :

* les protocoles utilisés,
* les hôtes impliqués,
* les ports d’accès,
* les fichiers sensibles transférés,
* ainsi que des éléments liés à une activité malveillante.

Ce rapport présente **ma démarche réelle**, **mes maladresses**, **mes difficultés**, et **ce que j’ai appris à chaque étape**, en me basant directement sur les questions du challenge.

---

## 2. Environnement et contexte

* Type d’analyse : Analyse PCAP (post‑mortem)
* Outil : Wireshark
* Type de trafic observé : ICMP, DNS, SSDP, SMB, FTP
* Systèmes impliqués :

  * Hôte Windows (IE.WIN7 – Windows 7 Enterprise)
  * Attaquant utilisant un serveur FTP

---

## 3. Analyse PCAP 1

### 3.1 Quel protocole a été utilisé sur le port 3942 ?

**Démarche :**

* J’ai commencé par filtrer le trafic en utilisant :

  ```
  tcp.port == 3942 || udp.port == 3942
  ```
* J’ai ensuite observé la colonne **Protocol** et le contenu des paquets associés.

**Résultat :**

* Le protocole utilisé sur le port **3942** est **SSDP**.

**Maladresse / difficulté rencontrée :**

* Au départ, je pensais qu’un port correspondait toujours à un protocole standard.
* J’ai eu du mal à comprendre qu’un protocole comme SSDP pouvait utiliser un port non conventionnel.

---

### 3.2 Quelle est l’adresse IP de l’hôte qui a été pingée deux fois ?

**Démarche :**

* Filtrage ICMP :

  ```
  icmp
  ```
* J’ai identifié les paquets **Echo (ping) request** dans la colonne *Info*.
* J’ai compté les requêtes ICMP ayant la même **adresse IP de destination**.

**Résultat :**

* L’hôte pingé deux fois correspond à l’adresse IP apparaissant deux fois comme destination des Echo Request.

**Maladresse / difficulté rencontrée :**

* Je confondais au départ requêtes et réponses ICMP.
* Je regardais l’adresse IP source au lieu de l’adresse IP destination.

---

### 3.3 Combien de paquets de réponse de requête DNS ont été capturés ?

**Démarche :**

* Filtrage DNS :

  ```
  dns
  ```
* Identification des paquets marqués comme **Standard query response**.
* Comptage des réponses DNS.

**Maladresse / difficulté rencontrée :**

* J’avais du mal à distinguer une requête DNS d’une réponse DNS.
* J’ai appris à vérifier le champ *Flags* pour confirmer qu’il s’agissait bien d’une réponse.

---

### 3.4 Quelle est l’adresse IP de l’hôte qui a envoyé le plus grand nombre d’octets ?

**Démarche :**

* Menu :

  ```
  Statistics → Conversations → IPv4
  ```
* Tri de la colonne **Bytes A → B** par ordre décroissant.

**Résultat :**

* L’adresse IP située en tête du tableau est celle ayant envoyé le plus grand nombre d’octets.

**Maladresse / difficulté rencontrée :**

* Au début, je regardais le nombre de paquets au lieu du nombre d’octets.
* Je ne connaissais pas encore le menu *Statistics*, ce qui m’a fait perdre du temps.

---

## 4. Analyse PCAP 2

### 4.1 Qu’est-ce que le mot de passe WebAdmin ?

**Démarche :**

* Analyse des protocoles non chiffrés.
* Observation du trafic FTP/HTTP où les identifiants sont transmis en clair.

**Maladresse / difficulté rencontrée :**

* Je pensais initialement que Wireshark pouvait toujours révéler des mots de passe.
* J’ai compris que cela n’est possible que si le protocole n’est pas chiffré.

---

### 4.2 Quel est le numéro de version du serveur FTP de l’attaquant ?

**Démarche :**

* Filtrage FTP :

  ```
  ftp
  ```
* Lecture de la bannière de connexion FTP.

**Résultat :**

```
pyftpdlib 1.5.5
```

**Maladresse / difficulté rencontrée :**

* Je ne savais pas au départ que la version du serveur est affichée dès la réponse `220`.

---

### 4.3 Quel port a été utilisé pour accéder à l’hôte Windows victime ?

**Démarche :**

* Sélection d’un paquet FTP.
* Dans *Packet Details*, déploiement de **Transmission Control Protocol**.
* Identification du **Destination Port**.

**Résultat :**

```
21
```

**Maladresse / difficulté rencontrée :**

* Je confondais port source et port destination.
* Je ne savais pas où cliquer dans le panneau *Packet Details*.

---

### 4.4 Quel est le nom d’un fichier confidentiel qui se trouve sur l’hôte Windows ?

**Démarche :**

* Filtrage FTP.
* Analyse des commandes FTP `RETR`.

**Résultat :**

```
malware.exe
```

**Maladresse / difficulté rencontrée :**

* J’ai d’abord cherché le fichier via SMB (NTLMSSP, Negotiate).
* Je ne comprenais pas que l’authentification SMB ne montre pas les fichiers.

---

### 4.5 Quel est le nom du fichier journal créé à 04:51 sur l’hôte Windows ?

**Démarche :**

* Modification du format de temps :

  ```
  View → Time Display Format → Date and Time of Day
  ```
* Recherche des fichiers `.log` autour de 04:51.
* Analyse du trafic FTP (STOR / RETR).

**Maladresse / difficulté rencontrée :**

* Je cherchais une “heure de création Windows”, inexistante dans Wireshark.
* J’analysais des paquets TCP bruts en hexadécimal, sans protocole applicatif.
* J’ai compris que la question se base sur l’heure du trafic réseau.

---

## 5. Difficultés générales rencontrées

* Lecture et interprétation du trafic binaire
* Confusion entre négociation, authentification et action réelle
* Mauvaise interprétation du temps dans Wireshark
* Recherche des fichiers dans le mauvais protocole
* Manque de méthode au début de l’analyse

---

## 6. Compétences acquises

* Utilisation avancée des filtres Wireshark
* Analyse ICMP, DNS, SMB et FTP
* Identification de fichiers sensibles transférés en clair
* Compréhension des risques liés aux protocoles non chiffrés
* Méthodologie d’analyse PCAP orientée Blue Team

---

## 7. Conclusion

Ce challenge m’a permis de progresser significativement en analyse de trafic réseau. Les difficultés rencontrées ont été essentielles pour comprendre la logique réelle d’une investigation Blue Team. J’ai désormais une meilleure approche méthodologique pour analyser un PCAP et extraire des preuves exploitables.

---

**Fin du rapport**
