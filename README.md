# Caelestia Shell + Hyprland Complete Setup Guide

A complete guide for installing and configuring **Caelestia Shell** with **Hyprland** on Ubuntu-based distributions (Ubuntu, Linux Mint, etc).

This guide includes:
- Hyprland
- Qt 6.11.1
- Quickshell
- Caelestia Shell
- Complete Hyprland Lua configuration with all keybindings

---

## 📋 Table of Contents

1. [Requirements](#requirements)
2. [Installation Steps](#installation-steps)
   - [1. Install Hyprland](#1-install-hyprland)
   - [2. Install Qt 6.11.1](#2-install-qt-6111)
   - [3. Install Build Tools](#3-install-build-tools)
   - [4. Install Quickshell](#4-install-quickshell)
   - [5. Install Caelestia Dependencies](#5-install-caelestia-dependencies)
   - [6. Install Caelestia Shell](#6-install-caelestia-shell)
   - [7. Setup Hyprland Configuration](#7-setup-hyprland-configuration)
   - [8. Launch Caelestia](#8-launch-caelestia)
3. [Complete Hyprland Lua Configuration](#complete-hyprland-lua-configuration)
4. [Complete Keybinding Reference](#complete-keybinding-reference)
   - [Window Management](#window-management)
   - [Window Grouping (Tabs)](#window-grouping-tabs)
   - [Focus & Navigation](#focus--navigation)
   - [Workspaces](#workspaces)
   - [Application Launchers](#application-launchers)
   - [Caelestia Shell Features](#caelestia-shell-features)
   - [Notifications](#notifications)
   - [Screenshots](#screenshots)
   - [Media Controls](#media-controls)
   - [System Controls](#system-controls)
5. [Layout Keybindings](#layout-keybindings)
6. [Troubleshooting](#troubleshooting)
7. [Updating](#updating)
8. [Customization Tips](#customization-tips)
9. [Resources](#resources)

---

## Requirements

Before starting, make sure you have:

- A brain 🧠
- A computer 💻
- Fingers, hands, and a keyboard
- Patience

Software requirements:

- Ubuntu / Linux Mint based system
- Internet connection
- Basic terminal knowledge

---

## Installation Steps

### 1. Install Hyprland

Caelestia Shell is designed to run on top of Hyprland.

If you do not already have Hyprland installed, follow:

https://github.com/LinuxBeginnings/Ubuntu-Hyprland

Make sure Hyprland starts correctly before continuing.

**IMPORTANT:** Caelestia is only a desktop shell. It is not a window manager. Hyprland handles window management while Caelestia provides the desktop environment.

---

### 2. Install Qt 6.11.1

#### Create a Qt account

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

#### Qt Installer Setup

Select:

```
Custom installation
```

Expand:

```
Qt for development
```

Then expand:

```
Qt
```

Expand:

```
Qt 6.11.1
```

Select only:

```
Desktop
```

Do not select extra platforms.

Expand:

```
Additional libraries
```

under Qt 6.11.1.

Scroll down and find:

```
Qt Wayland
```

Enable:

```
Qt Wayland
```

This is required for Wayland applications.

Do not install:

```
Build Tools
```

They are not required. We will use GCC, CMake, and Ninja from the system.

Accept the license agreement and continue with installation.

---

### 3. Install Build Tools

Install the required build packages:

```bash
sudo apt update
sudo apt install git cmake ninja-build build-essential
```

---

### 4. Install Quickshell

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

### 5. Install Caelestia Dependencies

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

### 6. Install Caelestia Shell

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

### 7. Setup Hyprland Configuration

Create the Hyprland config directory:

```bash
mkdir -p ~/.config/hypr
```

Create the main configuration file `~/.config/hypr/hyprland.lua`:

```bash
nano ~/.config/hypr/hyprland.lua
```

Copy and paste the complete Hyprland Lua configuration from the next section.

---

### 8. Launch Caelestia

Start the shell:

```bash
caelestia shell -d
```

or:

```bash
qs -c caelestia
```

To start Hyprland with Caelestia automatically:

```bash
Hyprland
```

Once in Hyprland, Caelestia will start automatically if configured in the autostart section.

---

## Complete Hyprland Lua Configuration

Save this as `~/.config/hypr/hyprland.lua`:

```lua
-- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
-- HYPRLAND + CAELESTIA SHELL COMPLETE CONFIGURATION    --
-- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

------------------
---- MONITORS ----
------------------

hl.monitor({
    output   = "",
    mode     = "preferred",
    position = "auto",
    scale    = 1.33,
})

---------------------
---- MY PROGRAMS ----
---------------------

local terminal    = "kitty"
local fileManager = "dolphin"
local browser     = "firefox"
local editor      = "code"
local calculator  = "gnome-calculator"

-------------------
---- AUTOSTART ----
-------------------

hl.on("hyprland.start", function()
    -- Start Caelestia Shell with proper environment
    hl.exec_cmd("bash -c 'source ~/.bashrc && sleep 2 && caelestia shell -d'")
end)

-------------------------------
---- ENVIRONMENT VARIABLES ----
-------------------------------

local home = os.getenv("HOME")

hl.env("XCURSOR_SIZE", "24")
hl.env("HYPRCURSOR_SIZE", "24")

-- Qt 6.11.1 and Caelestia environment
hl.env("PATH", home .. "/.local/bin:" .. home .. "/Qt/6.11.1/gcc_64/bin:" .. os.getenv("PATH"))
hl.env("LD_LIBRARY_PATH", home .. "/Qt/6.11.1/gcc_64/lib")
hl.env("Qt6_DIR", home .. "/Qt/6.11.1/gcc_64/lib/cmake/Qt6")
hl.env("CMAKE_PREFIX_PATH", home .. "/Qt/6.11.1/gcc_64")
hl.env("QML2_IMPORT_PATH", home .. "/.local/lib/qt6/qml:/usr/lib/qt6/qml")
hl.env("QML_IMPORT_PATH", home .. "/.local/lib/qt6/qml:/usr/lib/qt6/qml")

-----------------------
---- LOOK AND FEEL ----
-----------------------

hl.config({
    general = {
        gaps_in  = 5,
        gaps_out = 20,
        border_size = 2,
        
        col = {
            active_border   = { colors = {"rgba(33ccffee)", "rgba(00ff99ee)"}, angle = 45 },
            inactive_border = "rgba(595959aa)",
        },
        
        resize_on_border = false,
        allow_tearing = false,
        layout = "dwindle",
    },
    
    decoration = {
        rounding       = 10,
        rounding_power = 2,
        active_opacity   = 1.0,
        inactive_opacity = 1.0,
        
        shadow = {
            enabled      = true,
            range        = 4,
            render_power = 3,
            color        = 0xee1a1a1a,
        },
        
        blur = {
            enabled   = true,
            size      = 3,
            passes    = 1,
            vibrancy  = 0.1696,
        },
    },
    
    animations = {
        enabled = true,
    },
})

hl.config({
    xwayland = {
        force_zero_scaling = true
    }
})

-- Animation Curves
hl.curve("easeOutQuint",   { type = "bezier", points = { {0.23, 1},    {0.32, 1}    } })
hl.curve("easeInOutCubic", { type = "bezier", points = { {0.65, 0.05}, {0.36, 1}    } })
hl.curve("linear",         { type = "bezier", points = { {0, 0},       {1, 1}       } })
hl.curve("almostLinear",   { type = "bezier", points = { {0.5, 0.5},   {0.75, 1}    } })
hl.curve("quick",          { type = "bezier", points = { {0.15, 0},    {0.1, 1}     } })
hl.curve("easy",           { type = "spring", mass = 1, stiffness = 71.2633, dampening = 15.8273644 })

-- Animations
hl.animation({ leaf = "global",        enabled = true,  speed = 10,   bezier = "default" })
hl.animation({ leaf = "border",        enabled = true,  speed = 5.39, bezier = "easeOutQuint" })
hl.animation({ leaf = "windows",       enabled = true,  speed = 4.79, spring = "easy" })
hl.animation({ leaf = "windowsIn",     enabled = true,  speed = 4.1,  spring = "easy",         style = "popin 87%" })
hl.animation({ leaf = "windowsOut",    enabled = true,  speed = 1.49, bezier = "linear",       style = "popin 87%" })
hl.animation({ leaf = "fadeIn",        enabled = true,  speed = 1.73, bezier = "almostLinear" })
hl.animation({ leaf = "fadeOut",       enabled = true,  speed = 1.46, bezier = "almostLinear" })
hl.animation({ leaf = "fade",          enabled = true,  speed = 3.03, bezier = "quick" })
hl.animation({ leaf = "layers",        enabled = true,  speed = 3.81, bezier = "easeOutQuint" })
hl.animation({ leaf = "layersIn",      enabled = true,  speed = 4,    bezier = "easeOutQuint", style = "fade" })
hl.animation({ leaf = "layersOut",     enabled = true,  speed = 1.5,  bezier = "linear",       style = "fade" })
hl.animation({ leaf = "fadeLayersIn",  enabled = true,  speed = 1.79, bezier = "almostLinear" })
hl.animation({ leaf = "fadeLayersOut", enabled = true,  speed = 1.39, bezier = "almostLinear" })
hl.animation({ leaf = "workspaces",    enabled = true,  speed = 1.94, bezier = "almostLinear", style = "fade" })
hl.animation({ leaf = "workspacesIn",  enabled = true,  speed = 1.21, bezier = "almostLinear", style = "fade" })
hl.animation({ leaf = "workspacesOut", enabled = true,  speed = 1.94, bezier = "almostLinear", style = "fade" })
hl.animation({ leaf = "zoomFactor",    enabled = true,  speed = 7,    bezier = "quick" })

-- Layouts
hl.config({
    dwindle = {
        preserve_split = true,
    },
})

hl.config({
    master = {
        new_status = "master",
    },
})

hl.config({
    scrolling = {
        fullscreen_on_one_column = true,
    },
})

---------------
---- INPUT ----
---------------

hl.config({
    input = {
        kb_layout  = "us",
        kb_variant = "",
        kb_model   = "",
        kb_options = "",
        kb_rules   = "",
        follow_mouse = 1,
        sensitivity = 0,
        
        touchpad = {
            natural_scroll = false,
        },
    },
})

hl.gesture({
    fingers = 3,
    direction = "horizontal",
    action = "workspace"
})

---------------------
---- KEYBINDINGS ----
---------------------

local mainMod = "SUPER" -- Windows key as main modifier

-- ============================================
-- HYPRLAND CORE BINDINGS
-- ============================================

-- Application Launchers
hl.bind(mainMod .. " + RETURN", hl.dsp.exec_cmd(terminal))
hl.bind(mainMod .. " + SHIFT + RETURN", hl.dsp.exec_cmd(browser))
hl.bind(mainMod .. " + E", hl.dsp.exec_cmd(fileManager))
hl.bind(mainMod .. " + SHIFT + E", hl.dsp.exec_cmd(editor))
hl.bind(mainMod .. " + C", hl.dsp.exec_cmd(calculator))

-- Window Controls
hl.bind(mainMod .. " + Q", hl.dsp.exec_cmd(terminal))  -- Quick terminal
hl.bind(mainMod .. " + C", hl.dsp.window.close())      -- Close window
hl.bind(mainMod .. " + F", hl.dsp.window.fullscreen({ mode = "fullscreen", action = "toggle" }))
hl.bind(mainMod .. " + SHIFT + F", hl.dsp.window.fullscreen({ mode = "maximized", action = "toggle" }))
hl.bind(mainMod .. " + W", hl.dsp.window.float({ action = "toggle" }))
hl.bind(mainMod .. " + Z", hl.dsp.window.move({ workspace = "special:minimized" }))
hl.bind(mainMod .. " + SHIFT + Z", function()
    hl.dispatch(hl.dsp.workspace.toggle_special("minimized"))
end)

-- Window Grouping (Tabs)
hl.bind(mainMod .. " + G", hl.dsp.group.toggle())
hl.bind(mainMod .. " + H", hl.dsp.group.next())
hl.bind(mainMod .. " + SHIFT + H", hl.dsp.group.prev())

-- Window Stacking
hl.bind(mainMod .. " + T", hl.dsp.window.alter_zorder({ mode = "top" }))
hl.bind(mainMod .. " + SHIFT + T", hl.dsp.window.pin({ action = "toggle" }))

-- Focus Navigation
hl.bind(mainMod .. " + left",  hl.dsp.focus({ direction = "left" }))
hl.bind(mainMod .. " + right", hl.dsp.focus({ direction = "right" }))
hl.bind(mainMod .. " + up",    hl.dsp.focus({ direction = "up" }))
hl.bind(mainMod .. " + down",  hl.dsp.focus({ direction = "down" }))

-- Workspace Navigation
for i = 1, 10 do
    local key = i % 10
    hl.bind(mainMod .. " + " .. key,             hl.dsp.focus({ workspace = i}))
    hl.bind(mainMod .. " + SHIFT + " .. key,     hl.dsp.window.move({ workspace = i }))
end

-- Workspace Scrolling
hl.bind(mainMod .. " + mouse_down", hl.dsp.focus({ workspace = "e+1" }))
hl.bind(mainMod .. " + mouse_up",   hl.dsp.focus({ workspace = "e-1" }))

-- Layout Controls
hl.bind(mainMod .. " + J", hl.dsp.layout("togglesplit"))  -- Dwindle only

-- Mouse Window Controls
hl.bind(mainMod .. " + mouse:272", hl.dsp.window.drag(),   { mouse = true })
hl.bind(mainMod .. " + mouse:273", hl.dsp.window.resize(), { mouse = true })

-- System Controls
hl.bind(mainMod .. " + M", hl.dsp.exec_cmd("command -v hyprshutdown >/dev/null 2>&1 && hyprshutdown || hyprctl dispatch 'hl.dsp.exit()'"))
hl.bind(mainMod .. " + SHIFT + R", hl.dsp.exec_cmd("hyprctl dispatch 'hl.dsp.exit()' && Hyprland"))
hl.bind(mainMod .. " + SHIFT + X", hl.dsp.exec_cmd("hyprctl dispatch 'hl.dsp.exit()'"))

-- ============================================
-- CAELESTIA SHELL BINDINGS
-- ============================================

local function cael_bind(key, target, func)
    hl.bind(key, hl.dsp.exec_cmd(home .. "/.local/bin/quickshell ipc -c caelestia call " .. target .. " " .. func))
end

local function screenshot_bind(key, mode)
    hl.bind(key, hl.dsp.exec_cmd(home .. "/.local/bin/screenshot.sh " .. mode))
end

-- Launch Caelestia Features
cael_bind(mainMod .. " + A", "drawers", "toggle launcher")        -- App Launcher
cael_bind(mainMod .. " + D", "drawers", "toggle dashboard")       -- Dashboard
cael_bind(mainMod .. " + B", "drawers", "toggle sidebar")         -- Sidebar
cael_bind(mainMod .. " + U", "drawers", "toggle utilities")       -- Utilities
cael_bind(mainMod .. " + S", "nexus", "open")                     -- Settings/Control Center

-- Notifications
cael_bind(mainMod .. " + N", "notifs", "toggleDnd")               -- Toggle DND
cael_bind(mainMod .. " + SHIFT + N", "notifs", "clear")           -- Clear all notifications
cael_bind(mainMod .. " + CTRL + N", "notifs", "enableDnd")        -- Enable DND
cael_bind(mainMod .. " + CTRL + SHIFT + N", "notifs", "disableDnd") -- Disable DND

-- Screenshots
screenshot_bind("PRINT", "")                                      -- Full screenshot
screenshot_bind("SHIFT + PRINT", "region")                        -- Region screenshot
screenshot_bind(mainMod .. " + PRINT", "clipboard")               -- Full to clipboard
screenshot_bind(mainMod .. " + SHIFT + PRINT", "region-clipboard") -- Region to clipboard

-- Color Picker
cael_bind(mainMod .. " + P", "picker", "open")                    -- Color picker
cael_bind(mainMod .. " + SHIFT + P", "picker", "openClip")        -- Color picker to clipboard

-- System Controls
cael_bind(mainMod .. " + L", "lock", "lock")                      -- Lock screen
cael_bind(mainMod .. " + I", "idleInhibitor", "toggle")           -- Idle inhibitor toggle
cael_bind(mainMod .. " + SHIFT + O", "audio", "cycleOutput")      -- Cycle audio output

-- Hyprland Integration
cael_bind(mainMod .. " + SHIFT + S", "hypr", "cycleSpecialWorkspace next") -- Cycle scratchpad
cael_bind(mainMod .. " + CTRL + SHIFT + R", "hypr", "refreshDevices")      -- Refresh devices

-- ============================================
-- MEDIA KEYS
-- ============================================

-- Volume Controls
hl.bind("XF86AudioRaiseVolume", hl.dsp.exec_cmd("wpctl set-volume -l 1 @DEFAULT_AUDIO_SINK@ 5%+"), { locked = true, repeating = true })
hl.bind("XF86AudioLowerVolume", hl.dsp.exec_cmd("wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%-"),      { locked = true, repeating = true })
hl.bind("XF86AudioMute",        hl.dsp.exec_cmd("wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle"),     { locked = true, repeating = true })
hl.bind("XF86AudioMicMute",     hl.dsp.exec_cmd("wpctl set-mute @DEFAULT_AUDIO_SOURCE@ toggle"),   { locked = true, repeating = true })

-- Media Playback (Caelestia MPRIS)
cael_bind("XF86AudioPlay", "mpris", "playPause")
cael_bind("XF86AudioNext", "mpris", "next")
cael_bind("XF86AudioPrev", "mpris", "previous")

-- Brightness Controls
hl.bind("XF86MonBrightnessUp",  hl.dsp.exec_cmd("brightnessctl -e4 -n2 set 5%+"),   { locked = true, repeating = true })
hl.bind("XF86MonBrightnessDown",hl.dsp.exec_cmd("brightnessctl -e4 -n2 set 5%-"),   { locked = true, repeating = true })

--------------------------------
---- WINDOWS AND WORKSPACES ----
--------------------------------

-- Suppress maximize events (useful for Caelestia)
local suppressMaximizeRule = hl.window_rule({
    name  = "suppress-maximize-events",
    match = { class = ".*" },
    suppress_event = "maximize",
})

-- Fix XWayland dragging issues
hl.window_rule({
    name  = "fix-xwayland-drags",
    match = {
        class      = "^$",
        title      = "^$",
        xwayland   = true,
        float      = true,
        fullscreen = false,
        pin        = false,
    },
    no_focus = true,
})

-- Hyprland-run window rule
hl.window_rule({
    name  = "move-hyprland-run",
    match = { class = "hyprland-run" },
    move  = "20 monitor_h-120",
    float = true,
})

print("✅ Hyprland + Caelestia configuration loaded successfully!")
```

---

## Complete Keybinding Reference

### Window Management

| Keybinding | Action |
|------------|--------|
| `SUPER + Q` | Open terminal |
| `SUPER + W` | Toggle floating window |
| `SUPER + F` | Toggle fullscreen |
| `SUPER + SHIFT + F` | Toggle maximize |
| `SUPER + C` | Close window |
| `SUPER + Z` | Minimize window (to scratchpad) |
| `SUPER + SHIFT + Z` | Restore minimized window |

### Window Grouping (Tabs)

| Keybinding | Action |
|------------|--------|
| `SUPER + G` | Toggle window group |
| `SUPER + H` | Next window in group |
| `SUPER + SHIFT + H` | Previous window in group |
| `SUPER + T` | Toggle always on top |
| `SUPER + SHIFT + T` | Toggle pinned window |

### Focus & Navigation

| Keybinding | Action |
|------------|--------|
| `SUPER + ←` | Focus window to the left |
| `SUPER + →` | Focus window to the right |
| `SUPER + ↑` | Focus window above |
| `SUPER + ↓` | Focus window below |
| `SUPER + mouse:272` | Drag window |
| `SUPER + mouse:273` | Resize window |

### Workspaces

| Keybinding | Action |
|------------|--------|
| `SUPER + 1-9,0` | Switch to workspace 1-10 |
| `SUPER + SHIFT + 1-9,0` | Move window to workspace 1-10 |
| `SUPER + mouse_down` | Next workspace |
| `SUPER + mouse_up` | Previous workspace |
| `SUPER + SHIFT + S` | Cycle special workspace (scratchpad) |

### Application Launchers

| Keybinding | Action |
|------------|--------|
| `SUPER + RETURN` | Open terminal |
| `SUPER + SHIFT + RETURN` | Open browser |
| `SUPER + E` | Open file manager |
| `SUPER + SHIFT + E` | Open code editor |
| `SUPER + C` | Open calculator |

### Caelestia Shell Features

| Keybinding | Action |
|------------|--------|
| `SUPER + A` | Open app launcher |
| `SUPER + D` | Open dashboard |
| `SUPER + B` | Open sidebar |
| `SUPER + U` | Open utilities drawer |
| `SUPER + S` | Open settings/control center |
| `SUPER + L` | Lock screen |
| `SUPER + P` | Open color picker |
| `SUPER + SHIFT + P` | Color picker (copy to clipboard) |

### Notifications

| Keybinding | Action |
|------------|--------|
| `SUPER + N` | Toggle Do Not Disturb |
| `SUPER + SHIFT + N` | Clear all notifications |
| `SUPER + CTRL + N` | Enable Do Not Disturb |
| `SUPER + CTRL + SHIFT + N` | Disable Do Not Disturb |

### Screenshots

| Keybinding | Action |
|------------|--------|
| `PRINT` | Full screenshot (saved to ~/Pictures/) |
| `SHIFT + PRINT` | Region screenshot (saved to ~/Pictures/) |
| `SUPER + PRINT` | Full screenshot (to clipboard) |
| `SUPER + SHIFT + PRINT` | Region screenshot (to clipboard) |

### Media Controls

| Keybinding | Action |
|------------|--------|
| `XF86AudioRaiseVolume` | Increase volume |
| `XF86AudioLowerVolume` | Decrease volume |
| `XF86AudioMute` | Toggle mute |
| `XF86AudioMicMute` | Toggle mic mute |
| `XF86AudioPlay` | Play/Pause |
| `XF86AudioNext` | Next track |
| `XF86AudioPrev` | Previous track |

### System Controls

| Keybinding | Action |
|------------|--------|
| `XF86MonBrightnessUp` | Increase brightness |
| `XF86MonBrightnessDown` | Decrease brightness |
| `SUPER + SHIFT + R` | Reload Hyprland |
| `SUPER + SHIFT + X` | Exit Hyprland |
| `SUPER + M` | Shutdown menu |
| `SUPER + I` | Toggle idle inhibitor |
| `SUPER + SHIFT + O` | Cycle audio output |
| `SUPER + CTRL + SHIFT + R` | Refresh devices |

---

## Layout Keybindings

Hyprland supports multiple layout modes. Here's how to switch between them:

### Dwindle Layout (Default)

```lua
hl.config({
    dwindle = {
        preserve_split = true,  -- Keep split ratios
        pseudotile = true,      -- Enable pseudotiling
        force_split = 2,        -- Split direction preference
    }
})
```

### Master Layout

```lua
hl.config({
    master = {
        new_status = "master",  -- New windows become master
        orientation = "center", -- Master window position
    }
})
```

### Layout Switching Keybindings

Add these to your `hyprland.lua` if you want to switch layouts:

```lua
-- Layout switching
hl.bind("SUPER + CTRL + left",  hl.dsp.exec_cmd("hyprctl dispatch layoutmsg togglesplit"))   -- Toggle split
hl.bind("SUPER + CTRL + right", hl.dsp.exec_cmd("hyprctl dispatch layoutmsg swapwithmaster")) -- Swap with master
hl.bind("SUPER + CTRL + up",    hl.dsp.exec_cmd("hyprctl dispatch layoutmsg addmaster"))      -- Add master
hl.bind("SUPER + CTRL + down",  hl.dsp.exec_cmd("hyprctl dispatch layoutmsg removemaster"))   -- Remove master

-- Switch layout
hl.bind("SUPER + CTRL + SPACE", hl.dsp.exec_cmd("hyprctl dispatch layoutmsg cyclenext"))      -- Cycle layouts
```

---

## Troubleshooting

### Caelestia doesn't start

Check environment:
```bash
echo $CMAKE_PREFIX_PATH  # Should point to ~/Qt/6.11.1/gcc_64
echo $QML_IMPORT_PATH    # Should include /usr/lib/qt6/qml
```

Try manual start:
```bash
qs -c caelestia
```

### Qt not found

```bash
export CMAKE_PREFIX_PATH=$HOME/Qt/6.11.1/gcc_64
export QML_IMPORT_PATH=$HOME/.local/lib/qt6/qml:/usr/lib/qt6/qml
```

### Missing QML modules

```bash
sudo apt install qml6-module-qtquick-controls qml6-module-qtquick-layouts qml6-module-qtgraphicaleffects
```

### Brightness controls not working

```bash
sudo usermod -a -G video $USER
# Reboot or log out/in
```

### Audio controls not working

```bash
# Install PipeWire utilities
sudo apt install pipewire-pulse wireplumber
systemctl --user enable --now wireplumber
```

---

## Updating

### Update Hyprland

```bash
# If installed via git
cd ~/hyprland
git pull
make all
sudo make install
```

### Update Caelestia

```bash
cd ~/.config/quickshell/caelestia
git checkout main
git pull
cmake --build build
sudo cmake --install build
```

### Update Keybindings

Edit `~/.config/hypr/hyprland.lua` and restart Hyprland with `SUPER + SHIFT + R`.

---

## Customization Tips

### Add Custom Keybindings

Add to `hyprland.lua`:
```lua
hl.bind("SUPER + SHIFT + K", hl.dsp.exec_cmd("your-command-here"))
```

### Change Monitor Settings

```lua
hl.monitor({
    output   = "DP-1",      -- Your monitor name
    mode     = "1920x1080@144",
    position = "0x0",
    scale    = 1.0,
})
```

### Modify Gaps

```lua
hl.config({
    general = {
        gaps_in  = 10,      -- Inner gaps
        gaps_out = 30,      -- Outer gaps
    }
})
```

---

## Resources

- [Hyprland Wiki](https://wiki.hypr.land/)
- [Caelestia Shell](https://github.com/caelestia-dots/shell)
- [Quickshell Documentation](https://quickshell.outfoxxed.me)
- [Ubuntu Hyprland Guide](https://github.com/LinuxBeginnings/Ubuntu-Hyprland)

---

*This configuration provides a complete, modern desktop experience combining Hyprland's window management with Caelestia's beautiful shell interface.*
