Poc à réaliser, envoie de log important sur elasticsearch

5 vm
```
VM-Server (Allan)  login: intranet-server mdp: root
```
- <del>Pas d'interface graphique</del>
- <del>Accès à internet</del>
- <del>HTTP</del>
- Fail2ban
- Portsentry
- SSL + CA (haribo)
- GPG
- Un antimalware devra être installé sur chaque machine
- <del>Devra obtenir une adresse IP fixe automatiquement</del>
- Porcentry

```
VM-Network (Alexandre)
```
- <del>Pas d'interface graphique</del>
- <del>DHCP</del>
- <del>DNS</del>
- <del>Accès à internet</del>
- <del>Création domaine haribo.lan</del>
- SSH Serveur ( pour l'accès avec VM-User )
- <del>Le domaine devra etre installer sur la VM-Network</del>
- Un antimalware devra être installé sur chaque machine
- <del>L’adresse du serveur DNS devra être automatiquement distribué sur toutes les machines</del>
- Porcentry

```
VM-User (à determiner)
```
- <del>Interface graphique</del>
- <del>Accès à internet</del>
- Un antimalware devra être installé sur chaque machine
- <del>Devra obtenir une adresse IP automatiquement</del>
- Porcentry

```
VM-Gate (Nadir)
```
- <del>Pas d’environnement graphique</del>
- <del>L'adresse IP sera fixe</del>
- <del>Toutes les machines devront pouvoir accéder à internet à travers une Gateway</del>
- <del>Serveur proxy HTTP</del>
- Toute requête vers une page web devra obligatoirement passer par le serveur proxy
- Un antimalware devra être installé sur chaque machine
- Un programme permettant de détecter les IPS effectuant des scans de port devra être installé sans les bannir mais devra les logger dans un fichier.
- Portcentry

```
VM-SIEM (à determiner)
```
- Elasticsearch
- Kibana
- Un antimalware devra être installé sur chaque machine
- Trois utilisateurs ont accès à l’interface de kibana : analyste, client et admin.
- Le compte analyste ne pourra pas créer d’index mais le compte pourra cependant créer des dashboard
- Le compte client ne pourra pas créer de donnée ni de dashboard, il pourra seulement visualiser les dashboard des analystes et les données des index
- Le compte admin aura un accès administrateur sur toutes les fonctionnalités
```
Proposition d'amélioration
```
- Le client est en attente d’une proposition d’amélioration du filtrage proxy
- Le client est en attente d’une proposition d’amélioration de la sécurité de l’intranet.
- Le client est en attente d’une proposition d’amélioration de la sécurité de la connexion à distance aux machines.
