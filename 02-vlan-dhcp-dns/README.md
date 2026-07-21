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

## 🔍 Résultats obtenus

- ✅ Trois VLAN correctement isolés au niveau de la couche 2
- ✅ Communication inter-VLAN fonctionnelle grâce au routage Router-on-a-Stick
- ✅ Attribution automatique des adresses IP par DHCP, avec passerelle et DNS correctement distribués
- ✅ Résolution du nom de domaine interne via le serveur DNS

## 🧠 Compétences mises en pratique

- Segmentation réseau (VLAN) et sécurité par isolation du trafic
- Configuration de liaisons trunk et tagging 802.1Q
- Routage inter-VLAN (Router-on-a-Stick, sous-interfaces)
- Configuration et administration de services DHCP et DNS
- Méthodologie de conception réseau (plan d'adressage avant implémentation)

## 🔒 Note sur la sécurité

Ce projet est réalisé dans un environnement de simulation isolé (Cisco Packet Tracer), sans lien avec un réseau réel ou des données sensibles.

## 📌 Prochaines étapes

- Ajout de listes de contrôle d'accès (ACL) pour restreindre certains flux entre VLAN
- Mise en place du protocole STP pour la redondance
- Exploration du VTP (VLAN Trunking Protocol) pour la gestion centralisée des VLAN

---
*Projet réalisé dans le cadre de mon apprentissage autonome en sécurité réseau.*