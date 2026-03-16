# Milk-V Duo S AIC8800 Wifi/BT Driver

This code was extracted from [Milk-V's Duo Buildroot SDK](https://github.com/milkv-duo/duo-buildroot-sdk-v2), cleaned up to work on Linux 7.0, and modified to work without Milk-V's other kernel hacks. 

## Warning

This is intended specifically for Linux 7.0. While I did gate all my changes with kernel version macros, I set the cutoff at 7.0/6.19 for everything, though it's entirely possible these changes are necessary for earlier versions (probably ~6.17?). Thus, for kernel versions > 5.10 && < 7.0, this is unlikely to work correctly.

## Firmware

Since I do not know the license of the firmware binaries, I'm not re-hosting them here, but they are available from the [milkv-duo/duo-buildroot-sdk-v2 repo](https://github.com/milkv-duo/duo-buildroot-sdk-v2/tree/main/device/generic/rootfs_overlay/duos/mnt/system/firmware/aic8800) (that's a direct link to the files). They need to be placed in `/lib/firmware/aic8800/`.

## Disclosure

I am a professional developer, but I have very little experience with C, and none with hacking the kernel. An LLM was responsible for most of the actual code-writing involved here, though following my instructions, and verified and tested by me. Still, between my inexperience and it being an LLM, it's quite possible there's major issues with this implementation. YMMV.

FWIW, the changes are very small overall - almost all just renaming functions to their modern versions, passing in an extra `NULL` or `0` here or there (because modern cfg80211 calls support multi-link devices, but we only have one). The only "new" code is `cvi_sdio_rescan`, the main logic of which is simply inlined from the vendor's custom SDIO driver, and the LLM-generated portion is the device lookup code. It seems pretty standard to me, it's a pattern used in other kernel drivers (where else would the LLM have found it?).
