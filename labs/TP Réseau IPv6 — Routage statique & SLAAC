# TP Réseau IPv6 — Routage statique & SLAAC

---

## 1. Objectif du TP

L’objectif de ce TP est de :

* Mettre en place un **réseau IPv6 simple**
* Configurer l’**adressage IPv6** sur des routeurs
* Implémenter le **routage statique IPv6**
* Permettre aux hôtes d’obtenir automatiquement leurs adresses IPv6 grâce à **SLAAC**
* Vérifier la connectivité de bout en bout

---

## 2. Topologie du réseau

```
PC1 ── Switch1 ── R1 ── R2 ── Switch2 ── PC2
```

### Plan d’adressage IPv6

| Équipement | Interface | Adresse IPv6        | Rôle       |
| ---------- | --------- | ------------------- | ---------- |
| PC1        | NIC       | SLAAC               | Hôte LAN 1 |
| R1         | G0/0      | 2001:DB8:1:1::1/64  | LAN PC1    |
| R1         | G0/1      | 2001:DB8:12:1::1/64 | Lien R1–R2 |
| R2         | G0/0      | 2001:DB8:12:1::2/64 | Lien R1–R2 |
| R2         | G0/1      | 2001:DB8:2:1::1/64  | LAN PC2    |
| PC2        | NIC       | SLAAC               | Hôte LAN 2 |

---

## 3. Configuration IPv6 des routeurs

### Routeur R1

```bash
R1(config)# ipv6 unicast-routing

R1(config)# interface g0/0
R1(config-if)# ipv6 address 2001:DB8:1:1::1/64
R1(config-if)# no shutdown

R1(config)# interface g0/1
R1(config-if)# ipv6 address 2001:DB8:12:1::1/64
R1(config-if)# no shutdown
```

---

### Routeur R2

```bash
R2(config)# ipv6 unicast-routing

R2(config)# interface g0/0
R2(config-if)# ipv6 address 2001:DB8:12:1::2/64
R2(config-if)# no shutdown

R2(config)# interface g0/1
R2(config-if)# ipv6 address 2001:DB8:2:1::1/64
R2(config-if)# no shutdown
```

- La commande `ipv6 unicast-routing` est indispensable pour :

* Activer le routage IPv6
* Permettre l’envoi des **Router Advertisements (RA)**
* Activer **SLAAC** sur les réseaux LAN

---

## 4. Configuration du routage statique IPv6

### Sur R1 (vers le réseau de PC2)

```bash
R1(config)# ipv6 route 2001:DB8:2:1::/64 2001:DB8:12:1::2
```

### Sur R2 (vers le réseau de PC1)

```bash
R2(config)# ipv6 route 2001:DB8:1:1::/64 2001:DB8:12:1::1
```

- Le **next-hop** doit toujours être :

* L’adresse IPv6 du **routeur voisin**
* Jamais l’adresse IPv6 du routeur lui-même

---

## 5. Configuration SLAAC sur les PCs

### PC1 et PC2

* Mode IPv6 : **Auto Configuration (SLAAC)**
* Aucune configuration manuelle requise

Grâce aux Router Advertisements envoyés par les routeurs, chaque PC reçoit automatiquement :

* Une adresse IPv6 globale
* Le préfixe `/64`
* La passerelle par défaut (adresse link-local du routeur)

---

## 6. Vérification des configurations

### Sur les routeurs

```bash
show ipv6 interface brief
show ipv6 route
```

Les routes affichées :

* `C` → réseaux connectés
* `S` → routes statiques

---

### Sur les PCs

* Vérification de l’adresse IPv6 reçue automatiquement
* Vérification de la passerelle IPv6

---

## 7. Test de connectivité

### Depuis PC1

```bash
ping 2001:DB8:2:1::1
```

 Résultat attendu :

* Réponses reçues
* Connectivité IPv6 fonctionnelle entre les deux réseaux

---

## 8. Difficultés rencontrées et corrections

### Erreur :

```
% Not allowed to point static routes through yourself
```

### Cause :

* Le routeur était configuré avec **sa propre adresse IPv6 comme next-hop**

### Solution :

* Utiliser l’adresse IPv6 du **routeur voisin** sur le lien inter‑routeur

---

## 9. Conclusion

Ce TP a permis de comprendre concrètement :

* Le fonctionnement du **routage IPv6 statique**
* Le rôle central de `ipv6 unicast-routing`
* Le mécanisme **SLAAC** pour l’auto‑configuration des hôtes
* Les bonnes pratiques de configuration IPv6

- IPv6 simplifie les réseaux modernes tout en améliorant leur évolutivité et leur sécurité.

