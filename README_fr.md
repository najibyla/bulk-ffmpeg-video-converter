# Ultimate FFmpeg Script

Un script Bash de haute performance conçu pour la conversion massive et le redimensionnement de vidéos en parallèle. Ce script est optimisé pour les environnements **Windows (Git Bash)**, **macOS** et **Linux**.

## 🚀 Caractéristiques principales

- **Parallélisation intelligente** : Détecte automatiquement le nombre de cœurs CPU pour traiter plusieurs vidéos simultanément (testé jusqu'à 28 cœurs).
- **Accélération Matérielle** : Détection automatique des meilleurs encodeurs disponibles : **NVIDIA NVENC**, **Intel QuickSync (QSV)**, **Apple VideoToolbox** ou fallback sur **libx264**.
- **Calcul de Ratio Précis** : Maintient l'aspect ratio original avec un arrondi automatique à la largeur paire la plus proche (requis par H.264) via `awk`.
- **Robustesse Windows/Git Bash** : Nettoyage systématique des métadonnées pour éviter les erreurs de caractères invisibles (`\r`) propres à Windows .
- **Sécurité des Données** :
    - Utilisation de fichiers `.tmp` pour éviter la corruption en cas d'arrêt brutal .
    - Nettoyage automatique des fichiers temporaires via un `trap` sur les signaux système .
    - Gestion des noms de fichiers complexes (espaces, apostrophes, caractères spéciaux).
- **Optimisation Audio** : Encodage AAC 96k avec downmix stéréo automatique pour les sources multicanaux.

## 🛠 Prérequis

Le script nécessite les outils suivants installés et présents dans votre `PATH` :

- **FFmpeg** & **FFprobe**.
    
- **AWK** (pour les calculs de dimensions).
    
- **Bash** (v3.2 ou supérieur).
    

## 💻 Installation

1. Téléchargez le script `ffmpegFinal`.
    
2. Donnez-lui les droits d'exécution :
    
    Bash
    
    ```
    chmod +x ffmpegFinal
    ```
    
3. **Utilisateurs Windows** : Si vous rencontrez des erreurs de syntaxe liées aux fins de ligne, convertissez le fichier en format Unix :
    
    Bash
    
    ```
    dos2unix ffmpegFinal
    ```
    

## 📖 Utilisation

Le script scanne tous les sous-répertoires du dossier courant et place les vidéos converties dans un sous-dossier nommé `0` .

### Syntaxe

Bash

```
./ffmpegFinal [extension] [target_height]
```

- **`extension`** : L'extension de sortie souhaitée (mp4, mkv, ts, etc.). Défaut : `.m4v`.
    
- **`target_h`** : La hauteur cible en pixels. Défaut : `540`.
    

### Exemples

Bash

```
./ffmpegFinal               # Convertit en .m4v (540p) par défaut
./ffmpegFinal mp4 720       # Convertit en .mp4 (720p)
./ffmpegFinal mkv 1080      # Convertit en .mkv (1080p)
./ffmpegFinal --help        # Affiche l'aide [cite: 4]
```

## 📊 Logging & Résumé

À chaque exécution, un fichier de log horodaté (ex: `conversion_20260401_132201.log`) est généré. Il contient :

- L'encodeur et le nombre de cœurs utilisés .
    
- Le détail des succès (`[OK]`) et des échecs (`[ERREUR]`) .
    
- Un résumé comptable en fin de session .
    

## ⚙️ Détails Techniques (Encodeurs)

|**Encodeur**|**Paramètres de qualité**|
|---|---|
|**NVIDIA NVENC**|VBR, CQ 23, Preset P4|
|**Intel QSV**|Global Quality 23|
|**Apple VideoToolbox**|Quality 65|
|**CPU (libx264)**|CRF 22, Preset Medium|

---

**Note :** Ce script a été conçu pour être à la fois puissant et économe en interventions manuelles.
Pour toute suggestion ou rapport de bug, n'hésitez pas à ouvrir une issue.