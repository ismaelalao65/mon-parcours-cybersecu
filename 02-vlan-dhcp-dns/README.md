# 🌐 Segmentation réseau avec VLAN, DHCP et DNS

Conception et configuration d'un réseau d'entreprise segmenté, avec routage inter-VLAN, attribution automatique d'adresses et résolution de noms internes.

## 🎯 Objectif

Simuler l'infrastructure réseau d'une petite entreprise divisée en plusieurs départements, en appliquant les principes de segmentation, de sécurité réseau et d'automatisation de la configuration IP.

## 🖥️ Environnement technique

- **Outil de simulation** : Cisco Packet Tracer
- **Équipements** : 1 routeur, 1 switch, 6 PC, 1 serveur

## 🏗️ Architecture réseau

| VLAN | Département | Réseau | Passerelle |
|---|---|---|---|
| 10 | Direction | 192.168.10.0/24 | 192.168.10.1 |
| 20 | Comptabilité | 192.168.20.0/24 | 192.168.20.1 |
| 30 | IT (+ serveur) | 192.168.30.0/24 | 192.168.30.1 |

*(voir `screenshots/topologie.png`)*

## ⚙️ Étapes réalisées

1. **Création des VLAN** sur le switch (10, 20, 30), avec nommage par département
2. **Configuration des ports d'accès** — assignation de chaque port au VLAN correspondant
3. **Configuration du port trunk** (802.1Q) entre le switch et le routeur, autorisant les 3 VLAN
4. **Routage inter-VLAN via Router-on-a-Stick** — création de sous-interfaces sur le routeur (`g0/0.10`, `g0/0.20`, `g0/0.30`), chacune avec encapsulation dot1Q et une adresse IP de passerelle
5. **Configuration du service DHCP** sur le routeur — un pool par VLAN, avec exclusion des premières adresses et attribution du serveur DNS
6. **Configuration du serveur DNS** — résolution du nom interne `intranet.entreprise.local` vers l'adresse du serveur
7. **Configuration des PC en client DHCP**
8. **Tests de validation** : attribution IP automatique, ping intra-VLAN, ping inter-VLAN, résolution DNS

## 📸 Captures d'écran

- `screenshots/topologie.png` — schéma général du réseau
- `screenshots/config-vlan-trunk.png` — configuration des VLAN et du trunk sur le switch
- `screenshots/config-routeur.png` — sous-interfaces et configuration DHCP sur le routeur
- `screenshots/test-connectivite.png` — résultats des tests ping inter-VLAN et résolution DNS
- `screenshots/port-security-dhcp-snooping.png` — configuration des mesures de durcissement sur le switch
- `screenshots/acl-test.png` — test de blocage ACL (Comptabilité → Direction bloqué, retour Direction → Comptabilité fonctionnel)
- `screenshots/ssh-config.png` — configuration SSH active et test de connexion (Telnet refusé, SSH accepté)

## 🔍 Résultats obtenus

- ✅ Trois VLAN correctement isolés au niveau de la couche 2
- ✅ Communication inter-VLAN fonctionnelle grâce au routage Router-on-a-Stick
- ✅ Attribution automatique des adresses IP par DHCP, avec passerelle et DNS correctement distribués
- ✅ Résolution du nom de domaine interne via le serveur DNS

## 🛡️ Durcissement sécuritaire

Au-delà du fonctionnement de base, j'ai appliqué plusieurs mesures de sécurité pour rapprocher cette maquette des standards attendus dans un environnement d'entreprise réel :

| Mesure | Équipement | Objectif |
|---|---|---|
| **Port Security** | Switch | Limite chaque port PC/serveur à une seule adresse MAC autorisée ; désactive automatiquement le port en cas de violation |
| **Désactivation des ports inutilisés** | Switch | Réduit la surface d'attaque physique en neutralisant tous les ports non utilisés |
| **DHCP Snooping** | Switch | Empêche l'installation d'un faux serveur DHCP sur le réseau (attaque "rogue DHCP") ; seul le port relié au routeur est déclaré `trusted` |
| **VLAN natif dédié** | Switch + Routeur | Neutralise le VLAN natif par défaut (VLAN 1), une cible connue pour les attaques de type VLAN hopping |
| **ACL inter-VLAN** | Routeur | Applique le principe du moindre privilège : bloque une communication spécifique (VLAN Comptabilité → VLAN Direction) sans casser le retour du trafic légitime |
| **Accès SSH** | Routeur + Switch | Remplace Telnet (identifiants transmis en clair) par une administration chiffrée |
| **Bannière de connexion** | Routeur + Switch | Message d'avertissement légal à toute tentative d'accès |

## 🩺 Difficultés rencontrées et résolues

**1. Blocage bidirectionnel imprévu avec l'ACL**
En bloquant le trafic IP de Comptabilité vers Direction, j'ai constaté que la communication de Direction vers Comptabilité s'est également interrompue. Cause : une ACL bloquant tout le protocole IP bloque aussi les paquets de réponse (ping *echo-reply*), pas seulement les requêtes. Solution : cibler précisément le type de message ICMP concerné (`echo`, c'est-à-dire uniquement les requêtes), pour laisser passer les réponses légitimes.

**2. Panne DHCP après activation du DHCP Snooping**
Après configuration du DHCP Snooping, tous les PC se sont retrouvés avec une adresse APIPA (169.254.x.x), signe d'un échec de communication avec le serveur DHCP. Diagnostic : l'insertion automatique de l'option 82 (option de traçabilité du DHCP Snooping) n'est pas correctement interprétée par le serveur DHCP simulé dans Cisco Packet Tracer, ce qui bloquait silencieusement les échanges. Solution : désactiver spécifiquement l'insertion de cette option (`no ip dhcp snooping information option`), tout en conservant la protection DHCP Snooping active.

## 🧠 Compétences mises en pratique

- Segmentation réseau (VLAN) et sécurité par isolation du trafic
- Configuration de liaisons trunk et tagging 802.1Q
- Routage inter-VLAN (Router-on-a-Stick, sous-interfaces)
- Configuration et administration de services DHCP et DNS
- Méthodologie de conception réseau (plan d'adressage avant implémentation)
- Durcissement sécuritaire d'infrastructure réseau (Port Security, DHCP Snooping, ACL, SSH)
- Diagnostic et résolution d'incidents réseau en environnement de simulation

## 📌 Prochaines étapes

- Mise en place du protocole STP pour la redondance
- Exploration du VTP (VLAN Trunking Protocol) pour la gestion centralisée des VLAN
- Ajout d'une supervision des logs de sécurité (violations Port Security, tentatives DHCP non autorisées)

---
*Projet réalisé dans le cadre de mon apprentissage autonome en sécurité réseau.*