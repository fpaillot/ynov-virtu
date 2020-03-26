# TP1 Initialisation du cluster Ontap (en mode mono noeud

## Objectifs
* Créer le cluster
* Assigner les disques et créer un agrégat
* Créer une SVM et configurer les protocoles NFS et iSCSI en respectant le plan d'adressage

&nbsp;&nbsp;

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

&nbsp;&nbsp;

# TP2 Configuration iSCSI

* Se connecter à 'interface d'administration
* Aller dans "Storage > LUNs"
* Créer un **portset** ; nom : portset1, type : iSCSI, sélectionner la seule interface disponible
* Céer un **initiator group** ;  nom : ESXi, type : iSCSI, operating system : VMware, portset : portset1, initator : "nom initiateur VMware"


* Créer un LUN, nom : lun_1, type : VMware, space reserve : enable, taille : 10Go
* Créer un nouveau volume ; nom : lun_1_vol, agrégat : par défaut, tiering-policy : aucune
* Mapper au groupe "ESXi"

&nbsp;&nbsp;

# TP3 Configuration NFS

* Se connecter à 'interface d'administration
* Aller dans "Storage > Volume"
* Créer un nouveau volume Flexvol ; nom : vol_nfs1, agregat par défaut, type : NAS, taille : 1Ogo, snapshot reserve : 0, space reservation : "Thick"


* Aller dans "Storage > SVM"
* Cliquer sur "SVM settings"
* Aller dans "Exports Policies"
* Créer un **export policy** ; nom : ESXi, cliquer sur "Add" pour ajouter les IP de chaque ESX pour NFS (Access protocol : NFS, Allow super user access, sélectionner : Read-only et Read-write **uniquement** pour **Unix**)


* Aller dans "Junction Paths"
* Change les **export policy** pour le point de montage **/** et **vol_nfs1** et sélectionner "ESXi"

&nbsp;&nbsp;

# TP4
