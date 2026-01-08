# Protocole ICMP : notes de cours

## Définition
ICMP (Internet Control Message Protocol) est un protocole de la couche réseau (couche 3 OSI) utilisé pour :
- Diagnostiquer les problèmes réseau
- Signaler des erreurs
- Contrôler l’état des communications

> ICMP n’achemine pas de données utilisateur, il est utilisé uniquement pour le contrôle et les messages d’erreur.

---

## Rôles principaux d’ICMP
1. **Signaler une erreur**
   - Destination injoignable
   - TTL expiré
   - Fragmentation nécessaire mais interdite
   - Port inaccessible

2. **Tester la connectivité**
   - `ping` → Echo Request / Echo Reply
   - `traceroute` → Time Exceeded

---

## Structure d’un message ICMP
- **Type** : identifie le type de message (ex. 0, 8, 11…)
- **Code** : précise le type (ex. pour "destination unreachable")
- **Checksum** : contrôle d’erreur
- **Données supplémentaires**

---

## Types ICMP les plus connus

| Type | Code | Signification |
|------|------|---------------|
| 8    | 0    | Echo Request (ping) |
| 0    | 0    | Echo Reply (ping) |
| 3    | 0-15 | Destination Unreachable |
| 5    | 0-3  | Redirect (rediriger) |
| 11   | 0    | Time Exceeded (TTL expiré) |
| 12   | 0    | Parameter Problem |

---

## Exemple concret : traceroute vers google.com

### Commande
```bash
# Linux / Mac
traceroute google.com

# Windows
tracert google.com
```

### Résultat typique
```
1   192.168.1.1        2 ms
2   10.0.0.1           7 ms
3   154.72.40.1       19 ms
4   72.14.219.98      44 ms
5   142.250.37.18     61 ms
6   142.250.46.238    63 ms
7   142.251.36.174    65 ms   google.com
```

### Explication

- TTL = 1 → routeur 1 renvoie ICMP Time Exceeded
- TTL = 2 → routeur 2 renvoie ICMP Time Exceeded
- TTL = 3 → routeur 3 renvoie ICMP Time Exceeded
- … jusqu’à la destination finale, qui renvoie Echo Reply ou Port Unreachable

Traceroute utilise ICMP pour révéler chaque routeur sur le chemin jusqu’à la destination.

## Conclusion

- ICMP est essentiel pour le diagnostic et le contrôle réseau.
- Utilisé par ping et traceroute pour tester la connectivité et détecter les problèmes.
- Messages ICMP courants : Echo, Destination Unreachable, Time Exceeded.
