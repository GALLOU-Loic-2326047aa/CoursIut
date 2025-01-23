### Résumé du cours sur l’Internet Protocol version 6 (IPv6)

#### **Introduction**
- **Limites de l'IPv4 :**
  - Épuisement des adresses IPv4 malgré les mécanismes comme CIDR et NAT.
  - Exemple : Fin 2019, RIPE NCC a épuisé ses blocs IPv4 disponibles.
- **IPv6 en réponse :**
  - Adresse de 128 bits, permettant environ \( 3.4 \times 10^{38} \) adresses.
  - Chaque millimètre carré de la Terre pourrait avoir 667 millions d'adresses IPv6.

---

#### **Types d'adresses IPv6**
- **Unicast** : Identifie une seule interface, pour des communications point à point.
- **Multicast** : Permet d’envoyer un message à plusieurs interfaces.
- **Anycast** : Envoie un message à la meilleure interface (la plus proche).

---

#### **Représentation textuelle des adresses IPv6**
1. **Format complet** : Huit blocs de 16 bits en hexadécimal, séparés par des deux-points (e.g., `2007:0A8:9:0:0:75C:357:49C3`).
2. **Format compressé** : Regroupements de zéros remplacés par `::` (e.g., `2007:A8::75C:357:49C3`).

---

#### **Adresses IPv6 particulières**
- **Adresse de bouclage** : `::1` (équivalent à `127.0.0.1` en IPv4).
- **Adresse non spécifiée** : `::` (adresse vide).
- **Recommandation RFC 5952** : Utilisation d'une représentation unique pour chaque adresse.

---

#### **Adresses unicast**
- **Adresses locales de lien** : Préfixe `fe80::/64`, utilisées sur un réseau local.
- **Adresses globales** : Préfixe `2000::/3`, routables sur Internet.
- **Adresses locales uniques** : Préfixe `fc00::/7`, utilisées dans des contextes limités (non routables sur Internet).

---

#### **Adresses multicast**
- Préfixe `ff00::/8`.
- Identifiants spécifiques :
  - `ff02::1` : Groupe de tous les nœuds sur un lien local.
  - `ff02::2` : Groupe de tous les routeurs IPv6.
- Exemple : Adresse multicast sollicitée pour `2037::01:800:200e:8c6c` est `ff02::1:ff0e:8c6c`.

---

#### **Auto-configuration d'adresses IPv6**
1. **SLAAC (Stateless Address Autoconfiguration)** : Utilise des messages ICMPv6 de type RA (Router Advertisement).
2. **SLAAC + DHCPv6** : Complète SLAAC avec des paramètres supplémentaires via DHCPv6.
3. **Stateful DHCPv6** : Fournit tous les paramètres IP.

---

#### **Procédure DAD (Duplicate Address Detection)**
- Vérifie l'unicité d'une adresse IPv6 sur un lien.
- Envoie un message ICMPv6 NS (Neighbor Solicitation) à l’adresse multicast sollicitée associée.

---

#### **États d’une adresse IPv6**
1. **Tentative** : Durant le DAD.
2. **Valide** :
   - **Préférée** : Utilisable pour toutes les communications.
   - **Dépréciée** : Utilisable uniquement pour des communications existantes.
3. **Invalide** : Non utilisable après expiration.

---

#### **Configuration manuelle et automatique**
- **Routeurs** : Configuration manuelle (e.g., `ipv6 address`).
- **Hôtes** : Configuration manuelle ou automatique avec SLAAC ou DHCPv6.

---

