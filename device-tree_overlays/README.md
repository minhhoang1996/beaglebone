Environment:  
- Uboot tag: v2020.04(tested)
- Enable u-boot configuration:  
CONFIG_OF_LIBFDT_OVERLAY=y  

- Compile the device tree overlay:  
dtc -@ -I dts -O dtb -o led_node_overlay.dtb led_node_overlay.dts  

- Copy file led_node_overlay.dtb into the boot partition(mmcblk0p1) of SDcard  
- Run these commands on u-boot command line:  
=> setenv fdtovaddr 0x880c0000
=> load mmc 0:1 ${fdtovaddr} led_node_overlay.dtb
=> load mmc 0:1 ${fdtaddr} am335x-boneblack.dtb
=> fdt addr ${fdtaddr}
=> fdt resize 8192
=> fdt apply ${fdtovaddr}

=> setenv bootsettings 'setenv bootargs console=ttyO0,115200n8 root=/dev/mmcblk0p2 rw rootfstype=ext4 rootwait earlyprintk mem=512M'
=> run bootsettings
=> load mmc 0:1 ${loadaddr} uImage
=> bootm ${loadaddr} - ${fdtaddr}

Reference: 
- How to use device tree overlay:  
{source code uboot}/doc/README.fdt-overlays
- How to write device tree overlay:  
https://www.digi.com/resources/examples-guides/use-device-tree-overlays-to-patch-your-device-tree