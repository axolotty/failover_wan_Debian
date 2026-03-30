# 🔀 Failover WAN Debian

![Bash](https://img.shields.io/badge/Bash-5%2B-informational?style=for-the-badge)
![Debian](https://img.shields.io/badge/Debian-11%2F12-red?style=for-the-badge)
![Licence](https://img.shields.io/badge/Licence-MIT-green?style=for-the-badge)

Script de bascule automatique WAN1 → WAN2 → LAN pour serveur Debian. Surveille en continu la connectivité et bascule automatiquement vers l'interface de secours en cas de panne.

---

## Fonctionnement

- Surveillance continue de la connectivité WAN1 (ping vers gateway ou IP externe)
- Bascule automatique vers WAN2 en cas d'échec détecté
- Retour automatique sur WAN1 dès que la connectivité est rétablie
- Notifications SMS via Free Mobile (optionnel)
- Journalisation de tous les événements de bascule

---

## Prérequis

- Debian 11 ou 12
- Droits `sudo` / root
- Deux interfaces réseau WAN configurées
- `iproute2`, `iputils-ping` installés

---

## Installation

```bash
git clone https://github.com/Axolotty/failover_wan_Debian.git
cd failover_wan_Debian
cp config.env.example config.env
nano config.env
chmod +x monitor.sh
```

---

## Configuration

Éditer le fichier `config.env` :

```bash
# Interfaces réseau
WAN1_IFACE=eth0
WAN2_IFACE=eth1

# IP à pinger pour tester la connectivité
PING_TARGET=8.8.8.8
PING_COUNT=3

# Délai entre chaque vérification (secondes)
CHECK_INTERVAL=30

# Notifications Free Mobile (optionnel)
FREE_MOBILE_USER=
FREE_MOBILE_CODE_AUTH=
```

---

## Utilisation

### Lancement manuel

```bash
sudo ./monitor.sh
```

### Suivi de l'utilisation données

```bash
./data_usage.sh
```

---

## Mise en service automatique (systemd)

Copier le fichier de service systemd et activer :

```bash
sudo cp failover.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable failover.service
sudo systemctl start failover.service
```

Vérifier le statut :

```bash
sudo systemctl status failover.service
journalctl -u failover.service -f
```

---

## Fichiers du projet

| Fichier | Description |
|---------|-------------|
| `monitor.sh` | Script principal de surveillance et bascule |
| `data_usage.sh` | Suivi de la consommation de données par interface |
| `notify_free_sms.sh` | Envoi de notifications SMS via Free Mobile |
| `config.env` | Configuration (interfaces, cibles ping, credentials) |
| `state.json` | État courant du failover (WAN actif, timestamp) |
| `status.txt` | Dernier statut lisible |
| `logs/` | Journaux des événements de bascule |
| `web/` | Interface web de consultation du statut |

---

## Logs

Les logs sont stockés dans le dossier `logs/`. Chaque bascule est enregistrée avec horodatage, interface active avant/après et raison du changement.

---

## Licence

MIT © Axolotty