#### **Protocoles liés à IPv6**
- **ICMPv6** (intègre les fonctionnalités d'ARP et ND en IPv6) :
  - RS (Router Solicitation), RA (Router Advertisement), NS (Neighbor Solicitation), NA (Neighbor Advertisement).
- **NDP (Neighbor Discovery Protocol)** : Utilise les messages ICMPv6 pour découvrir voisins et résoudre adresses.

Ce résumé couvre les concepts fondamentaux d’IPv6 et son fonctionnement, en incluant les différences avec IPv4 et les mécanismes pour gérer les adresses.

Ce document est une présentation détaillée sur **IPv6 et ICMPv6**, en mettant en évidence les améliorations introduites par IPv6 par rapport à IPv4, ainsi que les spécificités techniques des datagrammes IPv6 et des messages ICMPv6.

<h1>2eme partie</h1>
### **Résumé et points-clés :**

#### **Introduction à IPv6 :**
- **Avantages majeurs d’IPv6 :**
  - Résolution de la pénurie d’adresses IP.
  - Suppression du besoin de NAT, ce qui réduit les latences.
  - Simplification et amélioration des performances grâce à des entêtes plus simples.
  - Suppression de la fragmentation au niveau des routeurs intermédiaires.

---

#### **Comparaison entre IPv4 et IPv6 :**
- **Changements dans l’entête :**
  - IPv6 élimine certains champs inutilisés et renomme ou déplace d'autres.
  - Ajout du champ **Flow Label** pour identifier les paquets nécessitant un traitement spécifique.

- **Suppression du checksum dans IPv6 :**
  - IPv6 n'inclut pas de checksum dans l’entête. Cette responsabilité est déléguée aux protocoles de couche supérieure comme TCP et UDP, qui utilisent un pseudo-entête pour calculer leurs checksums.

---

#### **Fragmentation en IPv6 :**
- **Différences avec IPv4 :**
  - En IPv6, la fragmentation est réalisée par la source et non par les routeurs intermédiaires.
  - Les routeurs envoient un message ICMPv6 **Packet Too Big** à la source lorsque la taille du paquet dépasse le MTU du lien suivant.
  - La **découverte du Path MTU (PMTUd)** est fortement recommandée pour éviter la fragmentation.

---

#### **Entêtes d’extension IPv6 :**
- Les entêtes d’extension permettent d’ajouter des fonctionnalités supplémentaires (sécurité, fragmentation, etc.).
- Chaque entête d’extension contient un champ **Next Header** pour indiquer le type d’entête ou de données qui suit.

---

#### **ICMPv6 :**
- **Fonctionnalités ICMPv6 :**
  - Messages d’erreurs (e.g., *Destination Unreachable*, *Packet Too Big*).
  - Messages d’information (e.g., *Echo Request*, *Echo Reply*).
  - Remplace les fonctions ARP et IGMP d’IPv4 via le **Neighbor Discovery Protocol (NDP)**.
  
- **Messages ICMPv6 importants :**
  - **Destination Unreachable (Type 1)** : Indique diverses erreurs, comme l'absence de route ou un port inaccessible.
  - **Packet Too Big (Type 2)** : Utilisé dans la PMTUd pour informer sur la taille maximale des paquets.
  - **Echo Request/Reply (Type 128/129)** : Pour tester la connectivité réseau.
  - **NDP Messages** : Inclut RS, RA, NS, NA et Router Redirect, essentiels pour découvrir les voisins et configurer les adresses.

---

#### **Neighbor Discovery Protocol (NDP) :**
- Protocole essentiel pour découvrir les routeurs, résoudre les adresses IP en adresses MAC, et configurer les adresses.
- Utilise cinq types de messages ICMPv6 : **RS (133)**, **RA (134)**, **NS (135)**, **NA (136)**, et **Router Redirect (137)**.

---

### **Exercice analysé :**
L'exercice présente un datagramme IPv6 avec ses champs principaux :
- **Version** : 6
- **Traffic Class** : 00
- **Flow Label** : 00000
- **Payload Length** : 32 octets
- **Next Header** : 58 (ICMPv6)
- **Hop Limit** : 255
- **Adresse Source** : `fe80::a00:27ff:fe71:109`
- **Adresse Destination** : `ff02::1:ff8d:7a2c`

Le datagramme encapsule un message ICMPv6.

---

### **Conclusion :**
Cette présentation offre une vue d’ensemble technique sur les principales caractéristiques d’IPv6 et de son protocole compagnon, ICMPv6. Elle illustre les améliorations significatives d’IPv6 pour répondre aux limitations d’IPv4, en simplifiant le traitement des paquets et en introduisant des mécanismes avancés comme le NDP et le PMTUd. Les exercices et exemples permettent de comprendre concrètement le format des datagrammes et l’utilisation des messages ICMPv6.

<h1>Exo</h1>
### **TP IPv6 - Résolutions et Explications**

#### **Exercice 1 : Autoconfiguration SLAAC**

**1. Configuration de R1**
```bash
Router(config)#interface G0/0
Router(config-if)#ipv6 address 2022:2222:4444:6666::1/64
Router(config)#interface G0/1
Router(config-if)#ipv6 address 2022:3333:5555:7777::1/64
Router(config)#ipv6 unicast-routing
```

---

**2. Vérifications sur PC0**

a) **Obtenir l’adresse IPv6 de lien local**  
Sur PC0, utilisez la commande :
```bash
ipconfig /all
```
Vous devriez voir une adresse de type **FE80::/10**, qui est l’adresse IPv6 de lien local.

