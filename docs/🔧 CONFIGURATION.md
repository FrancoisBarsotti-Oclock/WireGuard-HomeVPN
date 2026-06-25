# 🔧 Configuration

## 📖 Objectif

Ce document décrit la configuration détaillée du serveur WireGuard ainsi que les choix techniques retenus pour permettre un accès distant sécurisé au réseau local.

L'objectif est de comprendre le rôle de chaque paramètre utilisé dans la configuration, plutôt que de simplement reproduire les commandes d'installation.

---

# 📁 Structure des fichiers

Le serveur WireGuard repose principalement sur le fichier suivant :

```text
/etc/wireguard/wg0.conf
```

Ce fichier contient :

* la configuration de l'interface VPN ;
* les clés cryptographiques ;
* les règles de routage ;
* les clients autorisés (Peers).

---

# 🖥️ Configuration de l'interface WireGuard

Exemple de configuration :

```ini
[Interface]
Address = 10.10.10.1/24
ListenPort = 51820
PrivateKey = <clé privée du serveur>

PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; \
         iptables -A FORWARD -o wg0 -j ACCEPT; \
         iptables -t nat -A POSTROUTING -o wlo1 -j MASQUERADE

PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; \
           iptables -D FORWARD -o wg0 -j ACCEPT; \
           iptables -t nat -D POSTROUTING -o wlo1 -j MASQUERADE
```

---

# 🧩 Description des paramètres

| Paramètre      | Description                                                             |
| -------------- | ----------------------------------------------------------------------- |
| **Address**    | Adresse IP du serveur sur le réseau VPN.                                |
| **ListenPort** | Port UDP utilisé par WireGuard (51820).                                 |
| **PrivateKey** | Clé privée du serveur. Elle ne doit jamais être divulguée.              |
| **PostUp**     | Règles appliquées automatiquement lors du démarrage de l'interface VPN. |
| **PostDown**   | Suppression automatique des règles lors de l'arrêt de l'interface.      |

---

# 🔀 Routage IPv4

Le serveur agit comme un routeur entre deux réseaux :

| Interface | Réseau         |
| --------- | -------------- |
| **wg0**   | 10.10.10.0/24  |
| **wlo1**  | 192.168.1.0/24 |

Le paramètre :

```text
net.ipv4.ip_forward=1
```

autorise Linux à transférer les paquets entre ces deux réseaux.

---

# 🌍 Traduction d'adresses (NAT)

Le NAT est assuré par la règle suivante :

```bash
iptables -t nat -A POSTROUTING -o wlo1 -j MASQUERADE
```

Cette règle remplace l'adresse IP source des clients VPN par l'adresse IP locale du serveur afin que les réponses Internet puissent être correctement routées via la Livebox.

---

# 🔐 Gestion des clients (Peers)

Chaque appareil autorisé possède :

* une paire de clés publique/privée ;
* une adresse IP VPN dédiée ;
* une entrée spécifique dans `wg0.conf`.

Exemple :

```ini
[Peer]
PublicKey = <clé publique du client>
AllowedIPs = 10.10.10.2/32
```

Chaque client est isolé et identifié individuellement.

---

# 📋 Plan d'adressage VPN

| Équipement         | Adresse VPN |
| ------------------ | ----------: |
| Serveur Linux Mint |  10.10.10.1 |
| Galaxy S25 Ultra   |  10.10.10.2 |
| Galaxy S20 FE      |  10.10.10.3 |
| Laptop Windows     |  10.10.10.4 |

Cette organisation facilite l'ajout de nouveaux équipements et le diagnostic des connexions.

---

# 🔒 Protection des fichiers sensibles

Le fichier :

```text
/etc/wireguard/wg0.conf
```

contient la clé privée du serveur.

Les permissions sont donc limitées à l'utilisateur `root` :

```bash
sudo chmod 600 /etc/wireguard/wg0.conf
```

Cette configuration applique le **principe du moindre privilège** afin d'empêcher tout utilisateur non autorisé d'accéder aux informations sensibles.


|Chiffre|Propriétaire|Groupe|Autres|
|---|---|---|---|
|**6**|Lecture + Écriture (`rw-`)|||
|**0**||Aucun droit (`---`)||
|**0**|||Aucun droit (`---`)|


---

# ✅ Vérifications de configuration

Après chaque modification, les contrôles suivants permettent de valider la configuration :

```bash
sudo wg
```

Affiche :

* les interfaces actives ;
* les clients déclarés ;
* les handshakes ;
* les volumes de données échangés.

```bash
ip addr show wg0
```

Vérifie la configuration de l'interface VPN.

```bash
iptables -L
```

Contrôle les règles de filtrage.

```bash
iptables -t nat -L
```

Contrôle les règles de traduction d'adresses.

---

# 🎯 Résultat attendu

Une fois la configuration terminée :

* le serveur WireGuard écoute sur le port UDP 51820 ;
* les clients VPN peuvent établir un tunnel sécurisé ;
* le routage IPv4 est opérationnel ;
* le trafic Internet transite via la Livebox grâce au NAT (MASQUERADE) ;
* chaque client est identifié par une paire de clés et une adresse IP dédiée.

# 📋 Bonnes pratiques
* 🔑 Ne jamais partager une clé privée.
* 🔒 Protéger wg0.conf avec chmod 600.
* 📌 Attribuer une adresse VPN unique à chaque client.
* 📝 Documenter chaque nouveau peer ajouté.
* 🔄 Sauvegarder régulièrement le fichier wg0.conf.

---
