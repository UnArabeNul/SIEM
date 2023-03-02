# Nadir
Tâches :
* <del>Pas d’environnement graphique</del>
* <del>L'adresse IP sera fixe</del>
* <del>Toutes les machines devront pouvoir accéder à internet à travers une Gateway</del>
* <del>Serveur proxy HTTP</del>
* <del>Toute requête vers une page web devra obligatoirement passer par le serveur proxy</del>
* <del>Fail2ban
  * Si une IP récolte deux tentatives de connexions échouées dans une fenêtre de 6
  heures, son IP devra être bannie pour une durée de 2 minutes</del>
* <del>Metricbeat</del>
* Filebeat
* SSH
  * L'authentification en root devra être désactivé et ce même par clé.
  * Le serveur SSH devra logger chaque connexion échouée ou réussie.
* Portcentry
  * Un programme permettant de détecter les IPS effectuant des scans de port devra être installé sans les bannir mais devra les logger dans un fichier
* Un antimalware devra être installé sur chaque machine
## SOMMAIRE
* [Configuration de la VM-GATE](#configuration-de-la-vm-gate)
* [Serveur proxy HTTP](#mise-en-place-du-proxy-avec-filtrage)
* [Fail2ban](#fail2ban)
* [Metricbeat](#metricbeat)
* [Filebeat](#filebeat)
* SSH
* Portcentry
* Antimalware

## Configuration de la VM-GATE
#### VM Ware
Modifier la 1ere carte réseau en NAT  
Ajouter une 2eme carte réseau en host-only
#### VM-GATE
```
apt install sudo
sudo apt install iptables
```
### Configuration des cartes réseaux
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
#### Up l'interface :
```
ip link set ens33 up
ip link set ens36 up
```
#### Activation de l'ip forward :
> /etc/sysctl.conf
```
net.ipv4.ip_forward=1
```
#### Ajouter une règle postrouting :
```
sudo iptables -t nat -A POSTROUTING -o ens33 -j MASQUERADE
```
#### Rendre permanant les règles de routage :
```
sudo apt install iptables-persistent
yes
yes
```
## MISE EN PLACE DU PROXY AVEC FILTRAGE
#### VM-GATE
``` 
sudo apt install squid  
sudo systemctl enable squid
```
```
sudo nano /etc/squid/squid.conf
```
> squid.conf
```
acl lan src 10.0.0.0/24
acl azure_vm src 4.233.61.203
acl blacklist url_regex "/etc/squid/blacklist.acl"
http_access deny blacklist lan 
http_access deny blacklist azure_vm
http_access allow lan
http_access allow azure_vm
```
```
sudo nano /etc/squid/blacklist.acl
```
> blacklist.acl
```
.facebook.com
arme
```
```
sudo systemctl reload squid
```
### Mise en place du proxy sur les machines
#### VM-X
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
## Fail2ban
### Installation de Fail2ban
```
apt install fail2ban
```
### Configuration de Fail2ban
```
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
nano /etc/fail2ban/jail.local
```
> jail.local
```
[sshd]
enable = true
port    = ssh
logpath = %(sshd_log)s
backend = %(sshd_backend)s
bantime = 2 m
findtime = 6h
maxretry = 1
```
## Metricbeat
### Installation de Metricbeat
```
curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-8.6.2-amd64.deb
sudo dpkg -i metricbeat-8.6.2-amd64.deb
```
### Configuration de Metricbeat 
```
nano /etc/metricbeat/metricbeat.yml
```
> metricbeat.yml
```
=========================== Modules configuration ============================
reload.enabled: true
reload.period: 10s

================================= Dashboards =================================
setup.dashboards.enabled: true

=================================== Kibana ===================================
host: "4.233.61.203:5601"

---------------------------- Elasticsearch Output ----------------------------

output.elasticsearch:
  hosts: ["4.233.61.203:9200"]
  
  protocol: "https"

  username: "elastic"
  password: "YMmjKK_t_9KubA_8F4P7"
  ssl.verification_mode: none
```
### Initialisation de Metricbeat
```
metricbeat setup -e
```
```
systemctl start metricbeat
systemctl enable metricbeat
```
## Filebeat
### Installation de Filebeat
```

```
