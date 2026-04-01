A high-performance Bash script designed for bulk video conversion and resizing in parallel. This script is optimized for **Windows (Git Bash)**, **macOS**, and **Linux** environments.

## 🚀 Main Features

- **Intelligent Parallelization**: Automatically detects the number of CPU cores to process multiple videos simultaneously (tested on configurations with up to 28 cores) .
    
- **Hardware Acceleration**: Automatically detects and utilizes the best available hardware encoders: **NVIDIA NVENC**, **Intel QuickSync (QSV)**, **Apple VideoToolbox**, or falls back to **libx264**.
    
- **Precise Ratio Calculation**: Maintains the original aspect ratio by automatically rounding to the nearest even width (required by H.264) using `awk`.
    
    - Formula: $target\_w = \text{round}\left(\frac{src\_w \times target\_h}{src\_h \times 2}\right) \times 2$.
        
- **Windows/Git Bash Robustness**: Systematically cleans metadata to prevent "integer expression expected" errors caused by invisible carriage return characters (`\r`) often returned by FFprobe on Windows .
    
- **Data Safety**:
    
    - Uses `.tmp` files during processing to prevent corrupted files from being marked as "complete" if the process is interrupted .
        
    - Automatic cleanup of temporary files via a `trap` on system signals (EXIT, INT, TERM) .
        
    - Handles complex filenames containing spaces, apostrophes, and special characters.
        
- **Audio Optimization**: AAC 96k encoding with automatic stereo downmixing for multichannel sources.
    

## 🛠 Prerequisites

The script requires the following tools to be installed and available in your `PATH`:

- **FFmpeg** & **FFprobe**.
    
- **AWK** (for dimension calculations).
    
- **Bash** (v3.2 or higher).
    

---

## 💻 Installation & Setup (Windows)

To run this script on Windows, you need a Bash environment and FFmpeg correctly configured.

### 1. Install Git Bash

Git Bash provides the Unix-like environment necessary to run `.sh` scripts.

- **Download**: Visit [git-scm.com](https://git-scm.com/download/win).
    
- **Install**: Run the `.exe`.
    
- **Crucial Step**: During installation, select **"Checkout Windows-style, commit Unix-style line endings"** to avoid script syntax errors.
    

### 2. Install Chocolatey (Package Manager)

Chocolatey allows you to install FFmpeg easily and manages your system PATH automatically.

- Open **PowerShell** as an **Administrator**.
    
- Run the following command:
    
    PowerShell
    
    ```
    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
    ```
    
- Restart your terminal.
    

### 3. Install FFmpeg

Once Chocolatey is installed, run this command in an Admin PowerShell:

PowerShell

```
choco install ffmpeg
```

---

## 📖 Usage

The script scans all subdirectories of the current folder and places converted videos into a subfolder named `0` inside each parent directory.

### Syntax

Bash

```
./ffmpegFinal [extension] [target_height]
```

- **`extension`**: The desired output extension (mp4, mkv, ts, etc.). Default is `.m4v`.
    
- **`target_height`**: The target height in pixels. Default is `540`.
    

### Examples

Bash

```
./ffmpegFinal               # Converts to .m4v (540p) by default
./ffmpegFinal mp4 720       # Converts to .mp4 (720p)
./ffmpegFinal --help        # Shows detailed help
```

## 📊 Logging & Summary

A timestamped log file is generated for each run (e.g., `conversion_20260401_132201.log`). It contains:

- The detected encoder and the number of cores used .
    
- Timestamped details of successes (`[OK]`) and failures (`[ERREUR]`) .
    
- A final summary (total count of successes vs. errors) .
    

## ⚙️ Technical Details (Encoders)

| **Encoder**            | **Quality Settings**  |
| ---------------------- | --------------------- |
| **NVIDIA NVENC**       | VBR, CQ 23, Preset P4 |
| **Intel QSV**          | Global Quality 23     |
| **Apple VideoToolbox** | Quality 65            |
| **CPU (libx264)**      | CRF 22, Preset Medium |


Note: This script was designed to be both powerful and low-maintenance.
For any suggestions or bug reports, please feel free to open an issue.