## SAE21
github de depo perso de la sae21

Dans un premier temps j’ai fait un résumé de chaque jour, ou j’ai détaillé rapidement ce que nous avons fait, j’ai pris l’habitude d’utiliser l’application Dictaphone de mon téléphone pour dire les différentes étapes de mon travail, je vais donc dans une deuxième partie expliquer pourquoi dans cette SAE j’ai progressé dans les composantes essentielles. 

25/03/2022            
installations  des différent élèment et mise a jour du rasberry pi 3.
organisation du travail.

29/03/2022
Adressage du réseaux+début de la programmation du cisco. 
1111 1111.1111 1111.1111 1111.1111 1000
248-255 6 ip
10.40.21.248/29
10.40.21.249-10.40.21.254
10.40.21.255
04/04/2022 reorganisations du travaille, distributions des tache a faire, vue que gns3 ne fonctionne pas sur windows 11, installation d'une vm

11/04/2022  09 :34
Sur gns3 constructions du réseau, mise en place des adresse ip et début de la mise en places du serveur DHCP, configurations du switch, toujours un problèmes sur min pc portable, je n’arrives pas a importé le dossier de sauvegarde sur gns3.

15/04/2022        11:52
Mise en place des vlan sur gns3.

15/04/2022 après midi 
Fin de l’installation des vlan. 

21/04/2022 10:30 
présence d’eau dans les sales, je fais le tour de mon groupe pour expliquer les objectifs de la dernière séance qui était de connecté la partie physique et virtuelle ensemble, différente vérification pour vérifier le bon fonctionnement des vlan. 

22/04/2022 
On a eu un problème avec le routeur qui a était résolu, on a connecté les deux parties(virtuel et physique) 

Dans un premier j’ai décidé d’utiliser le logiciel gns3 car cela me permet d’installer tous les types d’appareils et que nous l’utilisons dans une ressource (R201) 
nous avons décidé qu’elle adresse IP nous allons utiliser, nous avons donc choisi pour la partie virtuelle l’adresse 192.168.0.0 avec le masque de sous réseaux 255.255.0.0 mais avec les vlan cela fait : 192.168.vlan.0, donc un masque de 255.255.255.0 ce qui nous permet d’avoir 253 machines dans chacun vlan.

# Il respecte les fondamentaux de la sécurité informatique. 
Vu que je m’occupais de la partie virtuelle j’ai aidé à faire le pare-feu en donnant les adresses IP qui devait sortir du réseau pour aller sur le internet, de plus j’ai fait les vlan en limitant les connexions pour augmenter la sécurité et bloquer des paquets entre les différents vlan. 
# Il utilise une approche rigoureuse pour la résolution des dysfonctionnements. 
Comme tout le monde nous avons eu beaucoup de problèmes, au début j’ai essayé d’importer le projet gns3 sur mon Pc perso. De là j’ai fait face à de nombreux problèmes, j’ai donc suivi les conseils que l’on nous a donnés, je n’essaye pas de faire plusieurs manipulations sans savoir laquelle résout le problème mais à chaque manipulation qui ne résous pas le problème je répartis sur la version précédente. J’ai aussi eu des problèmes avec le routeur virtuel ou les configurations disparaissent, j’ai résolu le problème qui d’après moi était qu'une personne essayée de récupérer les configurations de mon routeur (vu que le projet était enregistré en local sur le PC) j’ai donc fait une sauvegarde sur une clé USB. Depuis le problème a disparu. 
# il respecte les règles métier. 
Là il y a beaucoup d'exemple d’abord pour l’adresse IP des routeurs je lui donne la dernière adresse disponible, j’ai fait apparaître le numéro du vlan dans l’adresse IP.

# Il assure une veille technologique.
J’essaye de me tenir au courant de l’actualité technologique, que ce soit pour des projets (dans le cadre du BUT ou des projets personnels), à titre de culture technique ou pour communiquer avec d’autres personnes du milieu informatique (pour pouvoir suivre les discussions et pas que quelqu’un m’explique de quoi ils parlent).
Pour me tenir au courant de l’actualité informatique et scientifique j’ai plusieurs méthode, je suis des chaînes YouTube de professionnels et de fabricant pour me tenir au courant des nouveautés, je regarde des sites et magazines spécialisés et il m’arrive parfois de me perdre sur internet car je découvre (souvent par hasards) une technologie que je ne comprends pas mais qui m’intéresse. Se qui me permet de donc d’avoir des connaissances sur les autres composantes essentielles.


