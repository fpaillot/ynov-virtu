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


# TP5 : Création et modification de machines virtuelles

* Renomer le datastore de chaque ESXi en datastore-esxi(1-3)
* Pour chacune des 2 VM à créer ci-dessous :
  - Noter avant le déploiement : l’espace libre, utilisé et alloué pour sur le datastore de votre ESXI
  - Noter après le déploiement : l’espace libre, utilisé et alloué pour sur le datastore, la taille du fichier vmdk du disque de la VM
* Créer « VM1-ESXi(1-3)-thin » (exemple : VM1-ESXi1-thin pour la personne qui l'ESXi1) : 
  - type linux redhat/Centos 7, disque de 8go en thin, 1Go de RAM
* Créer « VM2-ESXi(1-3)-thick » (exemple : VM1-ESXi2-thick pour la personne qui l'ESXi1) :
  - type linux redha/Centost 7, disque de 10go en thick lazy, 1Go de RAM
* Installer un système d’exploitation sur VM1-ESXi(1-3)-thin (utiliser le lecteur CD de l'ESXi comme source)
* Dans le résumé de la machine virtuelle, relevez les informations suivantes :
  - Quel est le statut des VMware Tools ?
  - Quelle est l’IP de la VM ?
  - Mémoire hôte utilisée, mémoire invité active
  - Quel est l'état des VMware tools ?


Quel est le module noyau utilisé pour la carte réseau et le disque ?


&nbsp;&nbsp;

# TP6 : Les snapshots

* Arrêter la VM « « VM1-ESXi(1-3)-thin »
* Sélectionnez une VM et faites un clic droit « Snapshot > Take a snapshot »
* Donnez le nom que souhaitez
* Démarrer la VM
* Créer un répertoire dans la VM

&nbsp;

* Arrêter la VM
* Sélectionnez votre VM faites un clic droit « Snapshot > Snapshot manager »
* Revenez au snapshot précédent
* Démarrer la VM
	- Est-ce que le répertoire existe toujours ?

&nbsp;


* Répéter les mêmes étapes que précédemment mais cette fois supprimez le snapshot avec le snapshot manager.
	- Est-ce que le répertoire existe toujours ?

&nbsp;

* Se positionner sur la VM et noter son « Storage usage »
* Se positionner sur le DS hébergeant la VM et noter le « Used »
* Dans la VM lancer la commande suivante pour créer un fichier de 128Mo :
'''
dd if=/dev/urandom of=/root/sample.txt bs=16M count=8
'''
 &nbsp;

* Arrêter la VM
* Prendre un snapshot de la VM
* Démarrer la VM
* Supprimer le fichier /root/sample.txt
&nbsp;

* Aller dans « Home » -> « Storage »
* Sélectionner le Datastore qui héberge la VM
* Aller dans le répertoire portant le nom de la VM.
	- Que peut-on observer au niveau des fichiers de snapshot VMware ?
* Se positionner sur la VM et noter son « Storage usage »
* Se positionner sur le DS hébergeant la VM et noter le « Used »
* Supprimer le snapshot
* Se positionner sur la VM et noter son « Storage usage »
* Se positionner sur le DS hébergeant la VM et noter le « Used »
	- Que peut-on en conclure ?

&nbsp;&nbsp;


# TP7 Configuration de l'iSCSI
L'objectif de ce TP est d'activer la couche iSCSI logicielle d'ESXi et de se connecter au LUN créé sur le cluster ONTAP

* Aller dans "Stockage > Adaptateur"
* Configurer l'iSCSI ; utiliser le "vmkernel1" pour la liaison de port, en cible dynamique, indiquer l'IP **iSCSI** de la baie ONTAP : NE PAS CLIQUER SUR ENREGISTER avant l'étape ci-après
* Copier le nom (IQN) de l'initiateur
* Aller dans la configuration de **l'initator group** "ESXi" sur la baie ONTAP, l'éditer et ajouter l'IQN dans **Initiators**
* Lancer un "Réanalyser" dans "Adaptateur" puis "Réanalyser" dans "Périphériques"
* Créer un **datastore** (banque de données) sur le nouvel espace de stockage disponible, nom : datastore-iscsi, type : VMFS6

&nbsp;&nbsp;


# TP8 Configuration du NFS
L'objectif de ce TP est de se connecter au partage NFS créé sur le cluster ONTAP

* Aller dans "Stockage > Banque de données"
* Créer un **datastore** (banque de données), sélectionner "Monter la banque de données NFS", nom : datastore-nfs, indiquer l'IP NFS de baie ONTAP et le chemin, NFS v3

&nbsp;&nbsp;


# TP9 Création du cluster

* Allez dans  "Host and clusters"
* Faites un clic droit sur le vôtre datacenter  et cliquez sur  "New cluster"
* Nommez le cluster "cluster-gX" ou X est le numéro de votre groupe
* Ne pas activer le HA ni le DRS
* Faites un "transférer vers" de vos hyperviseurs dans le cluster 
* Connectez-vous en SSH sur vos hyperviseurs
* Lancez la commande :
'''
tail -f /var/log/fdm.log
'''

* Faites un clic droit sur votre cluster « Edit settings »
* Validez la case « Turn on VMware HA »

* Quel(s) message(s) observez-vous dans le fichier de log concernant une élection ?
* Que peut-on en déduire ?
* Vérifier que le HA est activé sur le cluster
* Quel est l’état HA de l’ESXi ?
* Quel est le niveau de « VM restart priority » par défaut du cluster ?
* Quelle est la « Host isolation response » par défaut du cluster ?
* Modifier les options d’HA d’une VM,  passer le « VM restart priority » en High
* Modifier les options d’HA d’une VM, passer le « Host isolation response » à shutdown

&nbsp;&nbsp;


# TP10 Activation du DRS
* Faites un clic droit sur votre cluster « Edit settings »
* Validez la case « Turn on VMware DRS »

* Créer 4 VM (appli1-app1, appl1-app2, appl1-db1, appli1-db2) de type CentOS 7 avec des disques en thin provisionning de 1GO et 512mo de RAM sur les datastore 
* Créer une régle pour que les "VM appli1-app1" et "appli1-app2" ne fonctionnent pas sur le même hyperviseur 
* Créer un groupe de VM nommé "appli1-app" avec les VM appli-app* et un groupe "appli1-db" avec les VM appli1-db*
* Créer une règle de redémarrage "virtual machines to virtual machines" nommé "appli1" pour que le groupe "appli1-app" démarre après le groupe "appli1-db"
* Créer un groupe d’hôte DRS nommé « appli1-db-host » contenant le serveur ESXi1
* Créer une règle pour que le groupe « appli1-db » réside si possible sur le groupe d’hôte « appl1-db-host »

* Démarrer les VM et noter leur placement. Est-il logique ?

* Faites un vMotion de la VM appli1-db2 vers ESXi 2 ou 3
  - Que constatez-vous ?

* Y’a-t-il de la contention ? Que pouvez-vous en déduire ?
