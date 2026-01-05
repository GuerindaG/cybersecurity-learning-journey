# Port Security – Notions et Configuration (Cisco)

## 1. Définition
Le **Port Security** est une fonctionnalité des switchs (Cisco) qui permet de :
- limiter le nombre d’adresses MAC autorisées sur un port
- autoriser uniquement des équipements connus
- bloquer ou alerter en cas de connexion non autorisée

 **Objectif** : empêcher les intrusions, le branchement de PC pirates et les attaques MAC flooding.

---

## 2. Principe de fonctionnement
- Le port surveille les **adresses MAC**
- Une MAC non autorisée = **violation**
- Une action est déclenchée selon le mode configuré

---

## 3. Conditions d’utilisation
- Fonctionne uniquement sur des **ports ACCESS**
- S’applique **port par port**
- Non recommandé sur les ports vers d’autres switchs ou routeurs

---

## 4. Types d’adresses MAC
| Type | Description |
|----|----|
| Statique | MAC définie manuellement |
| Dynamique | MAC apprise automatiquement (perdue au reboot) |
| Sticky | MAC apprise automatiquement et sauvegardée |

---

## 5. Modes de violation
| Mode | Comportement |
|----|----|
| protect | Bloque silencieusement |
| restrict | Bloque + log + compteur |
| shutdown | Désactive le port (par défaut) |

---

## 6. Configuration pas à pas (Cisco)

### Étape 1 : Accès à l’interface

### Étape 1 : Accès à l’interface
```bash
Switch(config)# interface fastEthernet0/1
```

### Étape 2 : Mode access

`Switch(config-if)# switchport mode access
`
### Étape 3 : Activation du Port Security

`Switch(config-if)# switchport port-security`
 ### Étape 4 : Limite du nombre de MAC
`Switch(config-if)# switchport port-security maximum 1
`
### Étape 5 : Définir les MAC autorisées
**MAC manuelle**
`Switch(config-if)# switchport port-security mac-address 00:11:22:33:44:55
`
**MAC sticky**
`Switch(config-if)# switchport port-security mac-address sticky
`
### Étape 6 : Mode de violation
`Switch(config-if)# switchport port-security violation shutdown
`
### Étape 7 : Sauvegarde
`Switch# write memory
`
## 7. Vérification
`
show port-security interface fa0/1
show port-security address
show interface status err-disabled
`
## 8. Réactivation d’un port bloqué
`interface fa0/1
shutdown
no shutdown
`** Ou automatique :**
`errdisable recovery cause psecure-violation
errdisable recovery interval 30
`
## 9. Bonnes pratiques
- 1 PC = 1 port = 1 MAC
- Utiliser sticky en production
- Utiliser manuel en examen
- Toujours sauvegarder la configuration

