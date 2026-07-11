# Building and Installing `lemonade-server-git`

This guide explains how to build and install the `lemonade-server-git` package from source using the Arch Linux Build System (`makepkg`).

## 📋 Prerequisites

Before building, ensure your system is up to date and you have the necessary base development tools installed.

1.  **Update your system:**
    ```bash
    sudo pacman -Syu
    ```

2.  **Install `base-devel` and other development and runtime dependencies:**
    The `base-devel` group contains essential tools like `make`, `gcc`, and `patch`.
    ```bash
    sudo pacman -S --needed base-devel cmake git openssl cpp-httplib zstd curl libwebsockets libdrm libcap systemd-libs libayatana-appindicator gtk3 libnotify
    ```

## 🚀 Building Instructions

Because this is a `-git` package, it will automatically pull the latest source code from the official GitHub repository.

### 1. Clone the Repository
First, clone the directory containing the `PKGBUILD` file to your local machine:
```bash
git clone https://github.com/mkhrussell/lemonade-server-git.git
cd lemonade-server-git
```

### 2. Build and Install
Use the `makepkg` command. The flags used are:
*   `-s`: Automatically install missing dependencies via `pacman`.
*   `-i`: Install the package once the build is successful.

```bash
makepkg -si
```

**What happens during this process?**
- `makepkg` downloads the latest source code from GitHub.
- It resolves and installs `makedepends` (like `cmake`, `ninja`, and `openssl`).
- It compiles the code using your system's optimized settings.
- It packages the files into a `.pkg.tar.zst` archive and installs it via `pacman`.

---

## ⚙️ Post-Installation & Configuration

### Configuration Files
After installation, the configuration files are located in:
*   **Main Config:** `/etc/lemonade/lemonade.conf`
*   **Secrets/Keys:** `/etc/lemonade/secrets.conf`
*   **Additional Configs:** `/etc/lemonade/conf.d/`

You can create or modify these files to set up your LLM serving environment.

### System User and Permissions
This package automatically creates a system user and temporary directory rules via:
*   `sysusers.d` (for user creation)
*   `tmpfiles.d` (for runtime directory management)

If you encounter permission issues, ensure your user is part of the necessary groups (e.g., `render` or `video` if using GPU acceleration).

### GPU/NPU Acceleration Issues
If the server starts but does not detect your GPU/NPU:
1. Ensure your drivers (Nvidia, AMD, or Intel NPU drivers) are correctly installed.
2. Check if your user has permission to access the hardware:
   ```bash
   groups
   ```
   (You may need to add yourself to the `render` group: `sudo usermod -aG render $USER`)

### Cleaning Up
If you want to start a clean build from scratch, run:
```bash
makepkg -Cci
```
*   `-C`: Cleans the source directory.
*   `-c`: Cleans up temporary build files after a successful build.
*   `-i`: Installs the package.
