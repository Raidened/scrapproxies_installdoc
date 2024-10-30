### 🚀 Guide d'Installation des Proxies IPv6 =>

### 📋 Prérequis :

- VPS Ubuntu sur OVH
- Accès root
- Block IPv6 configuré sur OVH

### 🛠 Installation de HAProxy :

```bash

# Mise à jour du système
sudo apt update && sudo apt upgrade -y

# Installation de HAProxy
sudo apt install haproxy -y

```

### ⚙️ Configuration de HAProxy :

```bash
# Création/édition du fichier de configuration
sudo nano /etc/haproxy/haproxy.cfg
```

```cfg
global
    log /dev/log local0
    maxconn 32768
    ulimit-n 65536
    user haproxy
    group haproxy
    daemon

defaults
    mode tcp
    log global
    option tcplog
    timeout connect 5s
    timeout client 30s
    timeout server 30s
    
frontend proxy_ipv6
    bind :::3128-3148 v4v6
    default_backend real_server

backend real_server
    server local 127.0.0.1:80
```

### 🔄 Démarrage du Service :

```bash
# Démarrer HAProxy
sudo systemctl start haproxy

# Activer au démarrage
sudo systemctl enable haproxy

# Vérifier le statut
sudo systemctl status haproxy
```

### 📝 Création du Fichier CSV pour le Code :

Créer un fichier proxies.csv avec vos adresses IPv6 :
```csv
2a02:c204:2:5012::2,3128
2a02:c204:2:5012::3,3129
[...]
```

### 🔍 Surveillance

```bash
# Consulter les logs
sudo tail -f /var/log/haproxy.log

# Vérifier les performances
sudo haproxy -d
```
### ⚡ Points Forts

- Performance optimisée pour le scraping
- Gestion efficace des connexions simultanées
- Compatible avec votre code existant (ProxyManager)
- Monitoring avancé

### 🚨 Dépannage

En cas d'erreur, vérifier :

- Les logs : /var/log/haproxy.log
- Le status du service : systemctl status haproxy
- La configuration : haproxy -c -f /etc/haproxy/haproxy.cfg

### 📊 Caractéristiques du VLE-4 :

- 4 Go de RAM (suffisant pour votre code avec maxConcurrentRequests = 1)
- 2 vCores (adapté au traitement séquentiel actuel)
- 80 Go SSD (amplement suffisant pour le système et logs)
- Bande passante partagée (suffisante vu vos délais de 1000ms entre requêtes)

### 🔑 Points clés pour votre utilisation :

- Support IPv6 natif (compatible avec vos proxies listés dans le CSV)
- Infrastructure réseau OVH stable
- Prix optimisé pour votre cas d'usage