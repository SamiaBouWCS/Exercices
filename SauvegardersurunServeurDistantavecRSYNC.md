# Création du dossier à synchroniser
## Commande :
````
$ mkdir -p ~/dossier_sync/sous_dossier1 ~/dossier_sync/sous_dossier2
$ echo "Fichier exemple 1" > ~/dossier_sync/sous_dossier1/fichier1.txt
$ echo "Fichier exemple 2" > ~/dossier_sync/sous_dossier2/fichier2.txt

````
# État initial du dossier
## Commande :
$ ls -R ~/dossier_sync
# Affichage :
dossier_sync:
sous_dossier1  sous_dossier2

dossier_sync/sous_dossier1:
fichier1.txt

dossier_sync/sous_dossier2:
fichier2.txt

# Première synchronisation
## Commande :
$ rsync -avz ~/dossier_sync/ samia@192.168.1.10:/home/samia/sync

# Affichage :
sending incremental file list
./
sous_dossier1/
sous_dossier1/fichier1.txt
sous_dossier2/
sous_dossier2/fichier2.txt

sent 200 bytes  received 60 bytes  260.00 bytes/sec
total size is 24  speedup is 0.10

````
# Vérification et suppression de fichiers sur le serveur ==
## Commande sur le serveur (via SSH) :
$ ls -R /home/samia/
# Affichage :
distant:
sous_dossier1  sous_dossier2

distant/sous_dossier1:
fichier1.txt

distant/sous_dossier2:
fichier2.txt

# Commande :
$ rm /home/samia/sous_dossier1/fichier1.txt

== Deuxième synchronisation ==
# Commande :
$ rsync -avz ~/dossier_sync/ samia@192.168.1.10:/home/samia/sync

# Affichage :
sending incremental file list
sous_dossier1/
sous_dossier1/fichier1.txt

sent 100 bytes  received 40 bytes  140.00 bytes/sec
total size is 24  speedup is 0.10

== Vérification finale ==
# Commande sur le serveur (via SSH) :
$ ls -R /home/samia/
# Affichage :
distant:
sous_dossier1  sous_dossier2

distant/sous_dossier1:
fichier1.txt

distant/sous_dossier2:
fichier2.txt
