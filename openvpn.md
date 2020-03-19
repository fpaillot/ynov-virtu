
# Mise en place du client OpenVPN
## Sous Windows (10)

* Télécharger le client : https://swupdate.openvpn.org/community/releases/openvpn-install-2.4.8-I602-Win10.exe
* Récupérer sur le présent dépôt dans **openvpn/windows** les fichiers client.ovpn, ca.crt et ta.key
* Les copier dans le répertoire **C:\Program Files\OpenVPN\config**
* Lancer **OpenVPN GUI**, cliquer sur l'icône qui apparait dans la barre des tâches en bas à droite et "connecter"

## Sous Linux
* Installer le paque openvpn (avec ou sans intégration network-manager)
* Récuper sur le présent dépôt dans **openvpn/linux/** les fichiers client.ovpn, ca.crt et ta.key
* Pour un lancement via la CLI, créer un répertoire .ovpn dans le homedir et y placer les trois fichiers puis lancer :
'''
cd ~/.opvn/
openvpn client.conf
'''
