# MiniDLNA audio-only patch
This is a patch for minidlna for disabling video tagging functionality and thus substantially reducing installed package space requirements. 
It was made with low flash memory (8MB) OpenWRT routers in mind. With libffmpeg dependencies cut off, it requires only ~1.3MB of memory (instead of ~3.5MB for full audio/video version).

This patch was generated and tested with minidlna version 1.3.0.

# Using this patch
1. Prepare your OpenWRT toolchain, pull OpenWRT revision matching the one on your device, update feeds, set up your devices architecture using `make menuconfig`.
2. Manually copy `Makefile` and `002-Audio-only-patch.patch` files into minidlna package directory (`feeds/packages/multimedia/minidlna/`)
3. Remove `005-added-support-RMVB.patch` - it breaks patch applying procedure and as it fixes video part of minidlna, we can skip it anyway.
4. Compile the package: `make package/minidlna/compile`
5. Now your ipk file should be generated in bin folder, i.e. `bin/packages/mipsel_24kc/packages/minidlna-audio-only_1.3.0-1_mipsel_24kc.ipk`
6. Copy package into `/tmp` folder on your router, update opkg and install new package: `opkg install /tmp/minidlna-audio-only_1.3.0-1_mipsel_24kc.ipk`.

# Further space reduction
It is possible to reduce space requirements even more by cutting off libvorbis/libogg dependencies (thus lack of reading Ogg tags properly) as they need ~400kB. Use `002-Audio-only-patch-no-vorbis.patch` in this case and remember to remove from Makefile `+libvorbis` dependency.
