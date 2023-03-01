# VM-Network
- <del>Pas d'interface graphique </del>
- <del>DHCP </del>
- <del>DNS </del>
- <del>Accès à internet </del>
- <del>Création domaine haribo.lan </del>
- SSH Serveur ( pour l'accès avec VM-User )
- <del>Le domaine devra etre installer sur la VM-Network </del>
- Un antimalware devra être installé sur chaque machine
- <del> L’adresse du serveur DNS devra être automatiquement distribué sur toutes les machines </del>
- Porcentry

#### Mise a jour du systeme de base
```
apt-get update && apt-get upgrade
```
#### Configuration de l'interface réseau
> /etc/network/interfaces                
```
auto ens36
iface ens36 inet static
        address 10.0.0.10                        #ip du serveur dhc
        netmask 255.255.255.0                    #masque du reseau
        gateway 10.0.0.254                       #passerelle
        dns-nameservers 10.0.0.10                #ip du serveur dns
```
#### On re-démarre le réseau pour la prise en charge des modifications
```
systemctl restart networking                     
```
### DHCP
#### Installation du service DHCP
```
apt install isc-dhcp-server                      
```
#### Configuration des interfaces d'écoute pour le DHCP
> /etc/default/isc-dhcp-server
```
DHCPDv4_CONF=/etc/dhcp/dhcpd.conf                #on decommente cette ligne
INTERFACESv4="ens36"
```
#### Configuration de paramettres DHCP
> /etc/dhcp/dhcpd.conf
```
default-lease-time 86400;                        # Bail de 24H
max-lease-time 172800;                           # Bail maxi de 48H
subnet 10.0.0.0 netmask 255.255.255.0 {       #declaration du reseau
        range                           10.0.0.50 10.0.0.150; # Plage IP
        option domain-name-servers      10.0.0.10;     # DNS
        option routers                  10.0.0.254;    # Passerelle
        
       host {
                hardware ethernet 68:5d:43:2a:f3:af;
                fixed-address 10.0.0.20;
        }
}
```
#### Démarrage du service DHCP et on le fait démarrer avec la VM
```
systemctl start isc-dhcp-server
systemctl enable isc-dhcp-server
```
### DNS
#### Installation du service DNS
```
apt-get install bind9
```
#### Déclaration du nom d'hôte
>/etc/hostname                      
#### Déclaration des hôtes distants
>/etc/hosts                         
```
127.0.0.1       localhost
127.0.1.1       VM-Network.haribo.lan
10.0.0.10       VM-Network.haribo.lan           VM-Network
10.0.0.20       VM-Server.haribo.lan            VM-Server
10.0.0.254      VM-Gateway.haribo.lan           VM-Gateway
```
#### Déclaration du domaine, de la zone de recherche du domaine et du serveur DNS
>/etc/resolv.conf                   
```
search haribo.lan
domain haribo.lan
nameserver 10.0.0.10
```
#### Déclaration des zones DNS
>/etc/bind/named.conf.local         
```
zone "haribo.lan" {
        type master;
        file "/etc/bind/db.haribo.lan";
};

zone "0.0.10.in-addr.arpa" {
        type master;
        file "/etc/bind/db.0.0.10.in-addr.arpa";
};
```
#### Configuration de la zone DNS
>/etc/bind/db.haribo.lan
```
$TTL 10800
$ORIGIN haribo.lan.
@       IN SOA VM-Network.haribo.lan root.haribo.lan (
        2023022201;
        3h;
        1h;
        1w;
        1h);
@       IN NS VM-Network.haribo.lan.
VM-Network  IN A  10.0.0.10
intra  IN A  10.0.0.20
localhost  IN A  127.0.0.1
```
#### Configuration de la zone inverse du DNS
>/etc/bind/db.0.0.10.in-addr.arpa
```
$TTL 10800
$ORIGIN 0.0.10.in-addr.arpa.
@       IN SOA VM-Network.haribo.lan root.haribo.lan (
        2023022201;
        3h;
        1h;
        1w;
        1h);
@       IN NS VM-Network.haribo.lan.
20     IN PTR intra.haribo.lan.
10     IN PTR VM-Network.haribo.lan.
1      IN PTR localhost.haribo.lan.
```
