```
VM-Network (Alexandre)
```
- Pas d'interface graphique
- DHCP
- DNS
- Accès à internet
- Création domaine haribo.lan
- SSH Serveur ( pour l'accès avec VM-User )
- Le domaine devra etre installer sur la VM-Network
- Un antimalware devra être installé sur chaque machine
- L’adresse du serveur DNS devra être automatiquement distribué sur toutes les machines
- Porcentry



apt-get update && apt-get upgrade      #mise a jour du systeme de base

nano /etc/network/interfaces           #dossier de configuration des interfaces reseau
allow-hotplug ens
iface eth0 inet ens
systemctl restart networking           #redemarrage du reseau pour la prise en charge des modification

apt install isc-dhcp-server            #installation du service DHCP
nano /etc/default/isc-dhcp-server      #dossier de configuration d'ecoute du service DHCP
DHCPDv4_CONF=/etc/dhcp/dhcpd.conf      #on decommente cette ligne
INTERFACESv4="ens"
INTERFACESv6="ens"
