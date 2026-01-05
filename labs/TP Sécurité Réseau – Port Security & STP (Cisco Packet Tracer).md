# TP Sécurité Réseau – Port Security & STP (Cisco Packet Tracer)

## 1. Contexte du TP
Ce TP a pour objectif de mettre en œuvre les mécanismes de **sécurité de couche 2** sur un switch Cisco, notamment :
- Port Security (MAC sticky, MAC manuelle)
- Modes de violation (shutdown, restrict, protect)
- Observation du comportement du switch face à un PC non autorisé (PC hacker)
- Notions liées à STP (PortFast, BPDU Guard)

La simulation a été réalisée à l’aide de **Cisco Packet Tracer**.

---

## 2. Topologie du réseau
Le réseau est constitué de :
- 1 switch Cisco
- Plusieurs PC appartenant au même réseau `192.168.1.0/24`
- Un PC simulant un attaquant (PC hacker)

Chaque port du switch possède une configuration **Port Security différente** :
- MAC sticky avec `violation shutdown`
- MAC sticky avec `violation restrict`
- MAC manuelle avec `violation protect`

---

## 3. Objectifs du TP
- Comprendre le fonctionnement réel de Port Security
- Observer l’apprentissage des adresses MAC
- Tester les différents modes de violation
- Vérifier l’impact d’un équipement non autorisé
- Comprendre l’importance de la sauvegarde de configuration

---

## 4. Configuration réalisée

### Exemple de configuration Port Security
```bash
interface fa0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation shutdown
```

## 5. Tests effectués
### 5.1 Apprentissage des MAC sticky

- Les PC légitimes ont été branchés
- Un ping a été effectué pour générer du trafic
- Les adresses MAC ont été apprises automatiquement

**Commande de vérification :**
`show port-security address
`
## 5.2 Test avec PC hacker

- Le PC légitime a été débranché
- Le PC hacker a été branché sur le port Fa0/1
- Une MAC différente a été détectée

Résultat :

- Le port est passé en err-disabled
- Le lien est devenu rouge dans Packet Tracer
- Aucune communication réseau possible

## 6. Difficultés rencontrées

**Difficulté 1 :** Ping qui ne fonctionnait pas entre deux PC autorisés

**Problème constaté :**
Le ping entre PC0 et PC4 ne fonctionnait pas alors que PC4 avait une MAC manuelle correcte.

**Cause réelle :**
Deux PC étaient branchés sur un port configuré avec :

`switchport port-security maximum 1
violation shutdown
`
Le port est passé en shutdown, bloquant tout le trafic entrant et sortant.

**Difficulté 2 :** Perte des MAC sticky après redémarrage

**Problème constaté :**
Après un redémarrage du switch, les MAC sticky avaient disparu.

**Cause :**
La configuration n’avait pas été sauvegardée.

**Solution appliquée :**
`copy running-config startup-config
`

❌ Difficulté 3 : Confusion entre MAC sticky et MAC manuelle

La MAC sticky est apprise automatiquement

La MAC manuelle doit être saisie explicitement

Les deux doivent être sauvegardées pour être persistantes

❌ Difficulté 4 : Compréhension du mode protect

Au départ, le mode protect semblait ne rien faire.

Explication :

protect bloque uniquement la MAC non autorisée

Le port reste actif

Aucun message ni shutdown visible

7. Observations importantes

Port Security agit au niveau du port

Une violation peut bloquer toute la communication, même vers des PC autorisés

Sticky MAC ≠ sauvegarde automatique

copy run start est indispensable en fin de configuration

Un seul PC par port est la meilleure pratique de sécurité

8. Commandes de diagnostic utilisées

`show port-security interface fa0/1
show port-security address
show interface status err-disabled
show mac address-table
`

9. Bonnes pratiques retenues

1 port = 1 PC

Utiliser sticky pour la flexibilité

Utiliser shutdown pour une sécurité stricte

Toujours sauvegarder la configuration

Activer PortFast et BPDU Guard sur les ports utilisateurs

10. Conclusion

Ce TP a permis de mieux comprendre le fonctionnement réel de Port Security et son impact sur la communication réseau.
Les difficultés rencontrées ont mis en évidence l’importance :

du respect des règles de branchement

de la sauvegarde de configuration

et de l’analyse des messages de violation

Ce TP renforce mes bases nécessaires à la sécurisation d’un réseau local.
