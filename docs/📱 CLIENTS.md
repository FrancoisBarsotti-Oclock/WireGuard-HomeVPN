# 📱 Clients VPN

## 📖 Objectif

Ce document décrit la configuration des différents clients WireGuard pouvant se connecter au serveur VPN.

Chaque appareil dispose :

* d'une paire de clés publique/privée ;
* d'une adresse IP VPN dédiée ;
* d'une entrée spécifique (Peer) dans la configuration du serveur.

Cette approche permet d'identifier individuellement chaque client et de simplifier la gestion des accès.

---

# 🌐 Plan d'adressage

| Équipement         | Adresse VPN | Fichier       |
| ------------------ | ----------: | ------------- |
| Serveur Linux Mint |  10.10.10.1 | `wg0.conf`    |
| Galaxy S25 Ultra   |  10.10.10.2 | `phone.conf`  |
| Galaxy S20 FE      |  10.10.10.3 | `s20.conf`    |
| Laptop Windows     |  10.10.10.4 | `laptop.conf` |

---

# 🔑 Création des clés d'un nouveau client

Chaque nouveau client nécessite une paire de clés dédiée.

```bash
cd ~

wg genkey | tee client_privatekey | wg pubkey > client_publickey
```

Vérification :

```bash
ls -l client_privatekey client_publickey
```

La clé publique est ensuite ajoutée dans le fichier :

```text
/etc/wireguard/wg0.conf
```

Exemple :

```ini
[Peer]
PublicKey = CLE_PUBLIQUE_DU_CLIENT
AllowedIPs = 10.10.10.X/32
```

Après chaque ajout :

```bash
sudo wg-quick down wg0
sudo wg-quick up wg0
```

---

# 🤖 Client Android

Installer l'application WireGuard depuis le magasin d'applications.

Créer ensuite un fichier de configuration :

```ini
[Interface]
PrivateKey = ...
Address = 10.10.10.2/32
DNS = 1.1.1.1

[Peer]
PublicKey = ...
Endpoint = IP_PUBLIQUE:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

Générer un QR Code :

```bash
qrencode -o phone.png < phone.conf
```

Scanner le QR Code avec l'application WireGuard.

![Import du tunnel sur Android]()

> 📷 **Figure 1 – Import du tunnel WireGuard sur Android**

---

# 💻 Client Windows

Installer WireGuard pour Windows.

Importer directement le fichier :

```text
laptop.conf
```

via :

```text
Add Tunnel
    ↓
Import tunnel(s) from file
```

Le tunnel peut ensuite être activé depuis l'interface graphique.

![Tunnel actif sur Win11]()

> 📷 **Figure 2 – Tunnel WireGuard actif sous Windows 11**

---

# 🐧 Client Linux

Le client Linux utilise le même principe.

Installation :

```bash
sudo apt install wireguard
```

Activation :

```bash
sudo wg-quick up NOM_INTERFACE
```

Arrêt :

```bash
sudo wg-quick down NOM_INTERFACE
```

---

# 📋 Paramètres communs

Tous les clients utilisent une configuration similaire.

| Paramètre           | Description                           |
| ------------------- | ------------------------------------- |
| Address             | Adresse IP VPN du client              |
| DNS                 | Serveur DNS utilisé par le client     |
| PublicKey           | Clé publique du serveur               |
| Endpoint            | Adresse IP publique et port UDP 51820 |
| AllowedIPs          | Réseaux accessibles via le tunnel     |
| PersistentKeepalive | Maintien de la connexion (25 s)       |

---

# 🔍 Vérification de la connexion

Une fois le tunnel activé :

```bash
sudo wg
```

doit afficher un **latest handshake**.

Les transferts de données doivent augmenter :

```text
transfer:
```

Le client peut ensuite vérifier son adresse IP publique :

* https://ifconfig.me
* https://whatismyipaddress.com

L'adresse IP affichée doit correspondre à celle de la connexion Internet du domicile.

---

# ➕ Ajouter un nouveau client

Pour ajouter un nouvel appareil :

1. Générer une nouvelle paire de clés.
2. Choisir une nouvelle adresse VPN.
3. Ajouter un Peer dans `wg0.conf`.
4. Créer un fichier de configuration dédié.
5. Recharger WireGuard.
6. Importer le fichier (ou le QR Code) sur le client.

Aucune autre modification du serveur n'est nécessaire.

---

# 🛡️ Bonnes pratiques

* Une paire de clés par appareil.
* Une adresse VPN unique par client.
* Ne jamais partager une clé privée.
* Supprimer immédiatement un Peer en cas de perte ou de vol d'un appareil.
* Conserver les fichiers de configuration dans un emplacement sécurisé.

---

# ✅ Résultat attendu

Chaque client est capable :

* d'établir un tunnel WireGuard sécurisé ;
* d'accéder au réseau local ;
* de faire transiter son trafic Internet via la Livebox ;
* d'être identifié individuellement par le serveur grâce à sa clé publique.

![Plusieurs peers & handshakes]()

> 📷 **Figure 4 : sortie de sudo wg montrant plusieurs peers et leurs handshakes.**

---
