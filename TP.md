# TP1 Configuration réseau

## Plan d'adressage :

Interfaces d'administration :
* ESXi1 : 10.<G>.0.2
* ESXi2 : 10.<G>.0.3
* ESXi3 (si dispo) : 10.<G>.0.4
* vCenter : 10.<G>.0.10

Réseau vMotion
* ESXi1 : 10.<G>.'''1'''.2
* ESXi2 : 10.<G>.'''1'''.3
* ESXi3 (si dispo) : 10.<G>.'''1'''.4

Réseau stockage NFS
* ESXi1 : 10.<G>.'''2'''.2
* ESXi2 : 10.<G>.'''2'''.3
* ESXi3 (si dispo) : 10.<G>.'''2'''.4
* vCenter : 10.<G>.'''2'''.10


## Situation de départ

Nous partons de la suiation suivante :
* l'hyperviseur possède 2 cartes réseaux
* la première carte réseau (uplink) est connectée au vSwitch0
* il y a un port groupe "VM network" qui est positionné sur le vSwitch0
* il y a un VMkernel avec le rôle de "management" qui est en place et qui permet d'accéder à l'interface d'administration de l'hyperviseur

## Objectif
