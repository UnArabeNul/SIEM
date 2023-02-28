# Installer Elasticsearch
*Installer GPG*
```
sudo apt install gpg
```
*Téléchargez et installez la clé de signature publique :*
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
```
*Installation à partir du référentiel APT*
```
sudo apt-get install apt-transport-https
```
```
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
```
*Installer le package Elasticsearch Debian avec :*
```
sudo apt-get update && sudo apt-get install elasticsearch
```
*Récuperer le password après l'installation :*
```
"The generated password for the elastic built-in superuser is : <password>"
```

**Exécuter Elasticsearch avec systemd**

*Pour configurer Elasticsearch afin qu'il démarre automatiquement au démarrage du système, exécutez les commandes suivantes :*
```
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable elasticsearch.service
```
*Elasticsearch peut être démarré et arrêté comme suit :*
```
sudo systemctl start elasticsearch.service
```
