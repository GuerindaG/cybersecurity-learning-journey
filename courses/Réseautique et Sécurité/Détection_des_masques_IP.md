
# Détection des masques IP : notes de cours

## 1. Adresse IP
Une adresse IP identifie un équipement sur un réseau.

Exemple : 192.168.1.1

 Elle est composée :
- d’une **partie réseau**
- d’une **partie hôte**

---

## 2. Masque de sous-réseau
Le masque permet de savoir :
- quelle partie de l’IP correspond au réseau
- combien d’hôtes sont possibles

Exemple : 255.255.255.0 → /24

---

## 3. Méthode rapide : classes d’adresses 

| Premier octet | Classe | Masque par défaut |
|---|---|---|
| 1 – 126 | A | 255.0.0.0 (/8) |
| 128 – 191 | B | 255.255.0.0 (/16) |
| 192 – 223 | C | 255.255.255.0 (/24) |

---

## 4. Application directe

### Exemple 1 : 192.168.1.1
➡ Classe C  
➡ Masque : `255.255.255.0 (/24)`

---

### Exemple 2 : 10.0.0.1
➡ Classe A  
➡ Masque : `255.0.0.0 (/8)`

---

### Exemple 3 : 172.20.5.10
➡ Classe B  
➡ Masque : `255.255.0.0 (/16)`

---

## 5. IP privées à retenir
| Réseau privé | Masque par défaut |
|---|---|
| 10.0.0.0 | /8 |
| 172.16.0.0 – 172.31.0.0 | /16 |
| 192.168.0.0 | /24 |

---

## 6. CIDR (Classless Inter-Domain Routing): notation moderne
C'est une méthode de gestion des adresses IP qui permet une allocation plus flexible et 
efficace des blocs d'adresses, remplaçant le système de classes d'adresses rigides (A, B, C) 
pour économiser les adresses IPv4 et simplifier le routage sur Internet. 
Il utilise une notation concise (ex: 192.168.0.0/16) où le nombre après le slash (/) 
indique le nombre de bits dédiés à la partie réseau, permettant de définir des sous-réseaux de tailles variées (subnetting) 
et de regrouper des réseaux contigus (supernetting) pour réduire la taille des tables de routage. 


Correspondance rapide :

| CIDR | Masque |
|---|---|
| /24 | 255.255.255.0 |
| /25 | 255.255.255.128 |
| /26 | 255.255.255.192 |
| /27 | 255.255.255.224 |
| /28 | 255.255.255.240 |
| /29 | 255.255.255.248 |
| /30 | 255.255.255.252 |

---

## 7. Astuce mémo
> **192 → /24**  
> **172 → /16**  
> **10 → /8**

---

## 8. Rappel important
 Le masque n’est pas contenu dans l’IP.  
Il est soit :
- donné explicitement (/xx)
- choisi par l’administrateur réseau



