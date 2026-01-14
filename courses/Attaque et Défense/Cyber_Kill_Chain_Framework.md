# CYBER KILL CHAIN
## Comprendre, analyser et contrer une cyberattaque

---

## 1. Introduction

La **Cyber Kill Chain** est un modèle conceptuel utilisé en cybersécurité pour décrire les **étapes successives d’une cyberattaque**, 
depuis la préparation de l’attaquant jusqu’à l’atteinte de son objectif final. Ce modèle a été proposé par **Lockheed Martin**, en s’inspirant du concept militaire de *kill chain*.

### Objectifs principaux
- Comprendre le **cycle de vie d’une attaque**
- Identifier les **points de détection et de blocage**
- Aider les équipes **Blue Team, Red Team et GRC**
- Améliorer la **réponse aux incidents**

---

## 2. Principe fondamental

> **Si une seule étape de la Kill Chain est interrompue, l’attaque échoue.**

La stratégie défensive consiste donc à **casser la chaîne le plus tôt possible**.

---

## 3. Les 7 étapes de la Cyber Kill Chain

---

### 1. Reconnaissance

#### Objectif de l’attaquant
Collecter des informations sur la cible afin de préparer l’attaque.

#### Exemples
- Recherche d’adresses IP et de domaines
- Collecte d’emails et de profils LinkedIn
- OSINT (Open Source Intelligence)
- Scan réseau passif

#### Contre-mesures défensives
- Sensibilisation à la divulgation d’informations
- Surveillance des fuites de données
- Réduction de la surface d’attaque
- Monitoring OSINT

---

### 2. Weaponization (Armement)

#### Objectif
Créer ou adapter une arme numérique prête à être utilisée contre la cible.

#### Exemples
- Malware + exploit
- Document Office avec macro malveillante
- PDF piégé
- Payload personnalisé

#### Contre-mesures
- Antivirus et sandboxing
- Blocage des macros
- Analyse comportementale
- Filtrage des fichiers suspects

---

### 3. Delivery (Livraison)

#### Objectif
Acheminer l’arme jusqu’à la victime.

#### Exemples
- Email de phishing
- Lien malveillant
- Site web compromis
- Clé USB infectée

#### Contre-mesures
- Filtrage des emails
- Anti-spam et anti-phishing
- Proxy web
- Formation des utilisateurs

---

### 4. Exploitation

#### Objectif
Exploiter une vulnérabilité pour exécuter du code malveillant.

#### Exemples
- Vulnérabilité logicielle non corrigée
- Mauvaise configuration système
- Failles applicatives (OWASP Top 10)

#### Contre-mesures
- Patching régulier
- Hardening des systèmes (consiste à réduire la surface d'attaque d'un système d'information (serveur, poste de travail, réseau, application) en désactivant les services inutiles)
- WAF (Web Application Firewall)
- Tests de vulnérabilités

---

### 5. Installation

#### Objectif
Installer un accès persistant dans le système compromis.

#### Exemples
- Backdoor
- Trojan (cheval de troie)
- Création de comptes cachés
- Services persistants

#### Contre-mesures
- EDR / XDR
- Contrôle d’intégrité des fichiers
- Surveillance des changements système
- Gestion stricte des privilèges

---

## 6. Limites de la Cyber Kill Chain

- Modèle linéaire
- Peu adapté aux menaces internes
- Moins précis pour les attaques cloud modernes

### Modèles complémentaires
- MITRE ATT&CK
- Diamond Model
- Unified Kill Chain

---

## 7. Cyber Kill Chain vs MITRE ATT&CK

| Critère | Cyber Kill Chain | MITRE ATT&CK |
|------|-----------------|-------------|
| Type | Modèle stratégique | Modèle tactique |
| Structure | Linéaire | Matricielle |
| Usage | Vision globale | Détection détaillée |
| Public | GRC / SOC | SOC / Red & Blue Team |

---

## 8. Points clés à retenir

- La Cyber Kill Chain décrit le **cycle complet d’une attaque**
- Elle aide à **prévenir, détecter et répondre** aux menaces
- Le but est de **bloquer l’attaque le plus tôt possible**
- Elle est souvent combinée avec **MITRE ATT&CK**

---

## 9. Conclusion

La Cyber Kill Chain est un **outil fondamental en cybersécurité**, servant de pont entre la **technique, l’opérationnel et la gouvernance**. Bien comprise, elle permet d’améliorer significativement la posture de sécurité d’une organisation.

---
