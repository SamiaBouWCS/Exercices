# Pour la mise en place du service DNS sur le serveur sur une VM en Windows Server 2019 suivre la procédure dans le lien ci-dessous : 

https://us.informatiweb-pro.net/system-admin/win-server/ws-2012-2012-r2-create-a-dns-server-and-delegate-subdomains.html

``` 1 - Étapes de configuration

Installer le rôle DNS
```

_**Ouvrir le Gestionnaire de serveur**_

Ajouter des rôles et des fonctionnalités :

Dans le Gestionnaire de serveur, cliquez sur "Gérer" puis sélectionnez "Ajouter des rôles et des fonctionnalités".
Assistant Ajout de rôles et de fonctionnalités :

Cliquez sur "Suivant" jusqu'à ce que vous arriviez à la section "Sélectionner les rôles de serveur".
Cochez la case "Serveur DNS".
Cliquez sur "Ajouter des fonctionnalités" si une fenêtre contextuelle apparaît.
Cliquez sur "Suivant" et suivez les instructions pour terminer l'installation.
Configurer le serveur DNS 


Zones de recherche directe :

_**Ouvrir le Gestionnaire DNS**_

Configurer une nouvelle zone :

Dans le Gestionnaire DNS, faire un clic droit sur "Zones de recherche directe" et sélectionner "Nouvelle zone...".
Assistant Nouvelle zone :

Cliquer sur "Suivant".
Sélectionner "Zone principale" et cliquer sur "Suivant".
Sélectionner "Zone de recherche directe" et cliquer sur "Suivant".
Entrer wilders.lan comme nom de la zone et cliquer sur "Suivant".
Sélectionner "Créer un nouveau fichier de zone avec ce nom de fichier" et cliquer sur "Suivant".
Sélectionner "Ne pas autoriser les mises à jour" (ou selon vos besoins) et cliquer sur "Suivant".
Cliquer sur "Terminer" pour créer la zone.
Zones de recherche inersée :

_**Configurer une nouvelle zone :**_
Dans le Gestionnaire DNS, faire un clic droit sur "Zones de recherche inversée" et sélectionner "Nouvelle zone...".
Assistant Nouvelle zone :
Cliquer sur "Suivant".
Sélectionner "Zone principale" et cliquer sur "Suivant".
Sélectionner "Zone de recherche inversée IPv4" et cliquer sur "Suivant".
Entrer votre ID réseau (exemple 172.20.0) cliquer sur "Suivant".
Sélectionner "Créer un nouveau fichier" et cliquer sur "Suivant".
Sélectionner "Ne pas autoriser les mises à jour" (ou selon vos besoins) et cliquer sur "Suivant".
Cliquer sur "Terminer" pour créer la zone.
Configurer les enregistrements DNS

Ajouter des enregistrements de ressource :

Dans le Gestionnaire DNS, développer "Zones de recherche directe" et sélectionner wilders.lan.
Faire un clic droit sur wilders.lan et sélectionner "Nouvel hôte (A ou AAAA)...".
Entrer le nom de l'hôte (par exemple, www) et l'adresse IP correspondante (par exemple 172.20.0.1)
Refaite l'opération pour votre machine cliente du réseau (par exemple cliwin01.wilders.lan sur 172.20.0.10)
Cliquer sur "Ajouter un hôte" puis sur "OK".
Ajouter un enregistrement CNAME :

Faites un clic droit sur wilders.lan et sélectionnez "Nouvel alias (CNAME)...".
Dans la fenêtre "Nouvel alias (CNAME)", entrez dns dans le champ "Nom de l'alias".
Dans le champ "Nom de domaine complet (FQDN) du nom de domaine cible", cliquer sur "Parcourir" et remonter à la racine du domaine
Cliquer sur "OK" pour créer l'enregistrement CNAME.
Vérification des enregistrements

Ouvrir powershell et entrer nslookup www.wilders.lan
Vous devriez voir une réponse indiquant que www.wilders.lan a l'adresse IP 172.20.0.1

Ouvrir powershell et entrer nslookup dns.wilders.lan
Vous devriez voir une réponse indiquant que dns.wilders.lan a l'adresse IP 172.20.0.1 avec l'alias dns.wilders.lan

Ouvrir powershell et entrer nslookup cliwin01.wilders.lan
Vous devriez voir une réponse indiquant que cliwin01.wilders.lan a l'adresse IP 172.20.0.10

Vérification sur la machine distante

Le serveur windows 2022 sur lequel nous travaillons est déjà configuré comme serveur DHCP pour qu'il attribue une adresse IP à notre machine distante, pour qu'il lui attribue un DNS :
Ouvrir le gestionnaire DHCP
Dans IPv4, cliquer droit sur "Options de serveur" et sélectionner "Configurer les options ..."
Sélectionner 006 Serveur DNS
Dans "Nom du serveur", entrer dns.wilders.lans et cliquer sur "Résoudre" ce qui devrait afficher l'ip correspondante (par exemple 172.20.0.1)
Cliquez sur "Ok" et vérifier et démarrer machine distante
Si tout fonctionne correctement, la machine distante aura son IP et DNS attribué automatiquement
Faire les tests nslookup de la partie 4.
