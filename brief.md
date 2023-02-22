# VM-Gate
- Pas d’environnement graphique [x]
- Serveur proxy HTTP [x]  
- Le client est en attente d’une proposition d’amélioration du filtrage proxy [x]
- Toute requête vers une page web devra obligatoirement passer par le serveur proxy [x]
- Toutes les machines devront pouvoir accéder à internet à travers une Gateway 
- Un antimalware devra être installé sur chaque machine
- L'adresse IP sera fixe [x]
- Un programme permettant de détecter les IPS effectuant des scans de port devra être installé sans les bannir mais devra les logger dans un fichier.

## MISE EN PLACE DU PROXY AVEC FILTRAGE
```
sudo apt-get update  
sudo apt-get install squid  
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


