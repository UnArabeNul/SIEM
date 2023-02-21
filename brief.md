# VM-Gate
- Pas d’environnement graphique [x]
- Serveur proxy HTTP [x]  

sudo apt-get update  
sudo apt-get install squid

sudo nano /etc/squid/squid.conf
### squid.conf
```
#décommenter la ligne ci-dessous
http_access allow localnet
```
- Le client est en attente d’une proposition d’amélioration du filtrage proxy [x]
### squid.conf
```
http_port 3128

acl CONNECT method CONNECT
acl blacklist dstdomain "/etc/squid/blacklist.acl"

http_access deny blacklist
http_access allow localhost

http_access deny all
```
sudo nano /etc/squid/blacklist.acl
### blacklist.acl
```
.facebook.com
.youtube.com
```

- Toute requête vers une page web devra obligatoirement passer par le serveur proxy [x]
- Toutes les machines devront pouvoir accéder à internet à travers une Gateway 
- Un antimalware devra être installé sur chaque machine
- L'adresse IP sera fixe
- Un programme permettant de détecter les IPS effectuant des scans de port devra être installé sans les bannir mais devra les logger dans un fichier.

