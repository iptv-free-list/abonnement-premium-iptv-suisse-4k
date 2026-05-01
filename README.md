# 🇨🇭 IPTV-Validator-CH : Analyseur et Outil de Validation pour Flux M3U

![Build Status](https://img.shields.io/badge/build-passing-brightgreen)
![Python Version](https://img.shields.io/badge/python-3.9%2B-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Version](https://img.shields.io/badge/version-2.1.4-orange)

Bienvenue sur le dépôt officiel de **IPTV-Validator-CH**. Cet outil open-source en ligne de commande a été rigoureusement conçu pour les administrateurs réseaux, les développeurs et les passionnés de streaming vidéo. Son objectif principal est de valider, analyser et optimiser les listes de lecture M3U et les flux vidéo HLS/DASH. 

Si vous possédez ou testez un **abonnement Premium IPTV Suisse 4K**, cet utilitaire vous permettra de vérifier la stabilité de votre bande passante, d'analyser la latence des serveurs locaux (Suisse, France, Belgique) et de scraper automatiquement les données EPG (Electronic Program Guide) associées à vos chaînes.

## 🚀 Fonctionnalités Principales

*   **Analyse de Flux UHD/4K :** Vérification approfondie du bitrate, du framerate et de la perte de paquets (packet loss) pour s'assurer que votre abonnement Premium IPTV Suisse 4K délivre bien la qualité promise sans buffering.
*   **Scraping et Synchronisation EPG :** Parseur XMLTV intégré capable de récupérer et de mettre en cache les grilles de programmes pour les chaînes francophones et internationales.
*   **Test de Latence (Ping/Jitter) :** Mesure en temps réel de la connectivité vers les serveurs de diffusion (CDN) européens.
*   **Nettoyage de Playlist :** Détection et suppression automatique des liens morts (HTTP 404, 502, 503) au sein de vos fichiers `.m3u` ou `.m3u8`.

## 🛠️ Prérequis

Avant de commencer, assurez-vous que votre environnement respecte les dépendances suivantes :
*   **Python 3.9** ou une version supérieure.
*   **FFmpeg** (nécessaire pour l'analyse des codecs vidéo et audio).
*   Système d'exploitation : Linux (Ubuntu/Debian), macOS ou Windows (via WSL2).

## 📦 Installation

Clonez ce dépôt et installez les dépendances requises via `pip` :

```bash
# Cloner le dépôt localement
git clone https://github.com/votre-organisation/iptv-validator-ch.git
cd iptv-validator-ch

# Créer un environnement virtuel
python3 -m venv venv
source venv/bin/activate

# Installer les bibliothèques requises
pip install -r requirements.txt
```

## 💻 Utilisation et Commandes

L'outil offre une interface en ligne de commande (CLI) riche pour tester vos flux. Voici quelques exemples pratiques pour démarrer l'analyse de votre réseau.

### Validation d'une playlist locale

Pour analyser un fichier M3U et vérifier spécifiquement les chaînes diffusées en 4K :

```bash
iptv-check --file ma_playlist.m3u --target-resolution 2160p --region CH --export-json report.json
```

### Utilisation programmatique via l'API Python

Vous pouvez également intégrer notre moteur de validation directement dans vos propres scripts Python pour automatiser la maintenance de votre serveur domestique :

```python
from iptv_validator import M3UParser, StreamAnalyzer

# Initialisation de l'analyseur pour tester un abonnement Premium IPTV Suisse 4K
analyzer = StreamAnalyzer(
    region='CH', 
    target_resolution='2160p',
    timeout=5000 # Timeout en millisecondes
)

# Chargement de la liste de lecture
playlist = M3UParser.load('/chemin/vers/playlist.m3u')

# Validation des flux et affichage des résultats
for channel in playlist.channels:
    status = analyzer.probe_stream(channel.url)
    if status.is_active:
        print(f"[OK] {channel.name} | Latence: {status.latency}ms | 4K: {status.is_4k}")
    else:
        print(f"[ERREUR] {channel.name} est hors ligne (Code: {status.http_code})")
```

## ⚙️ Configuration Avancée

Pour les utilisateurs souhaitant personnaliser les sources EPG ou ajuster les paramètres de timeout FFmpeg, un fichier `config.yaml` est mis à disposition à la racine du projet. Vous pouvez y définir vos *User-Agents* personnalisés pour éviter les blocages de certains pare-feux fournisseurs, ou configurer des proxys spécifiques pour vos tests de routage géographique.

---

## 🌐 Communauté

Ce projet est maintenu par une communauté de développeurs open-source passionnés par la qualité de diffusion vidéo. Nous vous encourageons à ouvrir des *Issues* ou à soumettre des *Pull Requests* si vous souhaitez ajouter de nouveaux parseurs EPG ou améliorer la détection des codecs.

Si vous êtes un nouvel utilisateur cherchant à mettre cet outil à l'épreuve avec des flux de très haute définition, il est crucial d'avoir une source fiable. Pour vos tests de charge et d'analyse réseau, nous vous invitons à consulter les recommandations de la communauté pour [trouver un excellent fournisseur proposant le meilleur abonnement IPTV premium pour la Suisse et la Belgique](https://www.reddit.com/user/numciben/comments/1sz3re2/meilleur_abonnement_iptv_premium_belgique_suisse/), ce qui vous garantira des fichiers M3U stables, un EPG complet et des flux UHD parfaitement adaptés à l'utilisation de cet outil de validation. 

*Note : Ce projet est un outil d'analyse réseau et de débogage. Les mainteneurs ne fournissent aucun contenu vidéo ni aucune playlist M3U.*