*Lightly* is a fork of breeze theme style that aims to be visually modern and minimalistic.

This fork is "porting" Lightly to Flatpak.

## Supported runtimes

org.kde.Platform/x86_64/6.7

## Installation

### Package

1. Download from releases 
2. Run `flatpak install lightly_6.7.flatpak` inside the dowload directory or double-click it to open and install in Discover

### Building and installing from source

``` bash
git clone --depth=1 https://github.com/MicrogamerCz/Lightly && cd Lightly

# User-wide installation
flatpak-builder build --user --force-clean --ccache --install org.kde.KStyle.Lightly.json 

# System-wide installation
sudo flatpak-builder build --force-clean --ccache --install org.kde.KStyle.Lightly.json
```

### Building and installing package from source

``` bash
git clone --depth=1 https://github.com/MicrogamerCz/Lightly && cd Lightly

flatpak-builder flatpak-build-dir --repo=lightly-master --force-clean --ccache org.kde.KStyle.Lightly.json

flatpak build-bundle --runtime lightly-master/ lightly_6.7.flatpak runtime/org.kde.KStyle.Lightly/x86_64/6.7
```