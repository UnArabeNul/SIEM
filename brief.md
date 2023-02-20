Poc à réaliser, envoie de log important sur elasticsearch
```
5 vm
```
VM-Server (Allan)  login: intranet-server mdp: root
```
- Pas d'interface graphique
- Accès à internet
- HTTP
- Fail2ban
- Portsentry
- SSL + CA (haribo)
- GPG
- Un antimalware devra être installé sur chaque machine
- Devra obtenir une adresse IP fixe automatiquement

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

```
VM-User (à determiner)
```
- Interface graphique
- Accès à internet
- Un antimalware devra être installé sur chaque machine
- Devra obtenir une adresse IP automatiquement

```
VM-Gate (Nadir)
```
- Pas d’environnement graphique
- Serveur proxy HTTP
- Toute requête vers une page web devra obligatoirement passer par le serveur proxy
- Toutes les machines devront pouvoir accéder à internet à travers une Gateway
- Un antimalware devra être installé sur chaque machine
- L'adresse IP sera fixe
- Un programme permettant de détecter les IPS effectuant des scans de port devra être installé sans les bannir mais devra les logger dans un fichier.
```
VM-SIEM (à determiner)
```
- Elasticsearch
- Kibana
- Un antimalware devra être installé sur chaque machine
- Trois utilisateurs ont accès à l’interface de kibana : analyste, client et admin.
- Le compte analyste ne pourra pas créer d’index mais le compte pourra cependant créer des dashboard
- Le compte client ne pourra pas créer de donnée ni de dashboard, le compte pourra
seulement visualiser les dashboard des analystes et les données des index
- Le compte admin aura un accès administrateur sur toutes les fonctionnalités
