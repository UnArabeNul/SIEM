# VM-Gate
- Pas d’environnement graphique [x]
- Serveur proxy HTTP [x]  
sudo nano /etc/squid/squid.conf  
### squid.conf
```
http_port 3128
acl localnet src 10.0.0.0/8 # définit le réseau local
acl SSL_ports port 443
acl Safe_ports port 80 # http
acl Safe_ports port 443  # https
acl CONNECT method CONNECT
http_access allow localnet
http_access allow localhost
http_access deny all
```
- Toute requête vers une page web devra obligatoirement passer par le serveur proxy
- Toutes les machines devront pouvoir accéder à internet à travers une Gateway
- Un antimalware devra être installé sur chaque machine
- L'adresse IP sera fixe
- Un programme permettant de détecter les IPS effectuant des scans de port devra être installé sans les bannir mais devra les logger dans un fichier.

