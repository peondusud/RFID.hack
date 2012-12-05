RFID.hack
=========

Hacking the NFC credit cards for fun and debit par Renaud Lifchitz
==================================================================

#Hackito Ergo Sum 2012 [13/04/2012] 10h-11h


Renaud commence par présenter ses activités au sein de BT et le principe du paiement sans contact (NFC). En France 100 000 terminaux sont déjà compatibles et 10 millions de cartes sont compatibles aux USA. Il existe deux systèmes un par Visa et un pour MasterCard. Le paiement est pour le moment limité à 4 paiements de maximum 20€. Les cartes NFS compatibles ont un logo de signal radio comme le wifi. Dans l’assemblée seule une personne a une carte NFC, sans doute que certains n’ont pas préféré le dire :) . Le but du paiement sans fil est censé simplifié l’utilisation du moyen de paiement et au Canada les statistiques montrent que les gens consomment plus facilement.

Le stockage et la sécurité est assuré par le standard EMV et la norme ISO 7816-4. La carte a un vrai filesystem avec un root directory et des répertoires / fichiers. Les données sont encodées en BER TLV. Renaud analyse les données présentes dans une requête de paiement sans contact. On retrouve une class, une instruction, deux paramètres, la longueur des données, les données et la longueur de la réponse espérée. A présent un parallèle est fait avec le pass Navigo, le ticket sans contact des transports parisien. La sécurité est bonne car aucune donnée personnelle n’est présente, aucun lien ne peut être fait entre une personne et sa carte sauf par les serveurs centraux. La carte possède un bon cryptage, une bonne authentification et une signature digitale.

Renaud Lifchitz at Hackito Ergo Sum 2012

Les cartes de paiement sans contact sont au contraire du pass navigo, aucun cryptage des données, aucune authentification, tout est ouvert. Le NFC regroupe le RFID, le NFC et le Cityzi, les fréquences sont en HF à 13,56Mhz et en LF entre 125 et 134 Khz. On trouve des lecteurs usb et des téléphones capables de lire les NFC. Pour les outils utiles scriptor pour le prototype, libnfc et pn53x-tamashell pour la partie NFC et quelques scripts persos sont nécessaires.

Les données que l’on peut extraire sont les données du propriétaire : sexe, nom, prénom, votre numéro de compte PAN, votre date d’expiration, les données de la bande magnétique, l’historique de vos transactions. Potentiellement d’autres données restent à découvrir comme les clefs publiques. Par contre le numéro de la carte CVV n’est pas inscrit dans les données NFC, on ne peut donc pas le récupérer.

Les attaques possibles, sont le fait de récupérer les données et de les utiliser pour des paiements en ligne. On peut aussi bloquer une carte bleue en envoyant trois requêtes de code pin erronés et ainsi bloquer tout paiement avec cette carte. On peut aussi tenter la duplication de la carte, les données NFC sont presques complètes et les données de la piste magnétique peuvent être reproduites. Une telle carte clonée pourra être utilisé dans les pays ne demandant pas le code pin comme les USA et certains pays européens par exemple. Enfin on peut reconnaître la présence d’une personne et déclencher des actions : surveillance des passages, terrorisme avec une voiture qui explose uniquement si la personne visée est présente.

Renaud aborde la méthode pour faire une attaque avec la libnfc. On envoie une séquence d’initialisation, il faut après retrouver le code AID de l’application de la banque, ce numéro se trouve sur tous les tickets de reçu de carte bleue et n’est pas tronqué. Enfin il faut lire la réponse EMV de la carte. Renaud nous présente les code AID des différents types de paiement visa, mastercard, american express, …

Renaud Lifchitz lecture des informations NFC carte bleue sans contact

Une démonstration est faite avec sa carte bleue dans son porte feuille qu’il approche du lecteur. Instannnément toutes les données se trouvant dans sa carte sont apparues. On y voit son numéro de carte bleue, sa date d’expiration, son identité et l’historique de ses achats. Une deuxième démonstration est faite sur un système Android, les mêmes données apparaissent. On imagine aisément les conséquences de scan avec un téléphone dans un métro bondé.

A présent nous voyons les limites des attaques. La principale contrainte est la distance entre 3 et 5 cm est la distance idéale. Mais en utilisant des amplificateurs on peut monter à 1,5m. Avec un récepteur type USRP avec une antenne directive on peut monter à 15m. Il rappel que des hackers avaient réussi en 2004 à monter la distance opérationnelle du bluetooth de 10m à 1,7km. La seule protection possible est un porte feuille anti RFID capable de bloquer les ondes radios comme une cage de Faraday. Renaud fait à présent les recommendations de ce que devrait être la sécurité sur les cartes NFC. Authentification, cryptage des communications, les sessions doivent être sécurisées. Aussi pour lui le système doit être entièrement revu et ne pas être utilisé. Le paiement NFC n’est pas compatible avec les recommandations PCI DSS alors que ces derniers sont à l’origine du paiement NFC. Aussi c’est un grand échec de la politique de sécurité et de lutte contre la fraude des paiements au niveau internationale. En France le fait de ne pas protéger des données personnelles est un acte criminel, la CNIL est déjà prête à demander des actions concrêtes, aussi l’utilisation de cartes NFC en France semble incompatible.

Renaud termine sur le fait qu’il n’a pas eu besoin de faire de reverse engineering puisque le protocole est public, qu’il n’est entré dans aucun système et n’a cassé aucune sécurité puisqu’il n’y en a simplement aucune.