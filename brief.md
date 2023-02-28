Installation d'une debian 11 non-graphique effectuer l'update + upgrade du systeme 

  1/ **installation du serveur Apache2**
   
 Création d'un utilisateur "intranet-server" pour eviter d'exposer le compte "root" lors de l'utilisation du serveur apache2
 
 Ajout de l'utilisateur "intranet-server" au groupe www-data pour permettre l'execution du server apache2

 ```sudo adduser intranet-server www-data```
 
 Verifier que l'utilisateur fait bien parti du groupe :
 
 ```id intranet-server```

 Création des dossiers log/ et /www  au niveau de la home de l'utilisateur intranet-server

  2/ **authentification basique**
   
 Pour acceder au ressources heberger par le site via l'utilisateur ------ "user" avec le mdp: "root"
 
 Utilisez la commande suivante pour crée un utilisateur:
 
 ```Sudo htpasswd -c /etc/apache2/.htpasswd "username voulu"```
 
**Pensez à supprimer le -c pour ajouter les utilisateurs suivant sans overwrite l'user**

 Modifier le fichier de configuration 80 
 
 ```nano /etc/apache2/sites-enabled/000-default.conf``` 

```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /home/intranet-server/www
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    <Directory "/home/intranet-server/www">
        AuthType Basic
        AuthName "Restricted Content"
        AuthUserFile /etc/apache2/.htpasswd
        Require valid-user
        Options Indexes Followsymlinks MultiViews
    </Directory>
</VirtualHost>
```

Le changement au niveau du DocumentRoot + Directory + Options Indexes ....  permet le direct listing du dossier www hebergé dans la home de l'utilisateur


 3/ **Cacher la version du server apache2** 

  En modifiant le fichier de conf de securité : 
  
  ```sudo nano /etc/apache2/conf-enabled/security.conf```

  Pour cacher la version du server:
  
  ServerTokens **OS**  par ServerTokens **Prod** 

          -------------------Ameliorations à proposer-------------------------

  Masquer la mention "Apache Server", il faut venir remplacer :

  ServerSignature **On** par ServerSignature **Off** 
  
          --------------------------------------------------------------------
  
  Ensuite on redémarre le service:

  ```sudo systemctl restart apache2```

4/ **mise en place de la PKI**

  - création de la CA (root) avec openssl
  
      On génére la clé qui servira pour la CA (root) et le certificat
  
   ```openssl genrsa -des3 -out ca.key 4096```    passphrase : root
   
   ```openssl req -new -x509 -days 365 -key ca.key -out ca_cert.pem```
   
   
   - création de la CA (inter) avec openssl
  
      On génére la clé qui servira pour la CA (inter) et le certificat
  
   ```openssl genrsa -des3 -out inter_ca.key 4096```    passphrase : root
   
   ```openssl req -new -x509 -days 365 -key inter_ca.key -out inter_ca_cert.pem```
   
   
    
  - On génére la clé qui servira pour le server apache2 et le certificat
   
   ```openssl genrsa -des3 -out apache_server.key 4096```
     
    ```openssl req -new -key apache_server.key -out apache_server.csr```
    
      Dans Organisation name HARIBO et "intra.haribo.lan" dans Common Name
      
      
    
   - Trusted du certificat server grace aux clé/certif de la CA (inter)
      
    ```openssl x509 -req -days 365 -in apache_server.csr  -CA inter_ca_cert.pem -CAkey inter_ca.key -CAcreateserial -out 				apache_server.crt```
    
    
    La CA root (clé+ certif) à été creée, il faut crée une autorité intermediaire (clé+certif signé par la CA root) et enfin cré 	un certificat server (clé + certif signé par la CA intermediaire) 
    
  
    
   - activation du module ssl
    
     ```sudo a2enmod ssl```

  
     Modification du fichier de conf 443
		 
        ```<VirtualHost _default_:443>
                ServerAdmin webmaster@localhost
                DocumentRoot **/var/www/html/WebGoatPHP**
          
                ErrorLog ${APACHE_LOG_DIR}/error.log
                CustomLog ${APACHE_LOG_DIR}/access.log combined


                #   SSL Engine Switch:
                #   Enable/Disable SSL for this virtual host.
                SSLEngine on

                SSLCertificateFile      **/etc/ssl/certs/apache_server.crt**
                SSLCertificateKeyFile **/etc/ssl/private/apache_server.key**

                #   Server Certificate Chain:
                #SSLCertificateChainFile /etc/apache2/ssl.crt/server-ca.crt

                #   Certificate Authority (CA):             
                SSLCACertificatePath /etc/ssl/certs/
                SSLCACertificateFile **/etc/ssl/ca_cert.pem**
          
                <FilesMatch "\.(cgi|shtml|phtml|php)$">
                                SSLOptions +StdEnvVars
                </FilesMatch>
                <Directory /usr/lib/cgi-bin>
                                SSLOptions +StdEnvVars
                </Directory>

        </VirtualHost>```
    
    Rajout du chemin dans le DocumentRoot est le dossier contenant notre site accesible en https
    
    
