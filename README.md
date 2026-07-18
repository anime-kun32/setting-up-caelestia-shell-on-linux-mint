# Caelestia Shell on Ubuntu / Linux Mint

A guide for installing Caelestia Shell on Ubuntu-based distributions (Ubuntu, Linux Mint, etc).

This guide installs:

- Hyprland
- Qt 6.11.1
- Quickshell
- Caelestia Shell


==================================================
REQUIREMENTS
==================================================

Before starting, make sure you have:

- A brain 🧠
- A computer 💻
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


==================================================
1. INSTALL HYPRLAND
==================================================

Caelestia Shell is designed to run on top of Hyprland.

If you do not already have Hyprland installed, follow:

https://github.com/LinuxBeginnings/Ubuntu-Hyprland

Make sure Hyprland starts correctly before continuing.

Caelestia is only a desktop shell.
It is not a window manager.


==================================================
2. INSTALL QT 6.11.1
==================================================

CREATE A QT ACCOUNT

You need a Qt account to download the official installer.

Go to:

https://www.qt.io/download

Download the Linux installer.

Run:

chmod +x qt-online-installer-linux-x64-*.run

./qt-online-installer-linux-x64-*.run

Sign into your Qt account and continue.


QT INSTALLER SETUP

Select:

Custom installation

[Image: guide-4.png]


Expand:

Qt for development

[Image: guide-5.png]


Then expand:

Qt


Expand:

Qt 6.11.1

Select only:

Desktop

Do not select extra platforms.

[Image: guide-6.png]


Expand:

Additional libraries

under Qt 6.11.1.

[Image: guide-7.png]


Scroll down and find:

Qt Wayland

Enable:

Qt Wayland

This is required for Wayland applications.

[Image: guide-8.png]


Do NOT install:

Build Tools

They are not required.

We will use:

- GCC
- CMake
- Ninja

from the system.


Accept the license agreement.

[Image: guide-9.png]


Before installing, check your selection matches:

[Image: guide-10.png]


Continue with installation.


==================================================
3. INSTALL BUILD TOOLS
==================================================

Install required packages:

sudo apt update

sudo apt install \
git \
cmake \
ninja-build \
build-essential


==================================================
4. INSTALL QUICKSHELL
==================================================

Quickshell is required by Caelestia.

The easiest method on Ubuntu/Linux Mint is using the DankLinux packages.

Add the repository:

sudo add-apt-repository ppa:avengemedia/danklinux

sudo apt update


Install stable release:

sudo apt install quickshell


OR install git version:

sudo apt install quickshell-git


==================================================
5. CAELESTIA DEPENDENCIES
==================================================

Install optional features used by Caelestia:

sudo apt install \
ddcutil \
brightnessctl \
network-manager \
lm-sensors \
fish \
libpipewire-0.3-dev \
libqalculate-dev \
swappy


Package purposes:

ddcutil
- Monitor brightness control

brightnessctl
- Laptop brightness control

network-manager
- Network widget

lm-sensors
- Hardware monitoring

fish
- Shell utilities

libpipewire-0.3-dev
- Audio features

libqalculate-dev
- Calculator features

swappy
- Screenshot editing


==================================================
6. CPPTRACE (OPTIONAL)
==================================================

Most users can skip cpptrace.

Only install it if CMake complains about missing cpptrace.

Build cpptrace:

git clone https://github.com/jeremy-rifkin/cpptrace.git

cd cpptrace

cmake -B build -G Ninja -DCMAKE_BUILD_TYPE=Release

cmake --build build

sudo cmake --install build


Return:

cd ..


==================================================
7. INSTALL CAELESTIA SHELL
==================================================

Create the Quickshell folder:

mkdir -p ~/.config/quickshell


Clone Caelestia:

cd ~/.config/quickshell

git clone https://github.com/caelestia-dots/shell.git caelestia


Enter directory:

cd caelestia


Configure:

cmake -B build \
-G Ninja \
-DCMAKE_BUILD_TYPE=Release \
-DCMAKE_INSTALL_PREFIX=/


Build:

cmake --build build


Install:

sudo cmake --install build


==================================================
8. START CAELESTIA
==================================================

Start shell:

caelestia shell -d


OR:

qs -c caelestia


==================================================
9. UPDATING CAELESTIA
==================================================

Go to installation:

cd ~/.config/quickshell/caelestia


Check git:

git status


If you see:

HEAD detached at v2.x.x

switch back:

git checkout main


Update:

git pull


Rebuild:

cmake --build build


Install:

sudo cmake --install build


==================================================
10. CONFIGURATION
==================================================

Caelestia configuration:

~/.config/caelestia/


Main configuration:

shell.json


Example:

~/.config/caelestia/shell.json


==================================================
TROUBLESHOOTING
==================================================

QT NOT FOUND

Check:

echo $CMAKE_PREFIX_PATH


It should point to:

~/Qt/6.11.1/gcc_64


Example:

export CMAKE_PREFIX_PATH=$HOME/Qt/6.11.1/gcc_64



MISSING QML MODULES

Check:

echo $QML_IMPORT_PATH


Example:

/usr/lib/qt6/qml



CAELESTIA DOES NOT START

Try:

qs -c caelestia


Look for missing QML modules or libraries.


==================================================
CREDITS
==================================================

Caelestia Shell:
https://github.com/caelestia-dots/shell

Quickshell:
https://quickshell.outfoxxed.me

Hyprland:
https://hyprland.org

Ubuntu Hyprland Guide:
https://github.com/LinuxBeginnings/Ubuntu-Hyprland
