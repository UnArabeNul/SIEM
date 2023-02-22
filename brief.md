```
VM-Network (Alexandre)
```
- Pas d'interface graphique *fait*
- DHCP *fait*
- DNS 
- Accès à internet *fait*
- Création domaine haribo.lan
- SSH Serveur ( pour l'accès avec VM-User )
- Le domaine devra etre installer sur la VM-Network
- Un antimalware devra être installé sur chaque machine
- L’adresse du serveur DNS devra être automatiquement distribué sur toutes les machines
- Porcentry



DHCP

apt-get update && apt-get upgrade                #mise a jour du systeme de base

nano /etc/network/interfaces                     #dossier de configuration des interfaces reseau
auto ens36
iface ens36 inet static
        address 10.0.0.10                        #ip du serveur dhc
        netmask 255.255.255.0                    #masque du reseau
        gateway 10.0.0.254                       #passerelle
        dns 10.0.0.10                            #ip du serveur dns
systemctl restart networking                     #redemarrage du reseau pour la prise en charge des modification

apt install isc-dhcp-server                      #installation du service DHCP
nano /etc/default/isc-dhcp-server                #dossier de configuration d'ecoute du service DHCP
DHCPDv4_CONF=/etc/dhcp/dhcpd.conf                #on decommente cette ligne
INTERFACESv4="ens36"
INTERFACESv6="ens36"

nano /etc/dhcp/dhcpd.conf.                       #dossier de configuration des paramattres DHCP

default-lease-time 86400;                        # Bail de 24H
max-lease-time 172800;                           # Bail maxi de 48H
subnet 10.0.0.0 netmask 255.255.255.0 {       #declaration du reseau
        range                           10.0.0.50 10.0.0.150; # Plage IP
        option domain-name-servers      10.0.0.10;     # DNS
        option routers                  10.0.0.254;    # Passerelle
}

systemctl start isc-dhcp-server
systemctl enable isc-dhcp-server


DNS

apt-get install bind9

nano /etc/hostname                      #y inscrire le FQDN "vm-network.haribo.lan
nano /etc/hosts                         #modifier le fichier et y ajouter le nom du serveur et son IPV4

127.0.0.1       localhost
127.0.0.1       VM-Network.haribo.lan
10.0.0.10       VM-Network.haribo.lan

nano /etc/resolv.conf                   #Indiquez le domaine et la zone de recherche DNS.

search haribo.lan
domain haribo.lan
nameserver 10.0.0.10

nano /etc/bind/named.conf.local         #déclarer nos zones DNS

zone "haribo.lan" {
        type master;
        file "/etc/bind/db.haribo.lan";
};

zone "0.0.10.in-addr.arpa" {
        type master;
        file "/etc/bind/db.0.0.10.in-addr.arpa";
};