b) **Méthode de génération de l’ID d’interface**  
L’ID d’interface est généré en fonction de l’adresse MAC de l’interface réseau (conversion EUI-64).

c) **Ping de PC0 à partir de Laptop0**  
Utilisez la commande suivante sur **Laptop0** :
```bash
ping FE80::<adresse_de_lien_local_de_PC0>
```
Assurez-vous d’utiliser le **suffixe de l’interface** (e.g., `%interfaceID`).

d) **Ping depuis PC1 vers PC0**  
Le ping échoue car les adresses de lien local sont limitées au même lien réseau (LAN). Elles ne sont pas routables entre deux réseaux différents.

---

**3. Configuration SLAAC sur PC0**

a) **Renouvellement de l’adresse SLAAC**
```bash
ipv6config /renew
```

b) **Vérification du préfixe**  
La nouvelle adresse IPv6 de PC0 doit inclure le préfixe **2022:2222:4444:6666::/64**, qui correspond à celui de l’interface de R1.

c) **Passerelle par défaut**  
La passerelle par défaut est l’adresse IPv6 globale de l’interface du routeur **2022:2222:4444:6666::1**.

---

**4. Répétition pour PC1**  
Réalisez les mêmes étapes pour PC1. Vérifiez que :
- Le préfixe est **2022:3333:5555:7777::/64**.
- La passerelle par défaut est **2022:3333:5555:7777::1**.

---

**5. Ping entre PC1 et PC0 (adresse globale)**  
Assurez-vous que PC1 peut pinguer PC0 en utilisant leurs **adresses globales** et non pas celles de lien local.

---

**6. Détails du processus SLAAC (Mode Simulation)**

a) **Rappel du processus SLAAC**  
- Un appareil envoie une sollicitation de routeur (**Router Solicitation - RS**).
- Le routeur répond avec une annonce de routeur (**Router Advertisement - RA**) contenant le préfixe réseau.
- Le périphérique forme son adresse en combinant le préfixe et son ID d’interface.

b) **Simulation avec filtrage NDP et ICMPv6**  
Configurez le mode pas à pas pour observer les messages échangés.

c) **Configuration automatique de Laptop0**  
Activez l’auto-configuration sur Laptop0.

d) **Premier message envoyé par PC1**  
Type : **Router Solicitation (RS)**  
Adresse de destination : **FF02::2** (adresse multicast pour tous les routeurs).

e) **Premier message du routeur R1**  
Type : **Router Advertisement (RA)**  
Adresse de destination : **FF02::1** (adresse multicast pour tous les nœuds).  
Valeur du préfixe annoncé : **2022:3333:5555:7777::/64**.

f) **Message suivant envoyé par Laptop0**  
Type : **Neighbor Solicitation (NS)**  
Adresse de destination : Adresse IPv6 globale pour vérification de doublon.  
Utilité : S’assurer que l’adresse générée est unique sur le réseau.

g) **Adresse IPv6 globale de Laptop0**  
Formée en combinant le préfixe du RA (**2022:3333:5555:7777::/64**) et l’ID d’interface généré automatiquement.

h) **Formation de l’adresse globale**  
- **Préfixe** : Annoncé par R1 dans le RA.  
- **ID d’interface** : Généré via EUI-64 ou aléatoirement (selon configuration).

---

#### **Exercice 2 : Comparaison des en-têtes IPv4 et IPv6**

1. **Champs supprimés dans IPv6**  
- **Checksum** : Supprimé pour simplifier la vérification d’intégrité.  
- **Identification, Flags, Fragment Offset** : Remplacés par un mécanisme de fragmentation géré uniquement à l’origine.  
- **Options** : Gérées via des extensions.

2. **Champs modifiés**  
- **Taille de l’adresse source et destination** : Augmentée à 128 bits.  
- **Protocol** : Devient **Next Header** pour indiquer le type de l’en-tête suivant.

3. **Nouveaux champs introduits**  
- **Flow Label** : Utilisé pour identifier les flux nécessitant un traitement particulier (QoS).  
- **Extension Headers** : Fournissent des fonctionnalités supplémentaires, comme la sécurité (IPsec) ou le routage.

Ces exercices renforcent votre compréhension d'IPv6 et de ses avantages par rapport à IPv4, notamment en termes de simplicité et d'efficacité.
