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


## Objectifs
* Créer un nouveau **vSwitch** nommé "vSwitch1"
* Rattacher le deuxiéme **uplink** sur ce nouveau **vSwitch**
* Créer un **portgroup** nommé "vMotion" sur le **vSwitch** "vSwitch0
* Créer un **portgroup** nommé "NFS" sur le **vSwitch** "vSwitch0"
* Supprimer le **portgroup** sur le "vSwitch0" nommé "VM network"
* Créer un **portgroup** "VM Network" sur le "vSwitch1"


Une fois ces différentes configuration en place, vous devrez vous assurer que tout est opérationnel.
Pour se faire, il faut utiliser la commande **vmkping** disponible en mode **console**.

Dans le cas présent, l'accés à la **console** ne sera possible que par **SSH**.
Activer le service **SSH** puis connectez-vous à votre hyperviseur. Utilisez l'aide de la commande **vmkping** pour trouver les bons arguments.
