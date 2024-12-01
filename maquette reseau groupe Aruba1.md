Maquette réseau groupe de Ryan Amsellem-Bousignac :

État de la maquette réseau :
Notre maquette réseau sur Cisco est actuellement fonctionnelle, avec plusieurs éléments clés configurés. Voici ce qui a été réalisé :

Les VLAN nécessaires sont créés et les PC peuvent se pinguer.
Un serveur DHCP est mis en place sur les switches L3. Nous avons décidé de le configurer sur les switches L3 car le serveur DHCP nous a créé des problèmes. Les switches L3 pouvant assumer cette fonctionnalité, nous avons préféré cette option qui a été plus simple et qui fonctionne. Les adresses IP sont bien attribuées aux différents équipements du réseau.
Le protocole HSRP est configuré sur les switches L3, sur les différents VLAN, afin d'assurer la redondance et la disponibilité de la passerelle.

Le seul élément manquant pour la maquette est la mise en place des ACL. Toutefois, nous avons rencontré des difficultés lors de la configuration des ACL : à chaque fois que nous les définissions, soit le DHCP ne fonctionnait plus, soit les pings étaient bloqués entre les équipements. Les ports 67 et 68 étaient bien ouverts afin de laisser passer le trafic DHCP, mais cela ne fonctionnait toujours pas comme attendu, ou bien les règles ne s'appliquaient pas.

Nous avions également trouvé des tutoriels indiquant qu'il faudrait utiliser des VACL (VLAN Access Map), mais la version de nos switches L3 ne permettait pas d'appliquer cette configuration correctement. Même lorsque j'appliquais les ACL directement sur les interfaces de VLAN, le DHCP était encore bloqué.

https://www.ciscomadesimple.be/2015/07/28/vlan-access-list-vacl/

Ci-dessous, je vous partage quelques commandes pour la configuration des ACL, que je maîtrise, afin de montrer ma compréhension :

```
ip access-list extended VLAN16-19 // créer une access-list
 permit ip 10.10.18.0 0.0.0.255 any // permet de communiquer
 permit udp any eq bootps any eq bootpc // permet le passage des requêtes DHCP
 deny ip any any // aucun trafic ne passe
```
Ce sont les commandes que nous avons utilisées dans notre maquette en les personnalisant bien sûr pour notre usage. Mais comme expliqué plus tôt, elles n’ont jamais fonctionné complètement.
Nous avons préféré vous montrer une maquette fonctionnelle avec ce que nous avions fait. Cependant, nous connaissons parfaitement la logique pour les ACL, et si nous avions trouvé d’où venait l’erreur, nous aurions pu les ajouter à notre maquette.
