# TP_Reseau
Répertoire documentation TP d'installation d'un réseau d'entreprise

## Sommaire

  * [Partie I: Mise en place du réseau sur les VMs](#partie-i-mise-en-place-du-réseau-sur-les-vms)
  * [Partie II: Interconnexion avec le "reste du monde"](#partie-ii-interconnexion-avec-le-reste-du-monde")
  * [Partie III: Communication du réseau privé avec les serveurs web](#partie-iii-communication-du-réseau-privé-avec-les-serveurs-web)
  * [Partie IV: Mise en place de la sécurité](#partie-iv-mise-en-place-de-la-sécurité)
  * [Partie V: Configuration automatique](#partie-v-configuration-automatique)
  
  ## Partie I: Mise en place du réseau sur les VMs 
  
  `nano /etc/network/interfaces`
  
  ![no data](https://scontent-cdt1-1.xx.fbcdn.net/v/t1.0-9/46485730_471699810022453_3969204257210499072_n.png?_nc_cat=109&_nc_ohc=-6Rf4mF7MnwAQlIKSApKVyDgu1ho83aWX4wOms5IkG4JERouQOqKYepeg&_nc_ht=scontent-cdt1-1.xx&oh=c34b53705cfa8cf25d6d047875223943&oe=5E88683D)
  
  ### Etape 1.1-Réseau privé de l'entreprise
  
  Pour le réseau privé de l'entreprise nous choisissons une adresse privée avec un masque de sous réseau 255.255.255.0 afin de permettre 2^7-2 soit 126 machines de se connecter ce qui est amplement suffisant dans notre architecture
  
  Pour commencer il faut définir un réseau sur l'interface de notre routeur relié à notre réseau local
  
  > éditer le fichier `/etc/network/interfaces`
  
  > configurer une interface avec une adresse statique
  
  >`iface eth1 inet static` 
  
  >   `address 192.168.1.1` --adresse physique de l'interface eth1 qui sera la passerelle du réseau privé
  
  >   `netmask 255.255.255.0` --masque de sous réseau de l'interface eth1
  
  >   `network 192.168.1.0` --Réseau de eth1
  
  ### Etape 1.2-Réseau public de l'entreprise (DMZ)
  
  Pour ce qui est de la zone démilitarisé (DMZ) de notre entreprise nous choisissons un réseau privé différent de celui de la partie privé toujours en /24.
  
  Il faut alors ajouter un autre réseau sur une des interfaces du routeur
  
    >`iface eth2 inet static` 
  
  >   `address 192.168.2.1` --adresse physique de l'interface eth2 qui sera la passerelle du réseau privé
  
  >   `netmask 255.255.255.0` --masque de sous réseau de l'interface eth1
  
  >   `network 192.168.2.0` --Réseau de eth2
  
  

  
  ### Etape 1.3-Interconnexion des réseaux de l'entreprise
  
  ![Reseau entreprise](https://image.noelshack.com/fichiers/2019/50/5/1576194122-capture-du-2019-12-13-00-41-50.png)
  Format: ![Alt Text](url)
  
  Il faut pour cela activer le routage sur notre routeur 
  
  > Editer `/etc/sysctl.conf`
  
  > Décommenter la ligne suivante `net.ipv4.ip_forward=1`
  
  > Valider les nouvelles modifications ajoutés au fichier /etc/sysctl.conf `sysctl -p`
  
  Nos réseaux sont à présent interconnectés car le routeur remplit automatiquement sa table de routage avec l'ensemble des réseaux pour lequel il est interconnecté.
  
  ### Questions
  
  #### Question 1.1
  
  L'intérêt majeur d'avoir une DMZ au sein de sont entreprise est de distinguer le réseau privé et l'ensemble des services qui peuvent être consultés par le reste du monde. 
    
  Le réseau privé de notre DMZ est 192.168.2.0/24
  
  Cependant notre serveur doit pouvoir être rejoint à l'adresse publique 202.202.202.2
  
  pour cela nous devons faire en sorte des masquer tous les paquets venant sur eth2 qui est l'interface directement connecté à la DMZ
  
  `iptables -t nat -A POSTROUTING -o eth2 -j MASQUERADE`
  
  - "-t nat" spécifie que l'on souhaite modifier la table nat
  
  - "-A" pour "append" permet d'ajouter une nouvelle règle
  
  - "POSTROUTING" action après le routage
  
  - "-o iface" s'applique sur l'interface output
  
  - "-j" jump
  
  - "MASQUERADE" le paquet sortant sur l'interface de sortie est masqué par l'adresse de cette même interface
  
  Par la suite il faut spécifier que tous les paquets sortant du réseau privé avec l'adresse 202.202.202.2 de notre serveur soit directement relié à l'adresse privée du serveur
  
  `iptables -t nat -A PREROUTING -i eth1 -d 202.202.202.2 -j DNAT --to-destination 192.168.2.2`
  
  - "PREROUTING" action après le routage
  
  - "-i iface" s'applique sur l'interface input 
  
  - "-d 202.202.202.2" adresse de destination
  
  - "-j DNAT --to-destination 192.168.2.2" redirige vers 192.168.2.2 qui est connu par notre routeur
  
  
  
  
  
  #### Question 1.2
  
  #### Question 1.3
  
  ## Partie II: Interconnexion avec le "reste du monde"
  
  ### Etape 2.1-Mise en place d'un routeur vers l'extérieur
  
  ### Questions
  
  #### Question 2.1
  On doit d’abord ajouter sur les routeurs de chaque entreprise des interfaces qui permettront de les relier au routeur vers l’extérieur (routeur central).
Ensuite sur le routeur central, on rajoutera des routes qui relieront tous les routeurs d’entreprises entre eux (via le routeur central).
Ainsi, chaque entité du réseau public d’une entreprise aura accès aux serveurs webs des autres entreprises.
  
  #### Question 2.2
  Ce sont les réseaux privés de chaque entreprise qui ne communiquent pas avec le reste du monde car ayant des adresses privées, ils ne peuvent pas accéder à l’extérieur.
  
  ## Partie III: Communication du réseau privé avec les serveurs web
  
 ### Etape 3.1-Réseau privé
 
 ### Questions
 
 #### Question 3.1
 
 NAT : Network Address Translation

NAT présente plusieurs avantages. Tout d’abord, le NAT a permis de répondre au problème de pénurie d’adresses sur Internet. Des plages d’adresses ont donc été réservées à une utilisation privée. En effet, tous les réseaux n’ont pas vocation à être accessibles depuis l’extérieur. De plus NAT permet également d’assurer la sécurité d’un réseau interne, en cachant sa topologie invisible au monde extérieur. En général, le NAT est implémenté sur un routeur.
 
 #### Question 3.2
  
  ## Partie IV: Mise en place de la sécurité
  
  ### Etape 4.1-Politique de sécurité
  
  ### Etape 4.2-Mise en place de la politique choisie
  
  ### Questions
  
  #### Question 4.1
  
  #### Question 4.2
  
  ## Partie V: Configuration automatique
  
  ### Etape 5.1-DHCP sur réseau privé
  
  Pour notre serveur DHCP nous avons décidé d'utiliser l'outils isc-dhcp-server fournit sur la distribution de Debian 8.
  
  Pour cela nous devons installer le paquet après avoir mis à jour l'ensemble des dépots APT
  
   `#apt-get update`
   
   `apt-get install isc-dhcp-server`
   
   Par la suite nous devons configurer notre serveur DHCP pour qu'il puisse attribuer dynamiquement les adresses ip à l'ensemble des machines de notre réseau privé
   
   A l'aide de l'éditeur de texte **nano** il faut apporter des modifications au fichier de configuration de notre dhcp présent dans **/etc/dhcp/dhcpd.conf**

puis modifier la section suivante:

> subnet 192.168.1.0 netmask 255.255.255.0 { -- réseau sur lequel s'applique le DHCP

>   range 192.168.1.4 192.168.1.21; -- définition d'une plage d'adresse à attribuer aux clients

>   option routers 192.168.1.1      -- spécifie la passerelle par défaut pour les clients

> }
  
  ### Questions
  
  #### Question 5.1
  
  #### Question 5.2
   
  #### Question 5.3
  
  ## Partie VI : Mise en place d'un proxy web
  
   A présent, notre objectif est de configurer un proxy http (on peut également configurer un proxy en ftp).
Dans un premier temps nous allons mettre en place un proxy cache web, puis un proxy cache transparent.

  ### Etape 6.1-Proxy cache web
  
  #### Installation d'un proxy
  
  Installer Squid3: `apt-get install squid3`
  
  à installer sur le routeur
  
  https://doc.ubuntu-fr.org/proxy_terminal
  
  #### Configuration
  
   Une fois le proxy installé on copie le fichier de configuration par défaut :
      
  `mv /etc/squid3/squid.conf /etc/squid3/squid.conf.old`
  
   On peut supprimer tous les commentaires du fichier, pour rendre l'edition plus facile:
   
   `grep -vE “^#|^$” /etc/squid3/squid.conf.old > /etc/squid3/squid.conf`
   
  #### Bloquer des noms de domaines
   
   Créer un fichier texte où lister les noms de domaines à bloquer.
   
   Puis ajouter les lignes suivantes dans le fichier de configuration :
   
    `acl deny_domain url_regex -i "/etc/squid/denydomain.txt"
    
     http_access deny deny_domain`
   
  #### Logs
  
   Les logs du serveur proxy sont dans le fichier "/var/log/squid3/access.log"
  
  ### Etape 6.2-Proxy cache transparent
  
  ### Questions
  
  #### Question 6.1
  
  Un proxy ou également appelé serveur mandataire permet de suivre les échanges réseaux entre un client et un serveur, cela va permettre de garantir l'anonymat ou de crypter les données.
  
  Différentes utilisations d'un proxy-cache :
  
- obligatoire et explicite : obligation de passer à travers le proxy mais les utilisateurs doivent configurer leurs applications ou navigateurs.

- obligatoire et implicite : « Proxy transparent » : obligation de passer à travers mais les utilisateurs n’ont pas besoin de configurer leur application ou leur navigateur.
  
  #### Question 6.2
  
  #### Question 6.3
  
  ## Partie VII : DNS
  
  ### Etape 7.1-Espace de nommage
  
  ### Etape 7.2-Serveur DNS
  
  ### Etape 7.3-Service global
  
  ### Questions
  
  #### Question 7.1
  
  #### Question 7.2
