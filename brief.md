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

*Modifier dans le fichier de conf avec nano /etc/elasticsearch/elasticsearch.yml :*
```
network.host: "0.0.0.0"
```

*Pour configurer Elasticsearch afin qu'il démarre automatiquement au démarrage du système, exécutez les commandes suivantes :*
```
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable elasticsearch.service
```
*Elasticsearch peut être démarré et arrêté comme suit :*
```
sudo systemctl start elasticsearch.service
```

*Tester elasticsearch :*
```
curl -k -u elastic:<mdp> https://localhost:9200
```
# Installer Kibana

*Installer le package Kibana Debian avec :*
```
sudo apt-get update && sudo apt-get install kibana
```

*Modifier dans le fichier de conf avec nano /etc/kibana/kibana.yml :*
```
server.host: "0.0.0.0"
```

*Exécutez Kibana avec systemd*

*Pour configurer Kibana afin qu'il démarre automatiquement au démarrage du système, exécutez les commandes suivantes :*
```
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable kibana.service
```
*Kibana peut être démarré et arrêté comme suit :*
```
sudo systemctl start kibana.service
sudo systemctl stop kibana.service
```

*Dans son navigateur saisir :*
```
http://4.233.61.203:5601
```

*Pour obtenir son enrollment token il faut se déplacer dans la machine elasticsearch en faisant :*
```
cd /usr/share/elasticsearch/bin
```
*et une fois ici saisir :*
```
./elasticsearch-create-enrollment-token --scope kibana
```

On peut maintenant copier coller le token obtenu dans la page kibana du navigateur 

*Un code de vérification est maintenant demandé. Pour l'obtenir il faut se placer dans la machine kibana en faisant :*

```
cd /usr/share/kibana/bin
```
*et une fois ici saisir :*
```
./kibana-verification-code
```

On peut maintenant copier coller le code obtenu dans la page kibana du navigateur.

# Installer Metricbeat
*Télécharger Metricbeat*
```
curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-8.6.2-amd64.deb
sudo dpkg -i metricbeat-8.6.2-amd64.deb
```

*Se connecter à la suite Elastic en définissant les informations de connexion en faisant*
```
nano /etc/metricbeat/metricbeat.yml
```
*et définir les paramètres suivants :*
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

*Configurer les ressources avec :*
```
metricbeat setup -e (cela peut durer un certain moment, il faut attendre qu'il soit loaded)
```

*Démarrer Metricbeat*
```
sudo service metricbeat start
```

*Vérifier son status*
```
sudo service metricbeat status
```
