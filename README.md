# Caelestia Shell on Ubuntu / Linux Mint

A guide for installing **Caelestia Shell** on Ubuntu-based distributions (Ubuntu, Linux Mint, etc).

This guide installs:

- Hyprland
- Qt 6.11.1
- Quickshell
- Caelestia Shell

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

- Ubuntu / Linux Mint based system
- Hyprland
- Qt 6.11.1
- Quickshell
- CMake
- Ninja
- Git

---

# 1. Install Hyprland

Caelestia Shell is designed to run on top of Hyprland.

If you do not already have Hyprland installed, follow:

https://github.com/LinuxBeginnings/Ubuntu-Hyprland

Make sure Hyprland starts correctly before continuing.

Caelestia is only a desktop shell.
It is not a window manager.

---

# 2. Install Qt 6.11.1

## Create a Qt account

You need a Qt account to download the official installer.

Go to:

https://www.qt.io/development/download-qt-installer-oss

Download the Linux installer.

Run:

```bash
chmod +x qt-online-installer-linux-x64-*.run

./qt-online-installer-linux-x64-*.run
```

Sign into your Qt account and continue.

---

## Qt Installer Setup

Select:

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

Select only:

```
Desktop
```

Do not select extra platforms.

![Qt Installer Step 6](guide-6.png)

---

Expand:

```
Additional libraries
```

under Qt 6.11.1.

![Qt Installer Step 7](guide-7.png)

---

Scroll down and find:

```
Qt Wayland
```

Enable:

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

Accept the license agreement.

![Qt Installer Step 9](guide-9.png)

---

Before installing, check that your selection looks like this:

![Qt Installer Step 10](guide-10.png)

Continue with installation.

---

# 3. Install Build Tools

Install the required build packages:

```bash
sudo apt update

sudo apt install \
git \
cmake \
ninja-build \
build-essential
```

---

# 4. Install Quickshell

Quickshell is required by Caelestia.

The easiest method on Ubuntu/Linux Mint is using the DankLinux packages.

Add the repository:

```bash
sudo add-apt-repository ppa:avengemedia/danklinux

sudo apt update
```

Install the stable release:

```bash
sudo apt install quickshell
```

or install the git version:

```bash
sudo apt install quickshell-git
```

---

# 5. Install Caelestia Runtime Dependencies

Install optional features used by Caelestia:

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

These provide:

| Package | Feature |
|-|-|
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

Caelestia can use cpptrace for debugging.

Most users can skip this.

Only install it if CMake complains about missing cpptrace.

Build cpptrace:

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

# 7. Install Caelestia Shell

Create the Quickshell folder:

```bash
mkdir -p ~/.config/quickshell
```

Clone Caelestia:

```bash
cd ~/.config/quickshell

git clone https://github.com/caelestia-dots/shell.git caelestia
```

Enter the directory:

```bash
cd caelestia
```

---

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

# 8. Launch Caelestia

Start the shell:

```bash
caelestia shell -d
```

or:

```bash
qs -c caelestia
```

---

# 9. Updating Caelestia

When a new Caelestia release comes out:

Go to your installation:

```bash
cd ~/.config/quickshell/caelestia
```

Check your git state:

```bash
git status
```

If you see:

```
HEAD detached at v2.x.x
```

you are currently on a release tag.

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

Install:

```bash
sudo cmake --install build
```

---

# 10. Configuration

Caelestia configuration is stored in:

```
~/.config/caelestia/
```

Main configuration file:

```
shell.json
```

Example:

```
~/.config/caelestia/shell.json
```

---

# Troubleshooting

## Qt not found

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

Try:

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
