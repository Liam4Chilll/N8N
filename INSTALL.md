# Installation n8n via Docker - VM Ubuntu Server 24.04

## Contexte
Installation de n8n sur VM Ubuntu Server via Docker avec accès depuis machine hôte Windows via VirtualBox NAT.

## Prérequis
- VM Ubuntu Server 24.04
- VirtualBox sur Windows
- Accès root/sudo sur la VM

## 1. Installation Docker

```bash
# Mise à jour système
sudo apt update

# Installation Docker + Docker Compose
sudo apt install -y docker.io docker-compose

# Activation service Docker
sudo systemctl start docker
sudo systemctl enable docker

# Ajout utilisateur au groupe docker (évite sudo)
sudo usermod -aG docker $USER
newgrp docker

# Vérification installation
docker --version
```

## 2. Configuration n8n Docker

### Structure de fichiers
```bash
# Création dossier projet
mkdir ~/n8n-docker && cd ~/n8n-docker
mkdir n8n-data
sudo chown -R 1000:1000 n8n-data
```

### Fichier docker-compose.yml
```yaml
version: '3.8'

services:
  n8n:
    image: n8nio/n8n:latest
    container_name: n8n
    restart: unless-stopped
    ports:
      - "0.0.0.0:5678:5678"  # Écoute toutes interfaces
    environment:
      - N8N_HOST=0.0.0.0
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
      - NODE_ENV=production
      - WEBHOOK_URL=http://localhost:5678/
      - GENERIC_TIMEZONE=Europe/Paris
    volumes:
      - ./n8n-data:/home/node/.n8n
```

## 3. Démarrage n8n

```bash
# Lancement conteneur en arrière-plan
docker-compose up -d

# Vérification statut
docker ps

# Vérification port ouvert
sudo netstat -tlnp | grep 5678
# Résultat attendu: 0.0.0.0:5678
```

## 4. Configuration réseau VirtualBox

### Port Forwarding (VM éteinte)
1. **VirtualBox Manager**
2. **Sélectionner VM → Paramètres**
3. **Réseau → Carte 1 → Avancé → Redirection de ports**
4. **Ajouter règle :**
   - Nom: `n8n`
   - Protocole: `TCP`
   - IP hôte: `127.0.0.1`
   - Port hôte: `5678`
   - IP invité: `(vide)`
   - Port invité: `5678`

### Firewall Ubuntu
```bash
# Autorisation port 5678
sudo ufw allow 5678
sudo ufw reload
```

## 5. Test d'accès

### Depuis VM Ubuntu
```bash
curl http://localhost:5678
```

### Depuis Windows
**Navigateur :** `http://localhost:5678` ou `http://127.0.0.1:5678`

## 6. Commandes de gestion

```bash
# Arrêt n8n
docker-compose down

# Redémarrage
docker-compose restart

# Logs temps réel
docker-compose logs -f n8n

# Mise à jour image
docker-compose pull && docker-compose up -d
```

## 7. Troubleshooting

### Vérifications
```bash
# Statut conteneur
docker ps

# Port d'écoute
sudo netstat -tlnp | grep 5678

# Logs erreurs
docker-compose logs n8n

# Test connectivité interne
curl -I http://localhost:5678
```

### Erreurs courantes
| Problème | Solution |
|----------|----------|
| Port non accessible | Vérifier port forwarding VirtualBox |
| Conteneur n'démarre pas | `docker-compose logs n8n` |
| Permissions n8n-data | `sudo chown -R 1000:1000 n8n-data` |
| Firewall bloque | `sudo ufw allow 5678` |

## 8. Sauvegarde (optionnel)

```bash
# Script backup données n8n
tar -czf n8n-backup-$(date +%Y%m%d).tar.gz n8n-data/
```

## Résultat
- **Interface n8n :** http://localhost:5678
- **Données persistantes :** ./n8n-data/
- **Logs :** `docker-compose logs n8n`
- **Auto-restart :** Activé via Docker

---
**Stack :** Ubuntu Server 24.04 • Docker • n8n • VirtualBox NAT
