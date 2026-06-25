# ⚙️ Installation

## 📖 Objectif

Ce document décrit l'installation complète d'un serveur WireGuard auto-hébergé sous Linux Mint, placé derrière une Livebox 6.

À l'issue de cette procédure, le serveur sera capable :

* d'accepter des connexions VPN WireGuard ;
* de fournir un accès distant sécurisé au réseau local ;
* de faire transiter le trafic Internet des clients VPN via la connexion domestique.

---

# 📋 Prérequis

Avant de commencer, vérifier les éléments suivants :

| Élément              | Valeur                       |
| -------------------- | ---------------------------- |
| Distribution         | Linux Mint 22.x              |
| Accès administrateur | sudo                         |
| Connexion Internet   | Fonctionnelle                |
| Adresse IP locale    | Réservation DHCP recommandée |
| Routeur              | Livebox 6                    |
| Port utilisé         | UDP 51820                    |

---

# 1️⃣ Attribution d'une adresse IP fixe

Une adresse IP stable est indispensable afin que la redirection NAT/PAT de la Livebox pointe toujours vers le serveur WireGuard.

### Identifier l'interface réseau

```bash
ip a
```

### Vérifier la passerelle

```bash
ip route
```

Exemple :

```text
default via 192.168.1.1 dev wlo1
```

### Réserver l'adresse IP

Depuis l'interface de la Livebox :

```
Paramètres avancés
    ↓
Réseau
    ↓
DHCP
    ↓
Baux DHCP statiques
```

![DHCP dans Livebox](https://github.com/FrancoisBarsotti-Oclock/WireGuard-HomeVPN/blob/main/images/DHP%20dans%20Livebox.png)

> 📷 **Figure 1 – Réservation DHCP dans la Livebox**

Reconnecter ensuite l'interface réseau puis vérifier :

```bash
hostname -I
```

---

# 2️⃣ Installation de WireGuard

Avec la MàJ et la vérification de version, comme bonnes pratiques de base :

```bash
sudo apt update
# Installer WireGuard et QRencode :
sudo apt install wireguard qrencode -y
# Vérifier la version :
wg --version
```
![Installation des paquets](https://github.com/FrancoisBarsotti-Oclock/WireGuard-HomeVPN/blob/main/images/Installation%20des%20paquets.png)

> 📷 **Figure 2 – Vérfication de version**
---

# 3️⃣ Génération des clés du serveur

Créer les clés :

```bash
# Génération
wg genkey | tee privatekey | wg pubkey > publickey
# y accéder
cd ~
wg genkey | tee privatekey | wg pubkey > publickey
# Afficher la clé privée du serveur
cat privatekey
# Afficher la clé publique du serveur
cat publickey
# Revérifier l'interface réseau
ip route | grep default
```

![Visualisation clés générées](https://github.com/FrancoisBarsotti-Oclock/WireGuard-HomeVPN/blob/main/images/Visualisation%20des%20cl%C3%A9s%20g%C3%A9n%C3%A9r%C3%A9es.png)

> 📷 **Figure 3 – Vérfication de version**

⚠️ La clé privée ne doit jamais être communiquée.

---

# 4️⃣ Configuration du serveur WireGuard

Créer le fichier :

```bash
sudo nano /etc/wireguard/wg0.conf
```

Configurer :

```ini
[Interface]
Address = 10.10.10.1/24
ListenPort = 51820
PrivateKey = ...

PostUp = ...
PostDown = ...
```

![Configuration de wg0.conf](https://github.com/FrancoisBarsotti-Oclock/WireGuard-HomeVPN/blob/main/images/Configuration%20de%20wg0.conf.png)

> 📷 **Figure 4 – Configuration de `wg0.conf`**

---

# 5️⃣ Protection des clés

on cherche à modifier les permissions du fichier `wg0.conf`, avec `sudo chmod 600 /etc/wireguard/wg0.conf`, pour qu'il puisse être lu et modifié uniquement par root (-rw------- 1), de façon à ce que la clé privée du serveur soit bien protégée par le **principe du moindre privilège**:


|**Chiffre**|**Propriétaire**|**Groupe**|**Autres**|
|---|---|---|---|
|**6**|Lecture + Écriture (`rw-`)|||
|**0**||Aucun droit (`---`)||
|**0**|||Aucun droit (`---`)|

---

# 6️⃣ Activation du routage IPv4

Modifier :

```bash
sudo nano /etc/sysctl.conf
```

Décommenter :

```text
net.ipv4.ip_forward=1
```

Appliquer :

```bash
sudo sysctl -p
```

Vérifier :

```bash
cat /proc/sys/net/ipv4/ip_forward
```

Résultat attendu :

```text
1
```

![Activation du routage](https://github.com/FrancoisBarsotti-Oclock/WireGuard-HomeVPN/blob/main/images/Activation%20du%20routage.png)

> 📷 **Figure 5 – Activation du routage IPv4**

---

# 7️⃣ Démarrage du serveur

Lancer WireGuard :

```bash
sudo wg-quick up wg0
# Vérifier 
sudo wg
# Puis :
ip addr show wg0
```

![Interface wg0 active](https://github.com/FrancoisBarsotti-Oclock/WireGuard-HomeVPN/blob/main/images/Interface%20wg0%20active.png)

> 📷 **Figure 6 – Interface `wg0` active**

---

# 8️⃣ Activation au démarrage

```bash
sudo systemctl enable wg-quick@wg0
```

Contrôle :

```bash
systemctl is-enabled wg-quick@wg0
```

Résultat attendu :

```text
enabled
```

![Service wg-quick@wg0 activé](https://github.com/FrancoisBarsotti-Oclock/WireGuard-HomeVPN/blob/main/images/Service%20wg-quick%40wg0%20activ%C3%A9.png)

> 📷 **Figure 7 – Service `wg-quick@wg0` activé**

---

# ✅ Vérifications finales

Le serveur est correctement installé lorsque :

* WireGuard écoute sur le port UDP 51820.
* L'interface `wg0` est active.
* Le routage IPv4 est activé.
* Le fichier `wg0.conf` est protégé.
* Le service démarre automatiquement au démarrage du système.

Le serveur est maintenant prêt à recevoir les premiers clients VPN.
