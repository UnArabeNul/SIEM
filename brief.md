```
VM-Server (Allan)  login: intranet-server mdp: root
```
- Pas d'interface graphique
- HTTP
- Fail2ban
- Portsentry
- SSL + CA (haribo)
- GPG
- Un antimalware devra être installé sur chaque machine

```
VM-Network (Alexandre)
```
- Pas d'interface graphique
- DHCP
- DNS 
- SSH Serveur ( pour l'accès avec VM-User )
- Le domaine devra etre installer sur la VM-Network
- Un antimalware devra être installé sur chaque machine


```
VM-User (à determiner)
```
- Un antimalware devra être installé sur chaque machine

```
VM-Gate (Nadir)
```
- Pas d’environnement graphique
- Serveur proxy HTTP
- Toute requête vers une page web devra obligatoirement passer par le serveur proxy
- Toutes les machines devront pouvoir accéder à internet à travers une Gateway
- Un antimalware devra être installé sur chaque machine
```
VM-SIEM (à determiner)
```
- Elastic
- 
- Un antimalware devra être installé sur chaque machine
