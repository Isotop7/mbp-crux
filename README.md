# crux-mbp

This repo contains patches for the Crux Makefile in order to create a custom ISO for my MacBook Pro 15,2.

Currently tested linux kernel: `5.12.14` (as mbp-arch is using this one)
- **Note**: This is still work in progress and hasn't been tested on any real hardware.

## Setup Crux repo

```bash
mkdir -p ~/crux3.6 && cd ~/crux3.6

# Clone ISO repo
git clone -b 3.6 git://crux.nu/system/iso.git

# Move to ports dir
cd iso/ports

# Clone ports
git clone -b 3.6 git://crux.nu/ports/core.git
git clone -b 3.6 git://crux.nu/ports/opt.git
git clone -b 3.6 git://crux.nu/ports/xorg.git

# Move back to ISO dir
cd ..
```

## Install prerequisites
```bash
# Generate missing packages file
make -B packages.all

# Install missing packages
prt-get depinst $(cat packages.all)
```

## Patch files

```bash
# Clone this repo
git clone https://github.com/Isotop7/mbp-crux

# Patching Makefile
patch -p1 < mbp-crux/Makefile.patch

# Patching pkgmk.conf
patch -p1 < mbp-crux/pkgmk.conf.patch

# Patching initramfs
patch -p1 < mbp-crux/init.patch
patch -p1 < mbp-crux/initramfs.lst.patch

# Copy kernel config
cp mbp-crux/linux-5.12.14.config kernel/
```

## Clone and apply kernel patches

```bash
# Clone repo
git clone https://github.com/aunali1/linux-mbp-arch.git

# Copy patches
mkdir kernel/mbp-patches
cp linux-mbp-arch/*.patch kernel/mbp-patches

# Remove archlinux stuff
rm kernel/mbp-patches/000*
```

## Clone and prepare kernel drivers

```bash
# Clone repo
git clone -b mbp15 https://github.com/t2linux/apple-ib-drv.git

# Switch to cloned repo and apply patch
cd apple-ib-drv && patch -p1 < ../mbp-crux/apple-ib-drv/Makefile.patch

# Switch back to base dir
cd ..

# Clone repo
git clone -b aur https://github.com/t2linux/apple-bce-drv.git

# Switch to cloned repo and apply patch
cd apple-bce-drv && patch -p1 < ../mbp-crux/apple-bce-drv/Makefile.patch

# Switch back to base dir
cd ..

```

## Build Crux

```bash
# Create kernel
make kernel

# Mandatory
# THIS CAN TAKE A LONG TIME
make bootstrap

# Create ISO
make iso
```

## Credits

All credits go to aunali1 for his [awesome work](https://github.com/aunali1/linux-mbp-arch).
The official ISO build for Crux was used as a template and is described [here](https://crux.nu/Wiki/OfficialISOBuildProcess).
