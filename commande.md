# COMMANDE
```
sudo apt install default-jdk 
sudo apt install gpg
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update
sudo apt install elasticsearch
sudo nano /etc/elasticsearch/elasticsearch.yml
```
### elasticsearch.yml
```
network.host: 0.0.0.0
discovery.seed_hosts: ["194.69.103.254", "0.0.0.0"]
```
### Azure
NSG : règle entrante sur le port 9200
### Index 
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
