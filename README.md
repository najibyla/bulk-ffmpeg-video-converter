A high-performance, **highly configurable** Bash script designed for bulk video conversion and resizing in parallel. Optimized for **Windows (Git Bash)**, **macOS**, and **Linux**.

## 🚀 New in v2: Modular Configuration

The script now features a dedicated **Configuration Section** at the top of the file. You can easily customize:

- **Default values**: Extension, target height, and output subdirectory.
    
- **Input Formats**: Add or remove source extensions (mp4, mkv, avi, etc.) via a dynamic array.
    
- **Encoder Presets**: Fine-tune quality settings for **NVENC**, **QSV**, **VideoToolbox**, and **libx264** without touching the core logic.
    
- **Muxer Options**: Specific flags for MP4 (faststart), MKV, and TS containers.
    

## ✨ Key Features

- **Intelligent Parallelization**: Automatically detects CPU cores and processes multiple videos simultaneously (tested up to 28 cores).
    
- **Auto-Detection of Hardware Acceleration**:
    
    - **NVIDIA NVENC** (HEVC/H.264)
        
    - **Intel QuickSync (QSV)**
        
    - **Apple VideoToolbox**
        
    - **libx264** (Software fallback)
        
- **Precise Aspect Ratio**: Uses `awk` to calculate dimensions, ensuring the width is always an **even number** (required for H.264 compatibility).
    
    - _Formula_: $target\_w = \text{round}\left(\frac{src\_w \times target\_h}{src\_h \times 2}\right) \times 2$
        
- **Bulletproof Filename Handling**: Uses `printf %q` and `find -print0` to safely process files with spaces, apostrophes, and special characters.
    
- **Data Safety**:
    
    - Converts to `.tmp` files first to prevent half-finished files in case of interruption.
        
    - **Trap System**: Automatically deletes partial `.tmp` files if the script is stopped (Ctrl+C).
        
- **Audio Optimization**: AAC encoding with a configurable bitrate and automatic downmixing to stereo for multichannel sources.
    

## 🛠 Prerequisites

Ensure these are in your `PATH`:

- **FFmpeg** & **FFprobe**
    
- **AWK** (Standard on most Unix systems)
    
- **Bash** (v3.2 or higher)
    

---

## 💻 Installation & Setup (Windows)

### 1. Install Git Bash

Required to run `.sh` scripts on Windows.

- **Download**: [git-scm.com](https://git-scm.com/download/win).
    
- **Crucial**: During install, select **"Checkout Windows-style, commit Unix-style line endings"**.
    

### 2. Install FFmpeg (via Chocolatey)

The easiest way to manage FFmpeg on Windows.

- Open **PowerShell (Admin)** and run:
    
    PowerShell
    
    ```
    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
    ```
    
- Then install FFmpeg:
    
    PowerShell
    
    ```
    choco install ffmpeg
    ```
    

---

## 📖 Usage

The script scans subdirectories and places results in a folder named `0` (configurable).

### Syntax

Bash

```
./ffmpegFinal [extension] [target_height]
```

### Examples

Bash

```
./ffmpegFinal               # Default: .m4v at 540p
./ffmpegFinal mp4 720       # Target: .mp4 at 720p
./ffmpegFinal --help        # View options and defaults
```

## 📊 Technical Presets (v6.4 Defaults)

|**Encoder**|**Default Settings**|
|---|---|
|**NVIDIA NVENC**|VBR, CQ 23, Preset P4|
|**Intel QSV**|Global Quality 23|
|**Apple VideoToolbox**|Quality 65|
|**Software (x264)**|CRF 22, Preset Medium|

## 📝 Logging

A log file (e.g., `conversion_20260407_150000.log`) is created for every session, providing a detailed breakdown of:

- Encoder used & CPU Core count.
    
- Per-file status: `[OK]` or `[ERREUR]`.
    
- Final summary of processed files.
    

---

**Note**: This script is designed for high-performance production environments. For bug reports or feature requests, please open an issue on the repository.