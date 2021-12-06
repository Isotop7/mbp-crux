# crux-mbp

This repo contains patches for the Crux Makefile in order to create a custom ISO for my MacBook Pro 15,2.

Tested linux kernel: `5.12.14`

## Setup Crux repo

```bash
mkdir -p ~/crux3.6
git clone git://crux.nu/system/iso.git
cd iso
cd ports
git clone git://crux.nu/ports/core.git
git clone git://crux.nu/ports/opt.git
git clone git://crux.nu/ports/xorg.git
cd ..
```

## Patch Makefile

```bash
git clone https://github.com/Isotop7/mbp-crux
patch -p1 < mbp-crux/Makefile.patch
cp mbp-crux/linux-5.12.14.config kernel/linux-5.12.14.config
```

## Copy mbp-patches

```bash
git clone https://github.com/aunali1/linux-mbp-arch.git
mkdir kernel/mbp-patches
cp linux-mbp-arch/*.patch
# Remove arch stuff
rm linux-mbp-arch/000*
```

## Build Crux

```bash
make kernel
make bootstrap
make iso
```

## Credits

All credits go to aunali1 for his [awesome work](https://github.com/aunali1/linux-mbp-arch).
