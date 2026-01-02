# Rapport d'installation et configuration de Wireshark sur Ubuntu 

---

## 1. Contexte

Aujourd’hui, j’ai voulu installer et configurer Wireshark sur mon Ubuntu pour analyser le trafic réseau, notamment le Wi-Fi.  

Mon objectif était de voir toutes mes interfaces réseau et de pouvoir capturer des paquets. Comme je débute encore avec Linux et les permissions, j’ai rencontré plusieurs difficultés et j’ai voulu noter chaque étape et mes réflexions pour garder une trace claire de mon apprentissage.

---

## 2. Étapes réalisées et observations

### Étape 1 : Installation de Wireshark

sudo apt install wireshark

    L’installation s’est déroulée normalement.

    Rien de spécial à signaler ici, mais j’étais un peu inquiète à l’idée de gérer correctement les permissions.

Étape 2 : Reconfiguration pour permettre aux utilisateurs non-root de capturer

sudo dpkg-reconfigure wireshark-common

    J’ai répondu YES à la question « Allow non-superusers to capture packets? »

    J’ai reçu le message :

Le groupe wireshark est un groupe système...

    Au début, ça m’a un peu inquiétée , je ne savais pas si ça allait bloquer le fonctionnement.

Étape 3 : Ajout de mon utilisateur au groupe wireshark

sudo usermod -aG wireshark $USER

    Après ça, j’ai vérifié mes groupes avec :

groups

    Résultat : wireshark n’apparaissait pas.

    Moment de doute : je pensais m’être trompée quelque part, mais j’ai compris qu’il fallait recharger ma session pour que les changements prennent effet.

Étape 4 : Redémarrage de la session

    Pour appliquer les changements de groupe, j’ai redémarré ma session Ubuntu.

    Après reconnexion, j’ai de nouveau exécuté :

groups

    Et cette fois, wireshark apparaissait 

Étape 5 : Lancement de Wireshark

    J’ai lancé Wireshark sans sudo :

wireshark

    Toutes mes interfaces réseau étaient visibles, y compris le Wi-Fi (wlp…).

    Enfin, je pouvais commencer à capturer les paquets sur ma machine.

3. Difficultés rencontrées

    Interface Wi-Fi invisible au départ

        Seule l’interface lo était visible.

        Cause : permissions non appliquées dans la session active.

    Message “Le groupe wireshark est un groupe système”

        Initialement inquiétant, mais en réalité ce message est normal et n’empêche pas le fonctionnement.

    Le groupe wireshark n’apparaissait pas après usermod

        Il fallait redémarrer la session pour que le groupe soit pris en compte.

    Compréhension du Wi-Fi sous Linux

        Même avec l’interface visible, Wireshark ne capture par défaut que le trafic de ma machine.

        Pour capturer tout le trafic Wi-Fi, il faudrait activer le mode monitor et avoir une carte compatible.

4. Observations supplémentaires

    Redémarrer la session est parfois indispensable pour que les modifications de groupes prennent effet.

    La gestion des permissions et des groupes sur Ubuntu est un point clé à connaître pour l’utilisation de Wireshark.

    J’ai maintenant toutes les interfaces visibles et je peux capturer du trafic sur ma machine sans utiliser sudo.

5. Conclusion

Cette session m’a permis de :

    Comprendre comment gérer les groupes et permissions sous Ubuntu.

    Identifier pourquoi Wireshark ne montre pas l’interface Wi-Fi immédiatement.

    Apprendre qu’un redémarrage de session est souvent nécessaire après l’ajout à un groupe.

    Lancer Wireshark correctement et être prête pour mes premières captures réseau.

Même si j’ai eu des hésitations et des moments de doute, cette expérience m’a permis d’avancer avec confiance et de mieux comprendre le fonctionnement de Wireshark sur Ubuntu.

