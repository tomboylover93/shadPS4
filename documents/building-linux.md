<!--
SPDX-FileCopyrightText: 2024 shadPS4 Emulator Project
SPDX-License-Identifier: GPL-2.0-or-later
-->

## Build shadPS4 for Linux

First and foremost, Clang 18 is the **recommended compiler** as it is used for official builds and CI. If you build with GCC, you might encounter issues — please report any you find. Additionally, if you choose to use GCC, please build shadPS4 with Clang at least once before creating an `[APP BUG]` issue or submitting a pull request.

## Preparatory steps

### Installing dependencies

#### Debian & Ubuntu

```
sudo apt install build-essential clang git cmake libasound2-dev libpulse-dev libopenal-dev libssl-dev zlib1g-dev libedit-dev libudev-dev libevdev-dev libsdl2-dev libjack-dev libsndio-dev qt6-base-dev qt6-tools-dev qt6-multimedia-dev libvulkan-dev vulkan-validationlayers
```

#### Fedora

```
sudo dnf install clang git cmake libatomic alsa-lib-devel pipewire-jack-audio-connection-kit-devel openal-devel openssl-devel libevdev-devel libudev-devel libXext-devel qt6-qtbase-devel qt6-qtbase-private-devel qt6-qtmultimedia-devel qt6-qtsvg-devel qt6-qttools-devel vulkan-devel vulkan-validation-layers
```

#### Arch Linux

```
sudo pacman -S base-devel clang git cmake sndio jack2 openal qt6-base qt6-declarative qt6-multimedia sdl2 vulkan-validation-layers
```

**Note**: The `shadps4-git` AUR package is not maintained by any of the developers, and it uses GCC as the compiler as opposed to Clang. Use at your own discretion.
#### OpenSUSE

```
sudo zypper install clang git cmake libasound2 libpulse-devel libsndio7 libjack-devel openal-soft-devel libopenssl-devel zlib-devel libedit-devel systemd-devel libevdev-devel qt6-base-devel qt6-multimedia-devel qt6-svg-devel qt6-linguist-devel qt6-gui-private-devel vulkan-devel vulkan-validationlayers
```

#### Other Linux distributions

You can try one of two methods:

- Search the packages by name and install them with your package manager, or
- Install [distrobox](https://distrobox.it/), create a container using any of the distributions cited above as a base, for Arch Linux you'd do:

```
distrobox create --name archlinux --init --image archlinux:latest
```

and install the dependencies on that container as cited above.
This option is **highly recommended** for NixOS and distributions with immutable/atomic filesystems (example: Fedora Kinoite, SteamOS).
### Cloning

```
git clone --recursive https://github.com/shadps4-emu/shadPS4.git
cd shadPS4
```

## Building

There are 3 options you can choose from. Option 1 is **highly recommended**.

#### Option 1: Terminal-only

Generate the build directory in the shadPS4 directory.

```
cmake -S . -B build/ -DENABLE_QT_GUI=ON -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++
```

To disable the Qt GUI, remove the `-DENABLE_QT_GUI=ON` flag. To change the build type (for debugging), add `-DCMAKE_BUILD_TYPE=Debug`.

Enter the directory:

```
cd build/
```

Use CMake to build the project:

```
cmake --build . --parallel$(nproc)
```

If your computer freezes during this step, this could be caused by excessive system resource usage. In that case, remove `--parallel$(nproc)`.

Now run the emulator. If Qt was enabled at configure time:

```
./shadps4
```

Otherwise, specify the path to your PKG's boot file:

```
./shadps4 /"PATH"/"TO"/"GAME"/"FOLDER"/eboot.bin
```

#### Option 2: Configuring with cmake-gui

`cmake-gui` should be installed by default alongside `cmake`, if not search for the package in your package manager and install it.

