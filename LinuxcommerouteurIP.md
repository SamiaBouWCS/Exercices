## Configuration des machines en ligne de commande dynamique : 
 **- M10 (10.0.0.10/24 et fd6a:47d5:647d::10/64)**
 
````
sudo ip addr add 10.0.0.10/24 dev eth0
sudo ip addr add fd6a:47d5:647d::10/64 dev eth0
sudo ip route add default via 10.0.0.1
sudo ip -6 route add default via fd6a:47d5:647d::1
 **- M11 (10.0.0.11/24 et fd6a:47d5:647d::11/64)**

sudo ip addr add 10.0.0.11/24 dev eth0
sudo ip addr add fd6a:47d5:647d::11/64 dev eth0
sudo ip route add default via 10.0.0.1
sudo ip -6 route add default via fd6a:47d5:647d::1
 **- M12 (10.0.1.12/24 et fd20:7336:af69::12/64)**

sudo ip addr add 10.0.1.12/24 dev eth0
sudo ip addr add fd20:7336:af69::12/64 dev eth0
sudo ip route add default via 10.0.1.1
sudo ip -6 route add default via fd20:7336:af69::1
 **- M13 (10.0.2.13/24 et fdde:7426:af03::13/64)**

sudo ip addr add 10.0.2.13/24 dev eth0
sudo ip addr add fdde:7426:af03::13/64 dev eth0
sudo ip route add default via 10.0.2.1
sudo ip -6 route add default via fdde:7426:af03::1
 **- R0 (10.0.0.1/24 et 192.168.0.250/24, fd6a:47d5:647d::1/64 et fd6a:47d5:647d::250/64)**

sudo ip addr add 10.0.0.1/24 dev eth0
sudo ip addr add fd6a:47d5:647d::1/64 dev eth0
sudo ip addr add 192.168.0.250/24 dev eth1
sudo ip addr add fd6a:47d5:647d::250/64 dev eth1
sudo ip route add 10.0.1.0/24 via 192.168.0.251
sudo ip route add 10.0.2.0/24 via 192.168.0.252
sudo ip -6 route add fd20:7336:af69::/64 via fd6a:47d5:647d::251
sudo ip -6 route add fdde:7426:af03::/64 via fd6a:47d5:647d::252
 **- R1 (10.0.1.1/24 et 192.168.0.251/24, fd20:7336:af69::1/64 et fd6a:47d5:647d::251/64)**

sudo ip addr add 10.0.1.1/24 dev eth0
sudo ip addr add fd20:7336:af69::1/64 dev eth0
sudo ip addr add 192.168.0.251/24 dev eth1
sudo ip addr add fd6a:47d5:647d::251/64 dev eth1
sudo ip route add 10.0.0.0/24 via 192.168.0.250
sudo ip route add 10.0.2.0/24 via 192.168.0.252
sudo ip -6 route add fd6a:47d5:647d::/64 via fd6a:47d5:647d::250
sudo ip -6 route add fdde:7426:af03::/64 via fd6a:47d5:647d::252
 **- R2 (10.0.2.1/24 et 192.168.0.252/24, fdde:7426:af03::1/64 et fd6a:47d5:647d::252/64)**

sudo ip addr add 10.0.2.1/24 dev eth0
sudo ip addr add fdde:7426:af03::1/64 dev eth0
sudo ip addr add 192.168.0.252/24 dev eth1
sudo ip addr add fd6a:47d5:647d::252/64 dev eth1
sudo ip route add 10.0.0.0/24 via 192.168.0.250
sudo ip route add 10.0.1.0/24 via 192.168.0.251
sudo ip -6 route add fd6a:47d5:647d::/64 via fd6a:47d5:647d::250
sudo ip -6 route add fd20:7336:af69::/64 via fd6a:47d5:647d::251

## Configuration des machines en fichier statique
/etc/network/interfaces
M10 (10.0.0.10/24 et fd6a:47d5:647d::10/64)

