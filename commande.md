# COMMANDE
---
### Elasticsearch 
```
sudo apt install default-jdk 
sudo apt install gpg
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update
sudo apt install elasticsearch
sudo systemctl enable elasticsearch
```
### elasticsearch.yml
```
sudo nano /etc/elasticsearch/elasticsearch.yml
network.host: 0.0.0.0
discovery.seed_hosts: ["194.69.103.254", "0.0.0.0"]
```
### Azure
NSG : règle entrante sur le port 9200
### Index Elasticsearch
```
creation de l'index, et du member et de l'id via postman 
curl --location --request POST 'http://20.111.15.63:9200/authors/member/1' \
--header 'Content-Type: application/json' \
--data-raw '
{
   "pseudo": "PapaMidnight",
   "color": "Vert", 
   "age": 1208993430
    }

'
```
### FileBeat
```
> création de l'index access-http-logs
curl --location --request PUT 'http://20.111.15.63:9200/access-http-logs/' 
```
### Kibana 
```
sudo apt install kibana
sudo systemctl enable kibana
sudo systemctl start kibana
```
### kibana.yml
```
server.port: 5601
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://20.111.15.63:9200"]
```
