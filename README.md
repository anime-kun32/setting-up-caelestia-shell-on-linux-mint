# Caelestia Shell Installation Guide (Ubuntu / Linux Mint)

A guide for installing **Caelestia Shell** on Ubuntu-based distributions such as Ubuntu and Linux Mint.

This guide focuses on a manual installation using:
- Qt 6.11.1
- Quickshell
- Caelestia Shell
- Hyprland

---

## 📋 Table of Contents

1. [Requirements](#requirements)
2. [Install Hyprland](#1-install-hyprland)
3. [Install Qt 6.11.1](#2-install-qt-6111)
4. [Install System Dependencies](#3-install-system-dependencies)
5. [Install Quickshell](#4-install-quickshell)
6. [Install Caelestia Dependencies](#5-install-caelestia-dependencies)
7. [cpptrace (Optional)](#6-cpptrace-optional)
8. [Install Caelestia Shell](#7-install-caelestia-shell)
9. [Running Caelestia](#8-running-caelestia)
10. [Updating Caelestia](#9-updating-caelestia)
11. [Configuration](#10-configuration)
12. [Troubleshooting](#troubleshooting)
13. [Credits](#credits)

---

# Requirements

Before starting, make sure you have:

- A brain 🧠
- A fucking computer 💻
- Fingers
- Hands
- A keyboard
- Patience

Software requirements:

- Linux Mint / Ubuntu based system
- Hyprland
- Qt 6.11.1
- Quickshell
- CMake
- Ninja
- Git

---

# 1. Install Hyprland

Caelestia Shell runs on top of Hyprland.

If you do not already have Hyprland installed, follow:

https://github.com/LinuxBeginnings/Ubuntu-Hyprland

Make sure Hyprland launches correctly before continuing.

Caelestia is only the desktop shell. It is not a window manager.

---

# 2. Install Qt 6.11.1

## Create a Qt account

You need a Qt account to download the official installer.

Go to:

https://www.qt.io/download

Download the Linux installer.

Run:

```bash
chmod +x qt-online-installer-linux-x64-*.run
./qt-online-installer-linux-x64-*.run
```

Sign in with your Qt account and continue.

---

## Qt Installer Setup

**Step 1: Select Installation Type**
When asked for the installation type, select:

```
Custom installation
```

![Qt Installer Step 4 - Custom Installation](guide-4.png)

*Screenshot: Selecting "Custom installation" during Qt setup.*

---

**Step 2: Select Qt for Development**
Expand:

```
Qt for development
```

![Qt Installer Step 5 - Qt for Development](guide-5.png)

*Screenshot: Expanding "Qt for development" section.*

Then expand:

```
Qt
```

---

**Step 3: Choose Qt Version**
Expand:

```
Qt 6.11.1
```

Inside Qt 6.11.1 select:

```
Desktop
```

Only select the desktop component. Do not select unnecessary platforms.

![Qt Installer Step 6 - Select Desktop](guide-6.png)

*Screenshot: Selecting "Desktop" under Qt 6.11.1.*

---

**Step 4: Add Qt Wayland**
Expand:

```
Additional libraries
```

under Qt 6.11.1.

![Qt Installer Step 7 - Additional Libraries](guide-7.png)

*Screenshot: Expanding "Additional libraries" section.*

Scroll down until you find:

```
Qt Wayland
```

Select:

```
Qt Wayland
```

This is required for Wayland applications.

![Qt Installer Step 8 - Qt Wayland](guide-8.png)

*Screenshot: Selecting "Qt Wayland" from Additional libraries.*

---

**Step 5: Skip Build Tools**
Do not install:

```
Build Tools
```

They are not required. We will use GCC, CMake, and Ninja from the system.

---

**Step 6: Accept License**
Accept the Qt license agreement.

![Qt Installer Step 9 - License Agreement](guide-9.png)

*Screenshot: Accepting the Qt license agreement.*

---

**Step 7: Verify Selection**
Before installing, make sure your selected components look like this:

- Qt 6.11.1 → Desktop
- Qt 6.11.1 → Additional Libraries → Qt Wayland

![Qt Installer Step 10 - Final Selection](guide-10.png)

*Screenshot: Final component selection before installation.*

Continue the installation.

---

# 3. Install System Dependencies

Install required tools:

```bash
sudo apt update
sudo apt install git cmake ninja-build build-essential
```

---

# 4. Install Quickshell

## Recommended method

Quickshell is available from the DankLinux repository.

Add the repository:

```bash
sudo add-apt-repository ppa:avengemedia/danklinux
sudo apt update
```

Install the stable version:

```bash
sudo apt install quickshell
```

or install the latest git version:

```bash
sudo apt install quickshell-git
```

---

# 5. Install Caelestia Dependencies

Install required libraries:

```bash
sudo apt install \
ddcutil \
brightnessctl \
network-manager \
lm-sensors \
fish \
libpipewire-0.3-dev \
libqalculate-dev \
swappy
```

| Package | Feature |
|---------|---------|
| ddcutil | Monitor brightness control |
| brightnessctl | Laptop brightness |
| network-manager | Network widget |
| lm-sensors | Hardware monitoring |
| fish | Shell utilities |
| PipeWire | Audio visualiser |
| libqalculate | Calculator |
| swappy | Screenshot editing |

---

# 6. cpptrace (Optional)

Caelestia can use cpptrace for debugging. Most users can skip this. Only install it if CMake complains about missing cpptrace.

Install cpptrace:

```bash
git clone https://github.com/jeremy-rifkin/cpptrace.git
cd cpptrace
cmake -B build -G Ninja -DCMAKE_BUILD_TYPE=Release
cmake --build build
sudo cmake --install build
cd ..
```

---

# 7. Install Caelestia Shell

Create the Quickshell directory:

```bash
mkdir -p ~/.config/quickshell
```

Clone Caelestia:

```bash
cd ~/.config/quickshell
git clone https://github.com/caelestia-dots/shell.git caelestia
```

Enter:

```bash
cd caelestia
```

Configure:

```bash
cmake -B build \
-G Ninja \
-DCMAKE_BUILD_TYPE=Release \
-DCMAKE_INSTALL_PREFIX=/
```

Build:

```bash
cmake --build build
```

Install:

```bash
sudo cmake --install build
```

---

# 8. Running Caelestia

Start Caelestia:

```bash
caelestia shell -d
```

or:

```bash
qs -c caelestia
```

---

# 9. Updating Caelestia

If Caelestia releases a new version:

Go to your Caelestia folder:

```bash
cd ~/.config/quickshell/caelestia
```

Check your branch:

```bash
git status
```

If you see:

```
HEAD detached at vX.X.X
```

you are on a release tag.

Switch back to main:

```bash
git checkout main
```

Update:

```bash
git pull
```

Rebuild:

```bash
cmake --build build
```

Install again:

```bash
sudo cmake --install build
```

---

# 10. Configuration

Caelestia configuration:

```
~/.config/caelestia/
```

Main file:

```
shell.json
```

Example:

```
~/.config/caelestia/shell.json
```

---

# Troubleshooting

## Qt not detected

Check:

```bash
echo $CMAKE_PREFIX_PATH
```

It should point to:

```
~/Qt/6.11.1/gcc_64
```

Example:

```bash
export CMAKE_PREFIX_PATH=$HOME/Qt/6.11.1/gcc_64
```

---

## Missing QML modules

Check:

```bash
echo $QML_IMPORT_PATH
```

Example:

```
/usr/lib/qt6/qml
```

---

## Caelestia does not start

Check:

```bash
qs -c caelestia
```

Look for missing QML modules or libraries.

---

# Credits

Caelestia Shell:
https://github.com/caelestia-dots/shell

Quickshell:
https://quickshell.outfoxxed.me

Hyprland:
https://hyprland.org

Ubuntu Hyprland Guide:
https://github.com/LinuxBeginnings/Ubuntu-Hyprland
