
- Mise en place debian 11 non-graphique
- update + upgrade du systeme 

   **installation du serveur Apache2**
   
- Création d'un utilisateur "intranet-server" pour eviter d'exposer le compte "root" lors de l'utilisation du serveur apache2
- Ajout de l'utilisateur "intranet-server" au groupe www-data pour permettre l'execution du server apache2
- Utiliser la commande >> sudo adduser intranet-server www-data
- Verifier que l'utilisateur fait bien parti du groupe >> id intranet-server

- Création des dossiers log/ et /www  au niveau de la home de l'utilisateur intranet-server

- Création d'une save du fichier de conf : ```000-default.conf en 000-default.conf.bak```

- Mise en place de l'**authentification basique** pour acceder au ressources heberger par le site via              l'utilisateur     **"user" avec le mdp: "root"**
- Utilisez la commande suivante pour crée un utilisateur>> Sudo htpasswd -c /etc/apache2/.htpasswd "username      voulu"
- Pensez à supprimer le -c pour ajouter les utilisateurs suivant sans owerwrite l'user

- Modifier le fichier de configuration  ```nano /etc/apache2/sites-enabled/000-default.conf"``` 

```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    <Directory "/var/www/html">
        AuthType Basic
        AuthName "Restricted Content"
        AuthUserFile /etc/apache2/.htpasswd
        Require valid-user
    </Directory>
</VirtualHost>
```

 - **Cacher la version du server apache2** 

 - En modifiant le fichier de conf de securité : sudo nano /etc/apache2/conf-enabled/security.conf

 - Pour cacher la version du server:
  
  ServerTokens **OS**  par ServerTokens **Prod** 

**Ameliorations à proposer :**

- Masquer la mention "Apache Server", il faut venir remplacer :

  ServerSignature **On** par ServerSignature **Off** 

 - Ensuite on redémarre le service:

  ```sudo systemctl restart apache2```

  **création de la CA avec openssl**
  
  - On génére la clé qui servira pour la CA 
  
   ```openssl genrsa -des3 -out ca.key 4096```a   
    passphrase : root
   
   ```openssl req -new -x509 -days 365 -key ca.key -out ca_cert.pem```
    
   **Création des clés pour les certifs du server http**
    ```openssl genrsa -des3 -out apache_server.key 4096```
    
   **Création de certificat server**
    ```openssl req -new -key apache_server.key -out apache_server.csr```
    
      Dans Organisation name name HARIBO et "intra.haribo.lan" dans Common Name
    
   **Trusted du certificat server grace aux clé/certif de la CA
      
    ```openssl x509 -req -days 365 -in apache_server.csr  -CA ca_cert.pem -CAkey ca.key  -CAcreateserial -out apache_server.crt (modifié)```
  
    Il faut **activer le protocole SSL** 
    
    ```sudo a2enmod ssl```
    
    Modification du fichier de config du virtual host "443"
    
    
   ```<IfModule mod_ssl.c>
        <VirtualHost _default_:443>
                ServerAdmin webmaster@localhost
                DocumentRoot /var/www/html/WebGoatPHP
          
                ErrorLog ${APACHE_LOG_DIR}/error.log
                CustomLog ${APACHE_LOG_DIR}/access.log combined


                #   SSL Engine Switch:
                #   Enable/Disable SSL for this virtual host.
                SSLEngine on

                SSLCertificateFile      /etc/ssl/certs/apache_server.crt
                SSLCertificateKeyFile /etc/ssl/private/apache_server.key

                #   Server Certificate Chain:
                #SSLCertificateChainFile /etc/apache2/ssl.crt/server-ca.crt

                #   Certificate Authority (CA):             
                SSLCACertificatePath /etc/ssl/certs/
                SSLCACertificateFile /etc/ssl/ca_cert.pem
          
                <FilesMatch "\.(cgi|shtml|phtml|php)$">
                                SSLOptions +StdEnvVars
                </FilesMatch>
                <Directory /usr/lib/cgi-bin>
                                SSLOptions +StdEnvVars
                </Directory>

        </VirtualHost>
</IfModule>```
    
    
    
    
