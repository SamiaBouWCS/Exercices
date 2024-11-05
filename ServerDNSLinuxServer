### Installer Bind9
Dans le terminal de votre VM Ubuntu ou Debian, saisir la commande : **sudo apt install -y bind9 bind9utils bind9-doc dnsutils**

_**Configurer le serveur DNS**_
Editer le fichier /etc/bind/named.conf.options (avec l'éditeur de votre choix) et y insérer:
acl internal-network {
172.20.0.0/24;
};
options {
directory "/var/cache/bind";
allow-query { localhost; internal-network; };
allow-transfer { localhost; };
forwarders { 8.8.8.8; };
recursion yes;
dnssec-validation auto;
listen-on-v6 { any; };
};
Editer le fichier /etc/bind/named.conf.local (toujours avec votre super éditeur) et y insérer:
zone "wilders.lan" IN {
type master;
file "/etc/bind/forward.wilders.lan";
allow-update { none; };
};
zone "0.20.172.in-addr.arpa" IN {
type master;
file "/etc/bind/reverse.wilders.lan";
allow-update { none; };
};

Aller dans le dossier **/etc/bind** et faire une copie du fichier exemple de base de donnée: sudo cp db.local forward.wilders.lan
Editer le fichier nouvellement créé:
```TTL 604800
@ IN SOA primary.wilders.lan. root.primary.wilders.lan. (
2022072651 ; Serial
3600 ; Refresh
1800 ; Retry
604800 ; Expire
604600 ) ; Negative Cache TTL
;Name Server Information
@ IN NS primary.wilders.lan.
;IP address of Your Domain Name Server(DNS)
primary IN A 172.20.0.254

;CNAME Record
dns IN CNAME primary.wilders.lan.
````
Faire pareil avec la plage reverse dns: sudo cp db.127 reverse.wilders.lan
Editer le fichier créé:
TTL 86400
@ IN SOA wilders.lan. root.wilders.lan. (
2022072752 ;Serial
3600 ;Refresh
1800 ;Retry
604800 ;Expire
86400 ;Minimum TTL
)
;Your Name Server Info
@ IN NS primary.wilders.lan.
primary IN A 172.20.0.254
;Reverse Lookup for Your DNS Server
254 IN PTR primary.linuxtechi.local.
Modifier le fichier /etc/default/named avec: OPTIONS="-u bind -4"
Ce qui permet de mettre le serveur en écoute sur IPv4
Activer le service et vérifiez son fonctionnement:
sudo systemctl start named
sudo systemctl enable named
sudo systemctl status named
Test
Sur une autre machine:
Modifier le DNS en modifiant le fichier /etc/resol.conf en y ajoutant:
search wilders.lan
nameserver 172.20.0.254
Taper dans le CLI pour tester le DNS:
dig primary.wilders.lan
Puis taper pour tester le reverse lookup:
dig -x 172.20.0.254
