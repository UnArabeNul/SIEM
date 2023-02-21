
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

**ServerTokens OS  par ServerTokens Prod** pour cacher la version du server 

**Ameliorations à proposer :**

- Masquer la mention "Apache Server", il faut venir remplacer :

**ServerSignature On par ServerSignature Off** 

- Redémarrer le service
  sudo systemctl restart apache2



