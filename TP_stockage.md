# TP1 Initialisation du cluster Ontap (en mode mono noeud

## Objectifs
* Créer le cluster
* Assigner les disques et créer un agrégat
* Créer une SVM et configurer les protocoles NFS et iSCSI en respectant le plan d'adressage



1. Cluster
- Indiquer le nom du cluster
- Indiquer un MDP pour l'utilisateur admin (Ingesup33!)
- Entrer les numéros de licences à utiliser :
```
SMKQROWJNQYQSDAAAAAAAAAAAAAA,OUVWXPKBFDUFZGABGAAAAAAAAAAA,
QFATWPKBFDUFZGABGAAAAAAAAAAA
```

2. Network
- Indiquer l'adresse de management du cluster (VIP) et sélectionner l'interace e0c
- Ne rien entrer pour le Service Processor, le DNS et le NTP

3. Support
- Désactiver les Autosuppot
- Cocher "Email", entrer l'IP 10.110.0.1 et en email : test@test.com
- Ne pas cocher SNMP ou Syslog Server
- Ne pas activer le Cluster Configuration backup details

4. Storage
- Utiliser les paramètres par défaut pour la création de l'agrégat de disques

5. SVM
- Indiquer "svm0" pour le nom de la SVM
- Activer le NFS et le iSCSI
- Indiquer l'IP pour le NFS et sélectionner l'interface e0c
- Indiquer l'IP pour le iSCSI et sélectionner l'interface e0c
