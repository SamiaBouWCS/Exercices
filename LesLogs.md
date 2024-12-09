# Étape 1 : Installer un serveur web Apache
## Installation d'Apache
Commandes à exécuter sur la machine virtuelle (se connecter en tant que root ou d'utiliser sudo).

**Mettre à jour les paquets :**
sudo apt update

**Installer Apache :**
sudo apt install apache2

**Démarrer et activer Apache :**

sudo systemctl start apache2
sudo systemctl enable apache2

Les logs Apache sont situés dans /var/log/apache2/ :

Accès : /var/log/apache2/access.log
Erreurs : /var/log/apache2/error.log
Ces fichiers sont configurés dans les fichiers de configuration d'Apache, mais ils sont par défaut déjà en place.

Vérification et ajustement la configuration dans le fichier de configuration :
Le fichier est situé dans /etc/apache2/apache2.conf.


# Étape 2 : Générer du trafic vers le serveur web
Pour générer du trafic sur le serveur web, il faut utiliser un navigateur ou des outils en ligne de commande comme curl pour envoyer des requêtes HTTP au serveur.

Utilisation de curl : Tu peux exécuter plusieurs requêtes HTTP pour simuler du trafic :

Requête simple réussie (code 200) :
curl http://localhost

Requête d’une page inexistante (erreur 404) :
curl http://localhost/404page
Requête avec des headers personnalisés ou redirections :

curl -I http://localhost

Utilisation du navigateur :
Ouvrir le navigateur et visite plusieurs pages du serveur, en accédant à des URLs existantes et inexistantes pour générer différents types de requêtes.

# Étape 3 : Analyser les logs générés
Les logs seront enregistrés dans les fichiers suivants : **Apache : /var/log/apache2/access.log et /var/log/apache2/error.log**

Analyse des logs à l’aide de commandes comme grep, awk, et sort.

Identifier les requêtes réussies (code 200)
Les requêtes avec le code 200 OK sont considérées comme réussies.

**grep " 200 " /var/log/apache2/access.log**

Cela retourne toutes les requêtes ayant répondu avec un code HTTP 200.

Les erreurs 404 (page non trouvée)
Les erreurs 404 sont celles où la page demandée n’a pas été trouvée.

Apache :
**grep " 404 " /var/log/apache2/access.log**
Cela permet de voir toutes les erreurs 404 dans les logs d'accès.

**Identifier les adresses IP les plus fréquentes**
Pour connaître les adresses IP qui génèrent le plus de requêtes, voici la commannde qui le permet : 

awk '{print $1}' /var/log/apache2/access.log | sort | uniq -c | sort -nr | head -n 10

Cela donne les 10 adresses IP les plus fréquentes, avec le nombre de fois où elles sont apparues dans les logs.