auto eth0
iface eth0 inet static
    address 10.0.0.10
    netmask 255.255.255.0
    gateway 10.0.0.1

iface eth0 inet6 static
    address fd6a:47d5:647d::10
    netmask 64
    gateway fd6a:47d5:647d::1
M11 (10.0.0.11/24 et fd6a:47d5:647d::11/64)

auto eth0
iface eth0 inet static
    address 10.0.0.11
    netmask 255.255.255.0
    gateway 10.0.0.1

iface eth0 inet6 static
    address fd6a:47d5:647d::11
    netmask 64
    gateway fd6a:47d5:647d::1
M12 (10.0.1.12/24 et fd20:7336:af69::12/64)

auto eth0
iface eth0 inet static
    address 10.0.1.12
    netmask 255.255.255.0
    gateway 10.0.1.1

iface eth0 inet6 static
    address fd20:7336:af69::12
    netmask 64
    gateway fd20:7336:af69::1

M13 (10.0.2.13/24 et fdde:7426:af03::13/64)

auto eth0
iface eth0 inet static
    address 10.0.2.13
    netmask 255.255.255.0
    gateway 10.0.2.1

iface eth0 inet6 static
    address fdde:7426:af03::13
    netmask 64
    gateway fdde:7426:af03::1
R0 (10.0.0.1/24 et 192.168.0.250/24, fd6a:47d5:647d::1/64 et fd6a:47d5:647d::250/64)

auto eth0
iface eth0 inet static
    address 10.0.0.1
    netmask 255.255.255.0

iface eth0 inet6 static
    address fd6a:47d5:647d::1
    netmask 64

auto eth1
iface eth1 inet static
    address 192.168.0.250
    netmask 255.255.255.0

iface eth1 inet6 static
    address fd6a:47d5:647d::250
    netmask 64

# Ajouter des routes statiques IPv4
up ip route add 10.0.1.0/24 via 192.168.0.251
up ip route add 10.0.2.0/24 via 192.168.0.252

# Ajouter des routes statiques IPv6
up ip -6 route add fd20:7336:af69::/64 via fd6a:47d5:647d::251
up ip -6 route add fdde:7426:af03::/64 via fd6a:47d5:647d::252
R1 (10.0.1.1/24 et 192.168.0.251/24, fd20:7336:af69::1/64 et fd6a:47d5:647d::251/64)

auto eth0
iface eth0 inet static
    address 10.0.1.1
    netmask 255.255.255.0

iface eth0 inet6 static
    address fd20:7336:af69::1
    netmask 64

auto eth1
iface eth1 inet static
    address 192.168.0.251
    netmask 255.255.255.0

iface eth1 inet6 static
    address fd6a:47d5:647d::251
    netmask 64

### Ajouter des routes statiques IPv4
up ip route add 10.0.0.0/24 via 192.168.0.250
up ip route add 10.0.2.0/24 via 192.168.0.252

### Ajouter des routes statiques IPv6
up ip -6 route add fd6a:47d5:647d::/64 via fd6a:47d5:647d::250
up ip -6 route add fdde:7426:af03::/64 via fd6a:47d5:647d::252
R2 (10.0.2.1/24 et 192.168.0.252/24, fdde:7426:af03::1/64 et fd6a:47d5:647d::252/64)

auto eth0
iface eth0 inet static
    address 10.0.2.1
    netmask 255.255.255.0

iface eth0 inet6 static
    address fdde:7426:af03::1
    netmask 64

auto eth1
iface eth1 inet static
    address 192.168.0.252
    netmask 255.255.255.0

iface eth1 inet6 static
    address fd6a:47d5:647d::252
    netmask 64

### Ajouter des routes statiques IPv4
up ip route add 10.0.0.0/24 via 192.168.0.250
up ip route add 10.0.1.0/24 via 192.168.0.251

### Ajouter des routes statiques IPv6
up ip -6 route add fd6a:47d5:647d::/64 via fd6a:47d5:647d::250
up ip -6 route add fd20:7336:af69::/64 via fd6a:47d5:647d::251
