# TP1 Configuration réseau : vSwitch et VMkernel

L'objectif de ce TP est la manipulation des différents objets de base utilisés sur ESXi pour la gestion du réseau.
N'hésitez pars à reprendre le cours pour revoit la terminologie.

## Situation de départ

Nous partons de la suiation suivante :
* l'hyperviseur possède 2 cartes réseaux
* la première carte réseau (**uplink**) est connectée au vSwitch0
* il y a un **port group** "VM network" qui est positionné sur le vSwitch0
* il y a un **port group** nommé "Management network"
* il y a un **VMkernel** "vmk0" avec le rôle de "management", qui utilise le **portgroup** "Management Network". Ce VMkernel pour l'IP d'administration de l'ESXi (**rôle** "management") 


## Actions
* Créer un nouveau **vSwitch** nommé "vSwitch1"
* Rattacher le deuxiéme **uplink** sur ce nouveau **vSwitch**
* Créer un **portgroup** nommé "vMotion" sur le **vSwitch** "vSwitch0
* Créer un **portgroup** nommé "stockage" sur le **vSwitch** "vSwitch0"
* Supprimer le **portgroup** sur le "vSwitch0" nommé "VM network"
* Créer un **portgroup** "VM Network" sur le "vSwitch1"

*Créer un **vmkernel** utilisant le **portgroup** "stockage", IPv4 uniquement et indiquer l'IP appropriée, ne pas cocher de service
*Créer un **vmkernel** utilisant le **portgroup** "Vmotion", IPv4 uniquement et indiquer l'IP appropriée, cocher le service "vmotion"


Une fois ces différentes configuration en place, vous devrez vous assurer que tout est opérationnel.
Pour se faire, il faut utiliser la commande **vmkping** disponible en mode **console**.

Dans le cas présent, l'accés à la **console** ne sera possible que par **SSH**.
Activer le service **SSH** puis connectez-vous à votre hyperviseur. Utilisez la commande **vmkping** ou **ping** pour vérifier la bonne configuration de vos interfaces

&nbsp;&nbsp;

# TP2 Rattachement des ESXi au vCenter

* Se connecter au vCenter
* Se positionner sur "host and clusters"
* Créer un "datacenter" nommé dc-gX
* Ajouter chaque ESXi (à partir de son IP) dans le vCenter

&nbsp;&nbsp;


# TP3 Configuration réseau : dvSwitch

* Créer un **dvswitch** nommé "DSwitch0", version 6.6.0, nombre de liaisons montantes : 1, I/O Control activé, pas de groupe de port par défaut
* Détacher l'interface **vmnic1** du "vSwitch1" 
* Créer un **distributed portgroup** par personne, nommé : gX-VMX, configuration par défaut
* Utiliser le menu **Ajouter et gérer les hôtes**  (clic droit sur le dvSwitch) et ajouter votre ESXi en utilisant la **vmnic1** libérée précédemment


**Validation**
Si tout est bien configuré, vous devez retrouver :
* 1 dvSwitch nommé "DSwitch0"
* 2 ou 3 distributed portgroup nommés : gX-VM(1-3)
* Sur chaque ESXi dans "Réseaux" (en passant par hôtes et cluster), un "Uplink port group" qui apparait

&nbsp;&nbsp;

# TP4 Configuration des accès

L'accès en administrator c'est bien mais l'accès nominatif, c'est mieux !

* A partir du menu "Administration", aller dans "Single Sign-on"
* Créer un utilisateur par personne présente dans le groupe, dans le domaine "vsphere.local"
* Ajouter ces utilisateurs au groupe "Administrators"
* Connectez-vous avec votre utilisateur nominatif et n'utilisez plus le compte "administrator@vsphere.local"

&nbsp;&nbsp;

# TP5 Configuration de l'iSCSI
L'objectif de ce TP est d'activer la couche iSCSI logicielle d'ESXi et de se connecter au LUN créé sur le cluster ONTAP

* Aller dans "Stockage > Adaptateur"
* Configurer l'iSCSI ; utiliser le "vmkernel1" pour la liaison de port, en cible dynamique, indiquer l'IP **iSCSI** de la baie ONTAP : NE PAS CLIQUER SUR ENREGISTER avant l'étape ci-après
* Copier le nom (IQN) de l'initiateur
* Aller dans la configuration de **l'initator group** "ESXi" sur la baie ONTAP, l'éditer et ajouter l'IQN dans **Initiators**
* Lancer un "Réanalyser" dans "Adaptateur" puis "Réanalyser" dans "Périphériques"
* Créer un **datastore** (banque de données) sur le nouvel espace de stockage disponible, nom : datastore-iscsi, type : VMFS6


# TP6 Configuration du NFS
L'objectif de ce TP est de se connecter au partage NFS créé sur le cluster ONTAP

* Aller dans "Stockage > Banque de données"
* Créer un **datastore** (banque de données), sélectionner "Monter la banque de données NFS", nom : datastore-nfs, indiquer l'IP NFS de baie ONTAP et le chemin, NFS v3
