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
  
  ### Etape 1.2-Réseau public de l'entreprise (DMZ)
  
  ### Etape 1.3-Interconnexion des réseaux de l'entreprise
  
  ### Questions
  
  #### Question 1.1
  
  #### Question 1.2
  
  #### Question 1.3
  
  ## Partie II: Interconnexion avec le "reste du monde"
  
  ### Etape 2.1-Mise en place d'un routeur vers l'extérieur
  
  ### Questions
  
  #### Question 2.1
  
  #### Question 2.2
  
  ## Partie III: Communication du réseau privé avec les serveurs web
  
 ### Etape 3.1-Réseau privé
 
 ### Questions
 
 #### Question 3.1
 
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
  
  ### Etape 6.1-Proxy cache web
  
  Un proxy ou également appelé serveur mandataire permet de suivre les échanges réseaux entre un client et un serveur, cela va permettre de garantir l'anonymat ou de crypter les données.
  
  Pour cela nous allons configurer un proxy http (on peut également configurer un proxy en ftp)
  
  --> Installation d'un proxy
  
  privoxy
  https://doc.ubuntu-fr.org/privoxy
  
  à installer sur le routeur 
  
  https://doc.ubuntu-fr.org/proxy_terminal
  
  
  
  ### Etape 6.2-Proxy cache transparent
  
  ### Questions
  
  #### Question 6.1
  
  #### Question 6.2
  
  #### Question 6.3
  
  ## Partie VII : DNS
  
  ### Etape 7.1-Espace de nommage
  
  ### Etape 7.2-Serveur DNS
  
  ### Etape 7.3-Service global
  
  ### Questions
  
  #### Question 7.1
  
  #### Question 7.2
