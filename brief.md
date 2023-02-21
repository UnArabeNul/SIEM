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
systemctl start isc-dhcp-server
systemctl enable isc-dhcp-server

nano /etc/dhcp/dhcpd.conf.             #dossier de configuration des paramattres DHCP

default-lease-time 86400; # Bail de 24H
max-lease-time 172800; # Bail maxi de 48H
subnet 192.168.1.0 netmask 255.255.255.0 { #declaration du reseau
        range                           192.168.1.100 192.168.1.199; # Plage IP
        option domain-name-servers      192.168.1.11; # DNS
        option routers                  192.168.1.1; # Passerelle
}
