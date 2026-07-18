# Installing Caelestia Shell on Linux Mint / Ubuntu

> A guide for getting Caelestia Shell running on Ubuntu-based distributions using Qt 6.11 and Quickshell.

## Requirements

Before you begin, make sure you have:

- 🧠 A brain (recommended)
- 💻 A computer
- ✋ Hands
- 👆 Fingers
- ⌨️ A keyboard
- 🖥️ Hyprland

If you don't have Hyprland installed yet, follow this excellent guide first:

https://github.com/LinuxBeginnings/Ubuntu-Hyprland

Once Hyprland is working, come back here.

---

# 1. Install Qt 6.11

**Do NOT install Qt from Ubuntu's repositories.**

Quickshell and Caelestia work much better using the official Qt installer.

## Step 1

Create a free Qt account:

https://www.qt.io/

Download the **Qt Online Installer for Linux** and run it.

Log into your Qt account.

Continue until you reach the installation page.

---

## Step 2

Choose:

**Custom Installation**

*(Screenshot: guide-4.png)*

---

## Step 3

Expand:

```
Qt
```

Then expand:

```
Qt 6.11
```

*(Screenshot: guide-5.png)*

---

## Step 4

Select **only**:

```
Desktop gcc 64-bit
```

Everything else inside Qt 6.11 should remain unchecked.

Do **NOT** install:

- Android
- WebAssembly
- Sources
- Documentation
- Examples
- Debug Information
- Additional Libraries (for now)

*(Screenshot: guide-6.png)*

---

## Step 5

Now expand:

```
Additional Libraries
```

Select only:

```
Qt Wayland
```

Leave every other library unchecked.

*(Screenshot: guide-7.png)*

---

## Step 6

Expand:

```
Developer and Designer Tools
```

Uncheck everything.

You do **not** need:

- Qt Creator
- Design Studio
- CMake
- Ninja
- Maintenance Tool extras
- Any build tools

*(Screenshot: guide-8.png)*

---

## Step 7

Continue through the installer and install Qt.

When finished, Qt will normally be installed to:

```
~/Qt/6.11.1/gcc_64
```

(or whatever the latest 6.11.x version is.)

---

# 2. Install Quickshell

Thankfully, you **no longer need to compile Quickshell yourself.**

Add the DankLinux repository:

```bash
sudo add-apt-repository ppa:avengemedia/danklinux
sudo apt update
```

Install the latest release:

```bash
sudo apt install quickshell
```

Or if you want bleeding edge:

```bash
sudo apt install quickshell-git
```

---

# 3. Install Dependencies

```bash
sudo apt install \
git cmake ninja-build \
network-manager brightnessctl \
ddcutil fish \
lm-sensors libcava \
libpipewire-0.3-dev \
libqalculate-dev \
libglib2.0-dev \
qt6-base-dev \
qt6-declarative-dev \
qt6-wayland \
libgtk-3-dev
```

---

# 4. Install Caelestia CLI

Clone and build the CLI following its official instructions.

---

# 5. Clone Caelestia Shell

```bash
mkdir -p ~/.config/quickshell
cd ~/.config/quickshell

git clone https://github.com/caelestia-dots/shell.git caelestia

cd caelestia
```

---

# 6. Build Caelestia

```bash
cmake -B build \
-G Ninja \
-DCMAKE_BUILD_TYPE=Release \
-DCMAKE_INSTALL_PREFIX=/
```

```bash
cmake --build build
```

```bash
sudo cmake --install build
```

---

# 7. Start Caelestia

```bash
caelestia shell -d
```

or

```bash
qs -c caelestia
```

---

# Updating

Whenever a new version releases:

```bash
cd ~/.config/quickshell/caelestia

git switch main

git pull

cmake --build build

sudo cmake --install build
```

---

# Optional: Skipping cpptrace

Some systems may fail to build because of **cpptrace**.

If this happens, you have two options:

### Option 1 (Recommended)

Install a packaged version of cpptrace if your distribution provides one.

### Option 2

Disable cpptrace in the CMake configuration before building.

This may require editing the project's CMake configuration depending on the version of Caelestia you're building.

The shell will still work, but crash stack traces will be unavailable.

---

# Notes

During development of this guide we discovered that:

- Qt 6.9 caused numerous build issues.
- Qt 6.11 builds successfully.
- Quickshell no longer needs compiling from source thanks to the DankLinux PPA.
- Caelestia itself still needs to be compiled from source.
- Keep the `build/` folder after compiling so future updates only require rebuilding instead of reconfiguring.

Enjoy your shiny desktop! ✨
