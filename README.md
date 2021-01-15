# tegra

## Pinout details

https://developer.nvidia.com/embedded/learn/jetson-nano-2gb-devkit-user-guide

## Custom build
https://docs.nvidia.com/jetson/l4t/index.html#page/Tegra%20Linux%20Driver%20Package%20Development%20Guide/adaptation_and_bringup_nano.html
https://docs.nvidia.com/jetson/l4t/index.html#page/Tegra%20Linux%20Driver%20Package%20Development%20Guide/kernel_custom.html

## Downloads
https://developer.nvidia.com/embedded/downloads

## generic repo
https://nv-tegra.nvidia.com/gitweb/

## Build kernel

### kernel repo
Preferred way to get the source:
Get the SDK Manager from https://developer.nvidia.com/nvidia-sdk-manager 
```
#~/nvidia/nvidia_sdk/JetPack_4.4.1_Linux_JETSON_NANO_2GB_DEVKIT/Linux_for_Tegra/source_sync.sh
```
Use tag `tegra-l4t-r32.4.4` upon source sync.

Other ways to get the source.
https://nv-tegra.nvidia.com/gitweb/?p=linux-4.9.git;a=summary


```
git clone git://nv-tegra.nvidia.com/linux-4.9.git
```
### install kernel
Follow https://docs.nvidia.com/jetson/l4t/index.html#page/Tegra%2520Linux%2520Driver%2520Package%2520Development%2520Guide%2Fkernel_custom.html%23wwpID0E03D0HA
```
export CROSS_COMPILE=~/jetson/workspace/gcc-linaro-7.5.0-2019.12-i686_aarch64-linux-gnu/bin/aarch64-linux-gnu-
make ARCH=arm64 O=TEGRA_KERNEL_OUT tegra_defconfig
make ARCH=arm64 O=TEGRA_KERNEL_OUT menuconfig
// enable drivers->Android
make ARCH=arm64 O=TEGRA_KERNEL_OUT -j8
```
Now start installing kernel, dtb, and heards in right place

```
cd Linux_for_Tegra/kernel
cp -rvf ../sources/kernel/kernel-4.9/arch/arm64/boot/dts/*.dtb* ./dtb/
cp ../sources/kernel/kernel-4.9/arch/arm64/boot/Image ./
sudo make ARCH=arm64 modules_install INSTALL_MOD_PATH=~/jetson/workspace/JetPack_4.4.1_Linux_                                        JETSON_NANO_2GB_DEVKIT-custom/Linux_for_Tegra/rootfs-custom/
cd ../../../rootfs-custom/
sudo tar --owner root --group root -cjf ./kernel_supplements.tbz2 lib/modules
cd Linux_for_Tegra/rootfs/usr/src/
tar -zcvf kernel-4.9.tar.gz kernel-4.9
mv kernel-4.9.tar.gz ../../../../
```

### Now build the mmc flash image
```
cd Linux_for_Tegra
sudo ./tools/jetson-disk-image-creator.sh -o sd-blob.img -b jetson-nano-2gb-devkit -r 300
```
This will create ready to flash `sd-blob.img`.
```
dd if=sd-blob.img of=/dev/mmcblk0 bs=4M status=progress
```

## uboot repo


## Boot flow
```
[0000.125] [L4T TegraBoot] (version 00.00.2018.01-l4t-80a468da)
[0000.131] Processing in cold boot mode Bootloader 2
..
[0000.269] Read GPT from (4:0)
..
[0000.327] Using GPT Primary to query partitions
[0000.332] Loading Tboot-CPU binary
..
[0000.352] Bootloader load address is 0xa0000000, entry address is 0xa0000258
[0000.359] Bootloader downloaded successfully.
[0000.363] Downloaded Tboot-CPU binary to 0xa0000258
..
[0000.386] Read GPT from (4:0)
..
[0000.398] Loading NvTbootBootloaderDTB
..
[0000.520] Loading NvTbootKernelDTB
..
[0000.641] Loading cboot binary
..
[0000.712] Bootloader load address is 0x92c00000, entry address is 0x92c00258
..
[0000.768] BoardId: 3448
..
[0000.782] Read GPT from (4:0)
[0000.790] Using GPT Primary to query partitions
[0000.818] Verifying SC7EntryFw in OdmNonSecureSBK mode
..
[0000.879] SC7EntryFw header found loaded at 0xff700000
..
[0001.073] Bpmp FW successfully loaded
..
[0001.433] Bootloader DTB loaded at 0x83000000
..
[0001.495] Welcome to L4T Cboot
[0001.498] 
[0001.499] Cboot Version: 00.00.2018.01-t210-4686ce50
..
[0001.777] board ID = D78, board SKU = 3
[0001.780] Skipping Z3!
..
[0007.241] kfs_getparname: name = LNX
[0007.244] Loading kernel from LNX
[0007.343] load kernel from storage
```
