# Building OpenWrt for Cudy WR1300 v4 from Source

↩️ [Русская версия (BUILD.md)](BUILD.md)

The patch adds support for the Cudy WR1300 v4. Patch author: Jan Brudna, [patch page (v2) on Patchwork](https://patchwork.ozlabs.org/project/openwrt/patch/20260513183938.242137-1-jan.brudna@gmail.com/). Tested on OpenWrt `v25.12.5`.

Before you start, set up the build environment: see the [list of dependencies](https://openwrt.org/docs/guide-developer/toolchain/install-buildsystem). Do not build as root, and make sure you have enough free disk space.

1. Clone OpenWrt and check out the required version:
```bash
git clone https://github.com/openwrt/openwrt.git
cd openwrt
git checkout v25.12.5
```

2. Update the feeds:
```bash
./scripts/feeds update -a
./scripts/feeds install -a
```

3. Set a name and email for git (locally, needed by `git am`) and apply the patch (the original authorship is preserved):
```bash
git config user.name "Your Name"
git config user.email "you@example.com"
curl -L https://raw.githubusercontent.com/LinSwitch/cudy-wr1300-v4-openwrt/refs/heads/main/v2-ramips-add-support-for-Cudy-WR1300-v4.patch | git am
```

4. Download the official configuration file for the target architecture. The version in the link must match the tag from step 1:
```bash
curl -L https://downloads.openwrt.org/releases/25.12.5/targets/ramips/mt7621/config.buildinfo -o .config
```

5. Select the device profile and check the configuration. Target System (`MediaTek Ralink MIPS`) and Subtarget (`MT7621 based boards`) should already be selected; change only Target Profile to `Cudy WR1300 v4`:
```bash
make menuconfig   # Target Profile: Cudy WR1300 v4
make defconfig
```

6. Download all sources in advance, then start the build:
```bash
make download
make -j$(nproc) V=s
```
   If the download hangs, run `make download V=s` to see which file it is stuck on. Download that file manually from [sources.openwrt.org](https://sources.openwrt.org/) into the `dl` folder, for example:
```bash
curl -L -O --output-dir dl https://sources.openwrt.org/bzip2-1.0.8.tar.gz
```
   Then run `make download` again, and once it finishes, run the build command.

The finished images are in `bin/targets/ramips/mt7621/`.