# Création de sous-réseaux avec VLSM  
## Réseau de base : 10.0.0.0/24

## Objectif
Créer deux sous-réseaux à partir du réseau **10.0.0.0/24** :
- Un sous-réseau pour **60 hôtes**
- Un sous-réseau pour **30 hôtes**

La méthode utilisée est le **VLSM (Variable Length Subnet Mask)**.

---

## Rappels importants
- Formule pour les hôtes :
  2^h - 2 ≥ nombre d’hôtes
- CIDR = 32 - h
- On commence toujours par le besoin en hôtes **le plus élevé**

---

## Sous-réseau 1 : 60 hôtes

### Calcul
- 2^6 - 2 = 62 ≥ 60  
- h = 6  
- CIDR = 32 - 6 = **/26**

### Informations réseau
- Adresse réseau : **10.0.0.0/26**
- Masque : **255.255.255.192**
- Pas (incrément) : 256 - 192 = **64**

### Plage d’adresses
- Adresse réseau : 10.0.0.0
- Première adresse utilisable : 10.0.0.1
- Dernière adresse utilisable : 10.0.0.62
- Adresse de broadcast : 10.0.0.63
- Nombre d’hôtes utilisables : **62**

---

## Sous-réseau 2 : 30 hôtes

### Calcul
- 2^5 - 2 = 30 ≥ 30  
- h = 5  
- CIDR = **/27**

### Informations réseau
- Adresse réseau : **10.0.0.64/27**
- Masque : **255.255.255.224**
- Pas (incrément) : 256 - 224 = **32**

### Plage d’adresses
- Adresse réseau : 10.0.0.64
- Première adresse utilisable : 10.0.0.65
- Dernière adresse utilisable : 10.0.0.94
- Adresse de broadcast : 10.0.0.95
- Nombre d’hôtes utilisables : **30**

---

## Résumé global

| Sous-réseau | CIDR | Masque | Plage utilisable |
|------------|------|--------|-----------------|
| 60 hôtes | /26 | 255.255.255.192 | 10.0.0.1 – 10.0.0.62 |
| 30 hôtes | /27 | 255.255.255.224 | 10.0.0.65 – 10.0.0.94 |

---

## Adresses restantes
- Début : **10.0.0.96**
- Fin : **10.0.0.255**
- Total restant : **160 adresses**

Ces adresses peuvent être utilisées pour créer d’autres sous-réseaux selon les besoins futurs.

---

## Points clés à retenir
- Toujours trier les sous-réseaux par nombre d’hôtes décroissant
- Toujours vérifier avec la formule 2^h - 2
- Le prochain sous-réseau commence juste après l’adresse de broadcast du précédent
