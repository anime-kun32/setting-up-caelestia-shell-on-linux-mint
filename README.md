# Caelestia Shell Installation Guide (Ubuntu / Linux Mint)

A guide for installing **Caelestia Shell** on Ubuntu-based distributions such as Ubuntu and Linux Mint.

This guide focuses on a manual installation using:
- Qt 6.11.1
- Quickshell
- Caelestia Shell
- Hyprland

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

When asked for the installation type select:

```
Custom installation
```

![Qt Installer Step 4](guide-4.png)

---

Expand:

```
Qt for development
```

![Qt Installer Step 5](guide-5.png)

Then expand:

```
Qt
```

---

Expand:

```
Qt 6.11.1
```

Inside Qt 6.11.1 select:

```
Desktop
```

Only select the desktop component.

Do not select unnecessary platforms.

![Qt Installer Step 6](guide-6.png)

---

Expand:

```
Additional libraries
```

under Qt 6.11.1.

![Qt Installer Step 7](guide-7.png)

---

Scroll down until you find:

```
Qt Wayland
```

Select:

```
Qt Wayland
```

This is required for Wayland applications.

![Qt Installer Step 8](guide-8.png)

---

Do not install:

```
Build Tools
```

They are not required.

We will use:

- GCC
- CMake
- Ninja

from the system.

---

Accept the Qt license agreement.

![Qt Installer Step 9](guide-9.png)

---

Before installing, make sure your selected components look like this:

![Qt Installer Step 10](guide-10.png)

Continue the installation.

---

# 3. Install System Dependencies

Install required tools:

```bash
sudo apt update

sudo apt install \
git \
cmake \
ninja-build \
build-essential \
qt6-base-dev \
qt6-declarative-dev \
qt6-wayland \
libwayland-dev
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
libcava-dev \
network-manager \
lm-sensors \
fish \
aubio-tools \
libpipewire-0.3-dev \
libqalculate-dev \
swappy
```

---

# 6. cpptrace

Caelestia can use cpptrace for debugging.

Install cpptrace:

```bash
git clone https://github.com/jeremy-rifkin/cpptrace.git

cd cpptrace

cmake -B build -G Ninja -DCMAKE_BUILD_TYPE=Release

cmake --build build

sudo cmake --install build
```

Return:

```bash
cd ..
```

---

## Skipping cpptrace

If you do not want cpptrace, you can try building without it.

Clone Caelestia:

```bash
git clone https://github.com/caelestia-dots/shell.git

cd shell
```

Configure:

```bash
cmake -B build \
-G Ninja \
-DCMAKE_BUILD_TYPE=Release
```

Build:

```bash
cmake --build build
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