Dans un premier temps j’ai programmer le router.
J’ai donc allumé l’interface f0/0
  configure terminal 
    interface fastEthernet 0/0
    no shutdown
    exit
    puis j’ai allumé f0/1

    interface fastEthernet 0/1
    ip address dhcp
    no shutdown 
    exit 
   
puis j’ai fait un dhcp

    ip dhcp pool LAN
    default-router 192.168.0.1
    dns-server 10.40.21.249
    network 192.168.0.0 255.255.255.0
    exit
puis j’ai crée les vlan sur le switch et j’ai rentré dans le router :
``` interface FastEthernet 0/0.1    #pour le vlan 1
    ip nat inside
  	encapsulation dot1Q 1
  	ip address 192.168.1.1 255.255.255.0
	no shut
  	exit
  interface FastEthernet 0/0.2    #pour le vlan 2
  	ip nat inside
encapsulation dot1Q 2
  	ip address 192.168.2.1 255.255.255.0
	no shut
  	exit
  interface FastEthernet 0/0.3    #pour le vlan 3
  	ip nat inside
	encapsulation dot1Q 3
  	ip address 192.168.3.1 255.255.255.0
	no shut
  	exit
  interface FastEthernet 0/0.4    #pour le vlan 4
  	ip nat inside
encapsulation dot1Q 4
  	ip address 192.168.4.1 255.255.255.0
	no shut
  	exit
  interface FastEthernet 0/1
  	ip nat outside
  	exit
 ```
   
   
    
les ACL:
 ```
 ip access-list extended VLAN1 INOUT
   permit udp any any eq 67   
   permit udp any any eq 68  
   permit ip host 192.168.2.1 any
   permit tcp 192.168.3.0 0.0.0.7 192.168.2.0 0.0.0.7 eq 22
   deny ip 192.168.4.0 0.0.0.7 192.168.2.0 0.0.0.7
   permit tcp any any eq 80
   permit tcp any any eq 443
   permit icmp any any 
   permit ip any any 
   permit tcp 192.168.2.0 0.0.0.7 192.168.3.0 0.0.0.7  established
   permit tcp any any eq 22
   exit


   ip access-list extended VLAN3INOUT
   permit udp any any eq 67
   permit udp any any eq 68
   permit ip host 192.168.3.1 any
   permit tcp 192.168.2.0 0.0.0.7 192.168.3.0 0.0.0.7  eq 22 established
   permit tcp 192.168.4.0 0.0.0.7 192.168.3.0 0.0.0.7  eq 22 established
   permit tcp 192.168.2.0 0.0.0.7 any established
   permit tcp 192.168.4.0 0.0.0.7 any established
   permit tcp any any eq 80
   permit tcp any any eq 443
   permit icmp any any
   permit ip any any
   permit tcp any any eq 22
   exit


   ip access-list extended VLAN4 INOUT
   permit udp any any eq 67
   permit udp any any eq 68
   permit ip host 192.168.4.1 any
   permit tcp 192.168.3.0 0.0.0.7 192.168.4.0 0.0.0.7 eq 22
   deny ip 192.168.2.0 0.0.0.7 192.168.4.0 0.0.0.7
   permit tcp any any eq 80
   permit tcp any any eq 443
   permit icmp any any 
   permit ip any any
   permit tcp 192.168.4.0 0.0.0.7 192.168.3.0 0.0.0.7  established
   permit tcp any any eq 22
   exit


   ip access-list extended VLAN1 INOUT
   permit udp any any eq 67
   permit udp any any eq 68
   permit ip host 192.168.1.1 any
   permit tcp 192.168.2.0 0.0.0.7 any eq 80
   permit tcp 192.168.3.0 0.0.0.7 any eq 80
   permit tcp 192.168.4.0 0.0.0.7 any eq 80
   permit icmp any any
   permit ip any any
   permit tcp any any established
   exit


   interface FastEThernet 0/0.1
   ip access-group VLAN1 INOUT in
   ip access-group VLAN1 INOUT out
   interface FastEThernet 0/0.2
   ip access-group VLAN2 INOUT in
   ip access-group VLAN2 INOUT out
   interface FastEThernet 0/0.3
   ip access-group VLAN3 INOUT in
   ip access-group VLAN3 INOUT out
   interface FastEThernet 0/0.4
   ip access-group VLAN4 INOUT in
   ip access-group VLAN4 INOUT out
   end
   wr




