# 🏗️ Architecture

## 📖 Objectif

Cette architecture a été conçue pour fournir un accès distant sécurisé au réseau local à l'aide de WireGuard, tout en conservant une infrastructure simple, économique et facilement reproductible.

Le serveur VPN est hébergé sur un ordinateur Linux Mint situé derrière une Livebox 6. Les clients (Android et Windows) établissent un tunnel VPN chiffré afin d'accéder aux ressources du réseau local et de faire transiter leur trafic Internet via la connexion domestique.

---

# 🌐 Architecture physique

> 📷 **Figure 1 – Architecture physique du projet**

![Architecture Physique](https://github.com/FrancoisBarsotti-Oclock/WireGuard-HomeVPN/blob/main/images/Architecture_Physique.png)

### Description

L'infrastructure repose sur les composants suivants :

| Élément         | Rôle                                    |
| --------------- | --------------------------------------- |
| 🌍 Internet     | Réseau public                           |
| 📡 Livebox 6    | Routeur, NAT/PAT et passerelle Internet |
| 🐧 Linux Mint   | Serveur WireGuard                       |
| 📱 Clients VPN  | Android, Windows 11                     |
| 🏠 Réseau local | Ressources accessibles via le VPN       |

---

# 🔒 Architecture logique

> 📷 **Figure 2 – Fonctionnement du tunnel WireGuard VPN**

![Fonctionnement Tunnel](https://github.com/FrancoisBarsotti-Oclock/WireGuard-HomeVPN/blob/main/images/Fonctionnement_Tunnel_VPN.png)

### Fonctionnement

1. Le client établit un tunnel WireGuard vers le serveur.
2. La Livebox redirige le port UDP 51820 vers le serveur Linux Mint.
3. WireGuard authentifie le client grâce à une paire de clés publique/privée.
4. Linux active le routage IPv4.
5. iptables applique un MASQUERADE afin que le trafic Internet sorte via la Livebox.
6. Les réponses reviennent par le tunnel chiffré jusqu'au client.

---

# 🔀 Routage réseau

> 📷 **Figure 3 – Routage IPv4 et traduction d'adresses (NAT)**

![Routage](https://github.com/FrancoisBarsotti-Oclock/WireGuard-HomeVPN/blob/main/images/Routage%20IPv4%20%26%20NAT.png)

Le serveur joue le rôle de routeur entre deux réseaux :

| Réseau         | Interface |
| -------------- | --------- |
| 10.10.10.0/24  | wg0       |
| 192.168.1.0/24 | wlo1      |

Le routage IPv4 (`net.ipv4.ip_forward=1`) permet de transférer les paquets entre ces deux réseaux.

Le NAT (MASQUERADE) remplace l'adresse source des clients VPN par l'adresse IP locale du serveur afin que la Livebox puisse renvoyer correctement les réponses.

---

# 📋 Plan d'adressage

| Équipement        | Adresse VPN |
| ----------------- | ----------: |
| Serveur WireGuard |  10.10.10.1 |
| Galaxy S25 Ultra  |  10.10.10.2 |
| Galaxy S20 FE     |  10.10.10.3 |
| Laptop Windows    |  10.10.10.4 |

---

# 🛡️ Choix techniques

Les principaux choix réalisés dans cette architecture sont les suivants :

* WireGuard comme solution VPN.
* Linux Mint comme serveur VPN.
* Attribution d'une adresse IP fixe via réservation DHCP.
* Utilisation de NAT/PAT sur la Livebox.
* Routage IPv4 activé.
* Traduction d'adresses avec iptables (MASQUERADE).
* Authentification par clés publiques et privées.
* Configuration indépendante de chaque client VPN.

Cette architecture est facilement transférable vers un Raspberry Pi ou un mini-PC dédié sans modifier la configuration des clients, sous réserve de conserver les mêmes clés et le même nom de domaine (ou la même adresse IP publique).

---
