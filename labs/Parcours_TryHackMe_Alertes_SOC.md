# Parcours TryHackMe – Alertes SOC Niveau 2 (L2)

## Objectif du parcours
Ce parcours vise à former à l'analyse et à la gestion des alertes de sécurité à un **niveau SOC L2**, en mettant l'accent sur :
- La détection d'activités suspectes sur les hôtes et réseaux.
- La qualification des alertes selon le **niveau de sévérité**.
- L'escalade et le traitement correct des incidents de sécurité.

## Concepts clés

### 1. Analyse d'alerte
- Identifier le **type d’alerte** : malware, hameçonnage, reconnaissance AD, mouvements latéraux.
- Examiner le **contexte** : processus source, utilisateur, hôte, commandes exécutées.
- Vérifier les indicateurs techniques : SPF/DKIM, fichiers suspects, comportements anormaux.

### 2. Qualification SOC L2
- **Niveau de sévérité** : Bas / Moyen / Élevé (High)
- **Statut** : Faux positif / Vrai positif
- **Escalade** : Les alertes à haut risque ou confirmées sont transmises au SOC L2 pour analyse approfondie.

### 3. Approche 5W pour les alertes
- **Who** : Utilisateur ou processus initiateur.
- **What** : Activité suspecte ou commande exécutée.
- **When** : Date/heure de détection.
- **Where** : Hôte ou segment réseau affecté.
- **Why** : Raisons ou objectif probable (ex : reconnaissance AD, exfiltration, malware).
- **How** (optionnel) : Méthode d'attaque ou vecteur utilisé.

### 4. Exemple typique
- **Alerte** : Exécution de commandes de découverte AD (`whoami`, `net user`, `nltest`) sur un serveur Exchange DMZ.
- **Contexte** :  
  - Processus parent : `revshell.exe`  
  - Processus source : `cmd.exe`  
  - Utilisateur : `SYSTEM`  
  - OS : Windows Server 2012 R2
- **Analyse** :  
  - Indicateur de post-exploitation et reconnaissance AD.
  - Risque élevé de mouvement latéral et d’escalade de privilèges.
- **Action recommandée** : Confinement immédiat, investigation SOC L2, recherche d’IOC sur les autres serveurs.

### 5. Bonnes pratiques SOC L2
- Toujours valider si l’activité est **légitime ou malveillante**.
- Documenter et consigner toutes les observations pour l’escalade.
- Prioriser les alertes selon l’impact sur le réseau et les données critiques.
- Utiliser des outils de forensic et sandbox pour analyser les fichiers suspects.

## Conclusion
La maîtrise du SOC L2 nécessite de combiner **analyse technique, contextualisation des alertes et prise de décision rapide**. L’approche 5W permet de structurer les rapports et de faciliter l’escalade efficace vers les équipes de réponse aux incidents.
