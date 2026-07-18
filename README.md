# Caelestia Shell on Ubuntu / Linux Mint

A guide to installing Caelestia Shell on Ubuntu-based systems
(Ubuntu / Linux Mint) using Hyprland.

This guide covers:

- Hyprland setup
- Qt 6.11.1 installation
- Qt Wayland
- Quickshell installation
- Caelestia Shell compilation
- cpptrace information
- Environment variables
- Hyprland Lua integration


==================================================
REQUIREMENTS
==================================================

Before starting:

- A brain
- A computer
- Fingers
- Hands
- Keyboard
- Patience


Software requirements:

- Ubuntu / Linux Mint
- Hyprland
- Qt 6.11.1
- Quickshell
- Caelestia Shell


Install Hyprland first:

https://github.com/LinuxBeginnings/Ubuntu-Hyprland


Make sure Hyprland is working before continuing.


==================================================
1. INSTALL BUILD DEPENDENCIES
==================================================


Open a terminal:

sudo apt update


Install required packages:

sudo apt install \
git \
cmake \
ninja-build \
build-essential \
pkg-config \
curl \
wget


Install Caelestia dependencies:

sudo apt install \
ddcutil \
brightnessctl \
libcava-dev \
network-manager \
lm-sensors \
fish \
aubio-tools \
libpipewire-0.3-dev \
qt6-declarative-dev \
qt6-base-dev \
libqalculate-dev \
swappy


Install fonts:


https://github.com/google/material-design-icons/tree/master/variablefont



==================================================
2. INSTALL QT 6.11.1
==================================================


Download the Qt installer:

https://www.qt.io/development/download-qt-installer-oss


Create a Qt account.

Download the Linux installer.

Run:

chmod +x qt-installer.run

./qt-installer.run


Login with your Qt account.


--------------------------------------------------
QT INSTALLATION OPTIONS
--------------------------------------------------


Guide 4:

Select:

Custom installation


Guide 5:

Expand:

Qt for development

then expand:

Qt


Guide 6:

Expand:

Qt 6.11.1


Select only:

Desktop


Do not select unnecessary components.


Guide 7:

Expand:

Additional Libraries


Guide 8:

Scroll down and find:

Qt Wayland


Enable:

Qt Wayland


Do NOT select:

Build Tools


Build tools are not required because we use
system packages.


Guide 9:

Accept the license agreement.


Guide 10:

Your installation should match the final screenshot.



==================================================
3. INSTALL QUICKSHELL
==================================================


Quickshell does not need to be built manually anymore.

Use the AvengeMedia DankLinux repository.


Add repository:

sudo add-apt-repository ppa:avengemedia/danklinux

sudo apt update


Install stable Quickshell:

sudo apt install quickshell


For the latest development version:

sudo apt install quickshell-git


Check:

qs --version



==================================================
4. INSTALL CAELESTIA SHELL
==================================================


Create Quickshell directory:

mkdir -p ~/.config/quickshell


Clone Caelestia:

cd ~/.config/quickshell

git clone https://github.com/caelestia-dots/shell.git caelestia


Enter folder:

cd caelestia


Configure build:

cmake -B build \
-G Ninja \
-DCMAKE_BUILD_TYPE=Release \
-DCMAKE_INSTALL_PREFIX=/


Compile:

cmake --build build


Install:

sudo cmake --install build



==================================================
5. CPPTRACE INFORMATION
==================================================


Caelestia uses cpptrace for its beat detection
library.

Recommended:

Install cpptrace and build normally.


If you do not want beat detection:

You can skip cpptrace.

The shell will still work.

Only affected features:

- Audio visualiser reactions
- Beat detection effects



==================================================
6. QT ENVIRONMENT VARIABLES
==================================================


Open:

nano ~/.bashrc


Add:


export Qt6_DIR=$HOME/Qt/6.11.1/gcc_64/lib/cmake/Qt6

export CMAKE_PREFIX_PATH=$HOME/Qt/6.11.1/gcc_64

export LD_LIBRARY_PATH=$HOME/Qt/6.11.1/gcc_64/lib

export QML_IMPORT_PATH=$HOME/.local/lib/qt6/qml:/usr/lib/qt6/qml

export QML2_IMPORT_PATH=$HOME/.local/lib/qt6/qml:/usr/lib/qt6/qml


Reload:

source ~/.bashrc



==================================================
7. TEST CAELESTIA
==================================================


Run:

caelestia shell -d


or:


qs -c caelestia



==================================================
8. START CAELESTIA WITH HYPRLAND LUA
==================================================


Open:

nano ~/.config/hypr/hyprland.lua


Add:


hl.on("hyprland.start", function()

    hl.exec_cmd(
        "bash -c 'source ~/.bashrc && sleep 2 && caelestia shell -d'"
    )

end)



==================================================
9. CAELESTIA LUA KEYBINDS
==================================================


Add to your Hyprland Lua configuration:


local function cael_bind(key,target,func)

    hl.bind(
        key,
        hl.dsp.exec_cmd(
        "quickshell ipc -c caelestia call "
        .. target .. " " .. func
        )
    )

end


-- Launcher

cael_bind(
"SUPER + A",
"drawers",
"toggle launcher"
)


-- Dashboard

cael_bind(
"SUPER + D",
"drawers",
"toggle dashboard"
)


-- Sidebar

cael_bind(
"SUPER + B",
"drawers",
"toggle sidebar"
)


-- Utilities

cael_bind(
"SUPER + U",
"drawers",
"toggle utilities"
)


-- Settings

cael_bind(
"SUPER + S",
"nexus",
"open"
)


-- Lock

cael_bind(
"SUPER + L",
"lock",
"lock"
)


-- Clear notifications

cael_bind(
"SUPER + SHIFT + N",
"notifs",
"clear"
)


-- Media

cael_bind(
"XF86AudioPlay",
"mpris",
"playPause"
)

cael_bind(
"XF86AudioNext",
"mpris",
"next"
)

cael_bind(
"XF86AudioPrev",
"mpris",
"previous"
)



==================================================
10. UPDATING CAELESTIA
==================================================


Go to your Caelestia folder:


cd ~/.config/quickshell/caelestia


Update:

git pull


Rebuild:

cmake --build build


Install:

sudo cmake --install build



==================================================
DONE
==================================================


You now have:

✓ Hyprland
✓ Qt 6.11.1
✓ Qt Wayland
✓ Quickshell
✓ Caelestia Shell
✓ Lua keybind integration


Enjoy your Caelestia desktop.
