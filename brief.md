# VM-Gate
- Pas d’environnement graphique [x]
- L'adresse IP sera fixe [x]
- Toutes les machines devront pouvoir accéder à internet à travers une Gateway [x]
- Serveur proxy HTTP 
- Le client est en attente d’une proposition d’amélioration du filtrage proxy 
- Toute requête vers une page web devra obligatoirement passer par le serveur proxy 
- Un antimalware devra être installé sur chaque machine
- Un programme permettant de détecter les IPS effectuant des scans de port devra être installé sans les bannir mais devra les logger dans un fichier

## CONFIGURATION DE L'INTERFACE RÉSEAU
### VmWare
Modifier la 1ere carte réseau en NAT  
Ajouter une 2eme carte réseau en host-only
### VM-GATE
```
apt install sudo
sudo apt install iptables
```
#### Configuration des cartes réseaux
> /etc/network/interfaces
```
auto ens33
iface ens33 inet dhcp

allow-hotplug ens36
iface ens36 inet static
address 10.0.0.254
netmask 255.255.255.0
gateway 10.0.0.254
```
#### Up l'interface
```
ip link set ens33 up
ip link set ens36 up
```
#### Activation de l'ip forward
> /etc/sysctl.conf
```
net.ipv4.ip_forward=1
```
#### Ajouter une règle postrouting
```
sudo iptables -t nat -A POSTROUTING -o ens33 -j MASQUERADE
```
#### Rendre permanant les règles de routage
```
sudo apt install iptables-persistent
yes
yes
```
## MISE EN PLACE DU PROXY AVEC FILTRAGE
### VM-GATE
``` 
sudo apt install squid  
sudo systemctl enable squid
```
```
sudo nano /etc/squid/squid.conf
```
> squid.conf
```
http_port 3128

acl CONNECT method CONNECT
acl blacklist dstdomain "/etc/squid/blacklist.acl"

http_access deny blacklist
http_access allow localnet
http_access allow localhost

http_access deny all
```
```
sudo nano /etc/squid/blacklist.acl
```
> blacklist.acl
```
.facebook.com
.youtube.com
```
```
sudo systemctl restart squid
```
## LIAISON VM-GATE <-> VM-AZURE
### VM-GATE
```
sudo nano /etc/squid/squid.conf
```
> squid.conf
```
acl azure_vm src 20.111.15.63
http_access allow azure_vm
```
```
sudo systemctl restart squid
```
### VM-AZURE
```
sudo nano /etc/environment
```
> environment
```
http_proxy=http://83.118.220.69:3128/
https_proxy=http://83.118.220.69:3128/
ftp_proxy=http://83.118.220.69:3128/
no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"
```
rechargez le fichier avec la commande suivante :
```
source /etc/environment
```


