# Mount IMG

```


# fdisk -l Sync4Dump64.img

Disk Sync4Dump64.img: 58.24 GiB, 62537072640 bytes, 122142720 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 00000000-0000-0000-0000-000000000000

Device            Start       End   Sectors  Size Type
Sync4Dump64.img1     64      2111      2048    1M FreeBSD boot
Sync4Dump64.img2   8192     10239      2048    1M FreeBSD boot
Sync4Dump64.img3  16384     24575      8192    4M QNX6 file system
Sync4Dump64.img4  24576     32767      8192    4M Linux filesystem
Sync4Dump64.img5  32768     81919     49152   24M QNX6 file system
Sync4Dump64.img6  81920    606207    524288  256M QNX6 file system
Sync4Dump64.img7 606208    671743     65536   32M QNX6 file system
Sync4Dump64.img8 671744    737279     65536   32M QNX6 file system
Sync4Dump64.img9 737280 122142711 121405432 57.9G QNX6 file system


```


### losetup

```

sudo losetup -f -P Sync4Dump64.img

sudo losetup -l


sudo gdisk -l /dev/loop3


Partition table holds up to 9 entries
Main partition table begins at sector 2 and ends at sector 4
First usable sector is 8, last usable sector is 122142711
Partitions will be aligned on 64-sector boundaries
Total free space is 12280 sectors (6.0 MiB)

Number  Start (sector)    End (sector)  Size       Code  Name
   1              64            2111   1024.0 KiB  A501  u-boot
   2            8192           10239   1024.0 KiB  A501  u-boot_bak
   3           16384           24575   4.0 MiB     B300  dps_mfg
   4           24576           32767   4.0 MiB     8300  boot_fs
   5           32768           81919   24.0 MiB    B300  dps_os
   6           81920          606207   256.0 MiB   B300  ifs_recovery
   7          606208          671743   32.0 MiB    B300  ifs_a
   8          671744          737279   32.0 MiB    B300  ifs_b
   9          737280       122142711   57.9 GiB    B300  storage

sudo fdisk -x /dev/loop3


Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

Device        Start       End   Sectors  Size Type
/dev/loop3p1     64      2111      2048    1M FreeBSD boot                      # u-boot
/dev/loop3p2   8192     10239      2048    1M FreeBSD boot                      # u-boot_bak
/dev/loop3p3  16384     24575      8192    4M QNX6 file system                  # dps_mfg keymgr-store -> sudo mount -t qnx6 /dev/loop3p3 /tmp/part3/ 
/dev/loop3p4  24576     32767      8192    4M Linux filesystem                  # boot_fs (contain factory bootlogs)
/dev/loop3p5  32768     81919     49152   24M QNX6 file system                  # dps_os (can be mounted as qnx6fs (see p5 tree below))
/dev/loop3p6  81920    606207    524288  256M QNX6 file system                  # ifs_recovery (can be unpacked with dumpifs)
/dev/loop3p7 606208    671743     65536   32M QNX6 file system                  # ifs_a (can be unpacked with dumpifs)
/dev/loop3p8 671744    737279     65536   32M QNX6 file system                  # ifs_b (can be unpacked with dumpifs)
/dev/loop3p9 737280 122142711 121405432 57.9G QNX6 file system                  # storage can be mounted as qnx6fs. Also contain squashfs os_image


Device        Start       End   Sectors Type-UUID                            UUID                                 Name         Attrs
/dev/loop3p1     64      2111      2048 83BD6B9D-7F41-11DC-BE0B-001560B84F0F 00000000-0000-0000-0000-000000000000 u-boot
/dev/loop3p2   8192     10239      2048 83BD6B9D-7F41-11DC-BE0B-001560B84F0F 00000000-0000-0000-0000-000000000001 u-boot_bak
/dev/loop3p3  16384     24575      8192 CEF5A9AD-73BC-4601-89F3-CDEEEEE321A1 00000000-0000-0000-0000-000000000002 dps_mfg
/dev/loop3p4  24576     32767      8192 0FC63DAF-8483-4772-8E79-3D69D8477DE4 00000000-0000-0000-0000-000000000003 boot_fs
/dev/loop3p5  32768     81919     49152 CEF5A9AD-73BC-4601-89F3-CDEEEEE321A1 00000000-0000-0000-0000-000000000004 dps_os
/dev/loop3p6  81920    606207    524288 CEF5A9AD-73BC-4601-89F3-CDEEEEE321A1 00000000-0000-0000-0000-000000000005 ifs_recovery
/dev/loop3p7 606208    671743     65536 CEF5A9AD-73BC-4601-89F3-CDEEEEE321A1 00000000-0000-0000-0000-000000000006 ifs_a
/dev/loop3p8 671744    737279     65536 CEF5A9AD-73BC-4601-89F3-CDEEEEE321A1 00000000-0000-0000-0000-000000000007 ifs_b
/dev/loop3p9 737280 122142711 121405432 CEF5A9AD-73BC-4601-89F3-CDEEEEE321A1 00000000-0000-0000-0000-000000000008 storage


// .......


sudo dd if=/dev/loop3p1 of=./64sync64Dump/Partition1.raw
// ...
sudo dd if=/dev/loop3p8 of=./64sync64Dump/Partition8.raw


sudo mount -t qnx6 /dev/loop3p5 /tmp/part5/
sudo tar cvpzf /mnt/Z9/4Sync4/64sync64Dump/part5.tar.gz /tmp/part5/



sudo mount -t qnx6 /dev/loop3p3 /tmp/mnt/




sudo mount -t qnx6 /dev/loop0p9 /tmp/p9
```



# Mount parts 

sudo mount -t qnx6 /dev/loop0p3 ./32SyncDump/p1/

cat /proc/filesystems

sudo fdisk -l /dev/loop0


sudo mount -o ro ./Partition5.raw /tmp/1


# Part 1 


https://rk3128-cfw.github.io/02_stock_firmware/3_extracting_ofw/

https://developer-archives.toradex.com/software-resources/arm-family/linux/features/uboot#tab-imx-8-family-based-modules



```shell

sudo dd if=/dev/loop3p1 of=./64sync64Dump/Partition1.raw

cd ./64sync64Dump/
binwalk Partition1.raw


# DECIMAL       HEXADECIMAL     DESCRIPTION
# --------------------------------------------------------------------------------
# 762696        0xBA348         SHA256 hash constants, little endian
# 765736        0xBAF28         CRC32 polynomial table, little endian
# 783985        0xBF671         Android bootimg, kernel size: 1684947200 bytes, kernel addr: 0x64696F72, ramdisk size: 1763734311 bytes, ramdisk addr: 0x6567616D, product name: "ddr 0x%08x size %u KiB"
# 869312        0xD43C0         LZ4 compressed data
# 975792        0xEE3B0         Flattened device tree, size: 12316 bytes, version: 17


dd if=Partition1.raw of=Partition1.dtb bs=1 skip=975792

dtc -s -I dtb Partition1.dtb  -O dts -o Partition1o.dtb


# Partition1o.dtb: Warning (unit_address_vs_reg): /imx8qm-pm/lsio_power_domain: node has a reg or ranges property, but no unit name
# Partition1o.dtb: Warning (unit_address_vs_reg): /imx8qm-pm/lsio_power_domain/lsio_gpio0: node has a reg or ranges property, but no unit name
# Partition1o.dtb: Warning (unit_address_vs_reg): /imx8qm-pm/lsio_power_domain/lsio_gpio1: node has a reg or ranges property, but no unit name
# Partition1o.dtb: Warning (unit_address_vs_reg): /imx8qm-pm/lsio_power_domain/lsio_gpio2: node has a reg or ranges property, but no unit name
# Partition1o.dtb: Warning (unit_address_vs_reg): /imx8qm-pm/lsio_power_domain/lsio_gpio3: node has a reg or ranges property, but no unit name
# Partition1o.dtb: Warning (unit_address_vs_reg): /imx8qm-pm/lsio_power_domain/lsio_gpio4: node has a reg or ranges property, but no unit name
# Partition1o.dtb: Warning (unit_address_vs_reg): /imx8qm-pm/lsio_power_domain/lsio_gpio5: node has a reg or ranges property, but no unit name
# Partition1o.dtb: Warning (unit_address_vs_reg): /imx8qm-pm/connectivity_power_domain: node has a reg or ranges property, but no unit name
# Partition1o.dtb: Warning (unit_address_vs_reg): /imx8qm-pm/connectivity_power_domain/conn_usb0: node has a reg or ranges property, but no unit name
# Partition1o.dtb: Warning (unit_address_vs_reg): /imx8qm-pm/connectivity_power_domain/conn_usb0_phy: node has a reg or ranges property, but no unit name
# Partition1o.dtb: Warning (unit_address_vs_reg): /imx8qm-pm/connectivity_power_domain/conn_sdhc0: node has a reg or ranges property, but no unit name
# Partition1o.dtb: Warning (unit_address_vs_reg): /imx8qm-pm/dma_power_domain: node has a reg or ranges property, but no unit name
# Partition1o.dtb: Warning (unit_address_vs_reg): /imx8qm-pm/dma_power_domain/dma_lpuart0: node has a reg or ranges property, but no unit name
# Partition1o.dtb: Warning (unit_address_vs_reg): /imx8qm-pm/dma_power_domain/dma_lpuart2: node has a reg or ranges property, but no unit name
# Partition1o.dtb: Warning (unit_address_format): /usbphy@0x5b100000: unit name should not have leading "0x"
# Partition1o.dtb: Warning (simple_bus_reg): /imx8qm-pm/lsio_power_domain: simple-bus unit address format error, expected "fff0"
# Partition1o.dtb: Warning (simple_bus_reg): /imx8qm-pm/connectivity_power_domain: simple-bus unit address format error, expected "fff0"
# Partition1o.dtb: Warning (simple_bus_reg): /imx8qm-pm/dma_power_domain: simple-bus unit address format error, expected "fff0"
# Partition1o.dtb: Warning (interrupt_provider): /interrupt-controller@51a00000: Missing #address-cells in interrupt provider
# Partition1o.dtb: Warning (interrupt_provider): /gpio@5d080000: Missing #address-cells in interrupt provider
# Partition1o.dtb: Warning (interrupt_provider): /gpio@5d090000: Missing #address-cells in interrupt provider
# Partition1o.dtb: Warning (interrupt_provider): /gpio@5d0a0000: Missing #address-cells in interrupt provider
# Partition1o.dtb: Warning (interrupt_provider): /gpio@5d0b0000: Missing #address-cells in interrupt provider
# Partition1o.dtb: Warning (interrupt_provider): /gpio@5d0c0000: Missing #address-cells in interrupt provider
# Partition1o.dtb: Warning (interrupt_provider): /gpio@5d0d0000: Missing #address-cells in interrupt provider


# ! 
# ! SEE in repo 

cat Partition1o.dtb.txt

# ! SEE 
# ! 



dd if=Partition1.raw of=Partition1boot.dtb bs=1 count=12381 skip=975792
12381+0 records in
12381+0 records out
12381 bytes (12 kB, 12 KiB) copied, 0.67677 s, 18.3 kB/s


dtc -s -I dtb Partition1boot.dtb -O dts -o Partition1boot.dts

#Partition1boot.dts: Warning (unit_address_vs_reg): /imx8qm-pm/lsio_power_domain: node has a reg or ranges property, but no unit name
#Partition1boot.dts: Warning (unit_address_vs_reg): /imx8qm-pm/lsio_power_domain/lsio_gpio0: node has a reg or ranges property, but no unit name
#Partition1boot.dts: Warning (unit_address_vs_reg): /imx8qm-pm/lsio_power_domain/lsio_gpio1: node has a reg or ranges property, but no unit name
#Partition1boot.dts: Warning (unit_address_vs_reg): /imx8qm-pm/lsio_power_domain/lsio_gpio2: node has a reg or ranges property, but no unit name
#Partition1boot.dts: Warning (unit_address_vs_reg): /imx8qm-pm/lsio_power_domain/lsio_gpio3: node has a reg or ranges property, but no unit name
#Partition1boot.dts: Warning (unit_address_vs_reg): /imx8qm-pm/lsio_power_domain/lsio_gpio4: node has a reg or ranges property, but no unit name
#Partition1boot.dts: Warning (unit_address_vs_reg): /imx8qm-pm/lsio_power_domain/lsio_gpio5: node has a reg or ranges property, but no unit name
#Partition1boot.dts: Warning (unit_address_vs_reg): /imx8qm-pm/connectivity_power_domain: node has a reg or ranges property, but no unit name
#Partition1boot.dts: Warning (unit_address_vs_reg): /imx8qm-pm/connectivity_power_domain/conn_usb0: node has a reg or ranges property, but no unit name
#Partition1boot.dts: Warning (unit_address_vs_reg): /imx8qm-pm/connectivity_power_domain/conn_usb0_phy: node has a reg or ranges property, but no unit name
#Partition1boot.dts: Warning (unit_address_vs_reg): /imx8qm-pm/connectivity_power_domain/conn_sdhc0: node has a reg or ranges property, but no unit name
#Partition1boot.dts: Warning (unit_address_vs_reg): /imx8qm-pm/dma_power_domain: node has a reg or ranges property, but no unit name
#Partition1boot.dts: Warning (unit_address_vs_reg): /imx8qm-pm/dma_power_domain/dma_lpuart0: node has a reg or ranges property, but no unit name
#Partition1boot.dts: Warning (unit_address_vs_reg): /imx8qm-pm/dma_power_domain/dma_lpuart2: node has a reg or ranges property, but no unit name
#Partition1boot.dts: Warning (unit_address_format): /usbphy@0x5b100000: unit name should not have leading "0x"
#Partition1boot.dts: Warning (simple_bus_reg): /imx8qm-pm/lsio_power_domain: simple-bus unit address format error, expected "fff0"
#Partition1boot.dts: Warning (simple_bus_reg): /imx8qm-pm/connectivity_power_domain: simple-bus unit address format error, expected "fff0"
#Partition1boot.dts: Warning (simple_bus_reg): /imx8qm-pm/dma_power_domain: simple-bus unit address format error, expected "fff0"
#Partition1boot.dts: Warning (interrupt_provider): /interrupt-controller@51a00000: Missing #address-cells in interrupt provider
#Partition1boot.dts: Warning (interrupt_provider): /gpio@5d080000: Missing #address-cells in interrupt provider
#Partition1boot.dts: Warning (interrupt_provider): /gpio@5d090000: Missing #address-cells in interrupt provider
#Partition1boot.dts: Warning (interrupt_provider): /gpio@5d0a0000: Missing #address-cells in interrupt provider
#Partition1boot.dts: Warning (interrupt_provider): /gpio@5d0b0000: Missing #address-cells in interrupt provider
#Partition1boot.dts: Warning (interrupt_provider): /gpio@5d0c0000: Missing #address-cells in interrupt provider
#Partition1boot.dts: Warning (interrupt_provider): /gpio@5d0d0000: Missing #address-cells in interrupt provider



```

### Part 1 - aboot

```shell

dd if=Partition1.raw of=Partition1aboot.raw bs=1 count=85327 skip=783985
??? no 
```



# Part 3

```

├── DID
│   ├── F111
│   ├── F113
│   └── F124
├── deviceInfo
│   ├── KeyPackageVersion
│   ├── btmac
│   ├── fesn
│   ├── hwi
│   ├── msn
│   ├── spid
│   └── wifiMac
└── keymgr-store
    ├── cert_sync_soa_tls_public.pem
    ├── key_ap_syncp_0.b64
    ├── key_ap_syncp_1.b64
    ├── key_ap_syncp_2.b64
    ├── key_ap_syncp_3.b64
    ├── key_ap_syncp_4.b64
    ├── key_ap_syncp_5.b64
    ├── key_ap_syncp_6.b64
    ├── key_ap_syncp_7.b64
    ├── key_sync_soa_tls_private.pem
    └── tls_tmc_device_private_key.pem



```

# Part 5

```
├── de-blocks
│   ├── dps_audio-tuning.bin
│   ├── dps_de-blocks.bin
│   ├── dps_display-illumination.bin
│   ├── dps_illumination.bin
│   └── dps_storage.bin
├── display  [error opening dir]
├── fdtm
│   ├── ignition_count
│   └── token_update_reason.log
└── token

4 directories, 7 files

```

# Part 6 


```

Decompressed 23661315 bytes-> 86318672 bytes
   Offset     Size  Name
        0      100  Startup-header flags1=0xd flags2=0x1 paddr_bias=0x7ffd00000000
      100    51008  startup.*
    51108       5c  Image-header mountpoint=/
    51164     ed30  Image-directory
     ----     ----  Root-dirent
     ----     ----  fs/os/usr/share/misc
     ----     ----  fs/os/usr/include/io-pkt/netdrvr
     ----     ----  fs/os/usr/include/sys
     ----     ----  fs/os/usr/include/net
     ----     ----  fs/os/usr/include/io-pkt
     ----     ----  fs/os/usr/include/hw
     ----     ----  fs/os/usr/share
     ----     ----  fs/os/usr/include
     ----       11  fs/os/usr/sbin -> ../aarch64le/sbin
     ----       18  fs/os/usr/libexec -> ../aarch64le/usr/libexec
     ----       10  fs/os/usr/lib -> ../aarch64le/lib
     ----       10  fs/os/usr/bin -> ../aarch64le/bin
     ----     ----  fs/os/aarch64le/boot/build
     ----     ----  fs/os/aarch64le/boot/sys
     ----     ----  fs/os/aarch64le/usr/libexec/awk
     ----     ----  fs/os/aarch64le/usr/libexec
     ----     ----  fs/os/aarch64le/lib/terminfo/x
     ----     ----  fs/os/aarch64le/lib/terminfo/v
     ----     ----  fs/os/aarch64le/lib/terminfo/q
     ----     ----  fs/os/aarch64le/lib/terminfo/a
     ----       1a  fs/os/aarch64le/lib/dll/pci/pci_hw-nxp-imx8-cpu.so -> pci_hw-nxp-imx8-cpu.so.2.4
     ----     ----  fs/os/aarch64le/lib/dll/pci
     ----       13  fs/os/aarch64le/lib/libtraceparser.so -> libtraceparser.so.1
     ----       10  fs/os/aarch64le/lib/libtermcap.so -> libncursesw.so.1
     ----       1c  fs/os/aarch64le/lib/libsync_critical_logger.so -> libsync_critical_logger.so.1
     ----        c  fs/os/aarch64le/lib/libstrm.so -> libstrm.so.1
     ----       10  fs/os/aarch64le/lib/libssl1_1.so -> libssl1_1.so.2.1
     ----        c  fs/os/aarch64le/lib/libreadline.so -> libedit.so.0
     ----        c  fs/os/aarch64le/lib/libpcre.so -> libpcre.so.3
     ----        e  fs/os/aarch64le/lib/libpanelw.so -> libpanelw.so.1
     ----        e  fs/os/aarch64le/lib/libpanel.so -> libpanelw.so.1
     ----       14  fs/os/aarch64le/lib/libnxp_imx8_sci.so -> libnxp_imx8_sci.so.1
     ----       10  fs/os/aarch64le/lib/libncursesw.so -> libncursesw.so.1
     ----       10  fs/os/aarch64le/lib/libncurses.so -> libncursesw.so.1
     ----       12  fs/os/aarch64le/lib/libncurses++w.so -> libncurses++w.so.1
     ----       12  fs/os/aarch64le/lib/libncurses++.so -> libncurses++w.so.1
     ----        a  fs/os/aarch64le/lib/libmq.so -> libmq.so.1
     ----        d  fs/os/aarch64le/lib/libmenuw.so -> libmenuw.so.1
     ----        d  fs/os/aarch64le/lib/libmenu.so -> libmenuw.so.1
     ----        c  fs/os/aarch64le/lib/libintl.so -> libintl.so.1
     ----        b  fs/os/aarch64le/lib/libgpt.so -> libgpt.so.1
     ----       10  fs/os/aarch64le/lib/libfsmerkle.so -> libfsmerkle.so.1
     ----        d  fs/os/aarch64le/lib/libformw.so -> libformw.so.1
     ----        d  fs/os/aarch64le/lib/libform.so -> libformw.so.1
     ----        c  fs/os/aarch64le/lib/libedit.so -> libedit.so.0
     ----       13  fs/os/aarch64le/lib/libcrypto1_1.so -> libcrypto1_1.so.2.1
     ----        b  fs/os/aarch64le/lib/libaoi.so -> libaoi.so.1
     ----        9  fs/os/aarch64le/lib/ldqnx-64.so.2 -> libc.so.4
     ----        b  fs/os/aarch64le/lib/libtar.so -> libtar.so.1
     ----       16  fs/os/aarch64le/lib/librpmsg_lite-imx.so -> librpmsg_lite-imx.so.1
     ----       10  fs/os/aarch64le/lib/libtracelog.so -> libtracelog.so.1
     ----        f  fs/os/aarch64le/lib/libelfcore.so -> libelfcore.so.1
     ----       14  fs/os/aarch64le/lib/libpaho-mqtt3cs.so -> libpaho-mqtt3cs.so.1
     ----       1b  fs/os/aarch64le/lib/libNS_FrameworkUnified.so -> libNS_FrameworkUnified.so.1
     ----       13  fs/os/aarch64le/lib/libpaho-mqtt3c.so -> libpaho-mqtt3c.so.1
     ----       14  fs/os/aarch64le/lib/libpaho-mqtt3as.so -> libpaho-mqtt3as.so.1
     ----       13  fs/os/aarch64le/lib/libpaho-mqtt3a.so -> libpaho-mqtt3a.so.1
     ----       18  fs/os/aarch64le/lib/libtcmalloc_minimal.so -> libtcmalloc_minimal.so.9
     ----       16  fs/os/aarch64le/lib/libNS_SharedMemIf.so -> libNS_SharedMemIf.so.1
     ----       17  fs/os/aarch64le/lib/libNS_MessageQueue.so -> libNS_MessageQueue.so.1
     ----       18  fs/os/aarch64le/lib/libNS_MessageCenter.so -> libNS_MessageCenter.so.1
     ----       11  fs/os/aarch64le/lib/libNS_Logger.so -> libNS_Logger.so.1
     ----       15  fs/os/aarch64le/lib/libSS_Region_Lib.so -> libSS_Region_Lib.so.1
     ----       16  fs/os/aarch64le/lib/libVS_CANShadowIf.so -> libVS_CANShadowIf.so.1
     ----        b  fs/os/aarch64le/lib/libpps.so -> libpps.so.1
     ----        c  fs/os/aarch64le/lib/libjson.so -> libjson.so.1
     ----        d  fs/os/aarch64le/lib/libhiddi.so -> libhiddi.so.1
     ----       18  fs/os/aarch64le/lib/libdevice-publisher.so -> libdevice-publisher.so.1
     ----       13  fs/os/aarch64le/lib/libSS_SystemIf.so -> libSS_SystemIf.so.1
     ----        9  fs/os/aarch64le/lib/libc.so -> libc.so.4
     ----        d  fs/os/aarch64le/lib/libutlfd.so -> libutlfd.so.1
     ----        e  fs/os/aarch64le/lib/libsocket.so -> libsocket.so.3
     ----       11  fs/os/aarch64le/lib/libslog2shim.so -> libslog2shim.so.1
     ----       12  fs/os/aarch64le/lib/libslog2parse.so -> libslog2parse.so.1
     ----        d  fs/os/aarch64le/lib/libslog2.so -> libslog2.so.1
     ----       16  fs/os/aarch64le/lib/libNS_NPServiceIf.so -> libNS_NPServiceIf.so.1
     ----        b  fs/os/aarch64le/lib/libcam.so -> libcam.so.2
     ----        c  fs/os/aarch64le/lib/libwipe.so -> libwipe.so.1
     ----       10  fs/os/aarch64le/lib/libfsshared.so -> libfsshared.so.1
     ----        e  fs/os/aarch64le/lib/libmu-mx8.so -> libmu-mx8.so.1
     ----       10  fs/os/aarch64le/lib/libdma-edma.so -> libdma-edma.so.1
     ----        b  fs/os/aarch64le/lib/libusb.so -> libusb.so.1
     ----     ----  fs/os/aarch64le/lib/terminfo
     ----     ----  fs/os/aarch64le/lib/dll
     ----        2  fs/os/aarch64le/bin/waitfor -> on
     ----        2  fs/os/aarch64le/bin/ability -> on
     ----        5  fs/os/aarch64le/bin/view -> elvis
     ----        5  fs/os/aarch64le/bin/vi -> elvis
     ----        3  fs/os/aarch64le/bin/sh -> ksh
     ----        5  fs/os/aarch64le/bin/ex -> elvis
     ----        5  fs/os/aarch64le/bin/chgrp -> chown
     ----        2  fs/os/aarch64le/bin/xzcat -> xz
     ----        3  fs/os/aarch64le/bin/uuencode -> uue
     ----        3  fs/os/aarch64le/bin/uudecode -> uud
     ----        2  fs/os/aarch64le/bin/unxz -> xz
     ----        2  fs/os/aarch64le/bin/unlzma -> xz
     ----        7  fs/os/aarch64le/bin/setconf -> getconf
     ----        2  fs/os/aarch64le/bin/od -> hd
     ----        4  fs/os/aarch64le/bin/awk -> gawk
     ----        4  fs/os/aarch64le/bin/more -> less
     ----        2  fs/os/aarch64le/bin/lzma -> xz
     ----        2  fs/os/aarch64le/bin/lzcat -> xz
     ----        4  fs/os/aarch64le/bin/fgrep -> grep
     ----        4  fs/os/aarch64le/bin/egrep -> grep
     ----        5  fs/os/aarch64le/bin/bzcat -> bzip2
     ----        5  fs/os/aarch64le/bin/bunzip2 -> bzip2
     ----     ----  fs/os/aarch64le/boot
     ----     ----  fs/os/aarch64le/usr
     ----     ----  fs/os/aarch64le/sbin
     ----     ----  fs/os/aarch64le/lib
     ----     ----  fs/os/aarch64le/bin
    5fe94        0  fs/os/var/chroot/sshd/.gitkeep
     ----     ----  fs/os/var/chroot/sshd
     ----     ----  fs/os/var/chroot
     ----     ----  fs/os/root/.ssh
     ----     ----  fs/os/etc/keymgr/keys
     ----     ----  fs/os/etc/soa/gateway/config
     ----     ----  fs/os/etc/soa/gateway
     ----     ----  fs/os/etc/dvfs/config/imx8
     ----     ----  fs/os/etc/dvfs/config
    5fe94        0  fs/os/etc/sysconfig/alm_cabilities.conf
     ----     ----  fs/os/etc/startup/module
     ----     ----  fs/os/etc/startup/scripts
     ----     ----  fs/os/etc/keymgr
     ----     ----  fs/os/etc/stabilitymonitor
     ----     ----  fs/os/etc/soa
     ----     ----  fs/os/etc/skel
     ----     ----  fs/os/etc/default
     ----     ----  fs/os/etc/config
     ----     ----  fs/os/etc/dvfs
     ----     ----  fs/os/etc/sysconfig
     ----     ----  fs/os/etc/ssh
     ----     ----  fs/os/etc/fonts
     ----     ----  fs/os/etc/startup
    5fe94        0  fs/os/.gitkeep
    5fe94        0  fs/os/.stamp_swpart_staging_dircreated
     ----     ----  fs/os/keys
     ----        e  fs/os/sbin -> aarch64le/sbin
     ----        d  fs/os/lib -> aarch64le/lib
     ----        d  fs/os/bin -> aarch64le/bin
     ----     ----  fs/os/usr
     ----     ----  fs/os/aarch64le
     ----     ----  fs/os/recovery
     ----     ----  fs/os/var
     ----     ----  fs/os/root
     ----     ----  fs/os/etc
     ----        f  recovery -> /fs/os/recovery
     ----       12  fs/rwdata -> /fs/storage/rwdata
     ----       23  proc/boot/gpio_mapping.so -> /lib/dll/sync4_imx8_gpio_mapping.so
     ----        4  var/log -> /tmp
     ----        9  dev/console -> /dev/null
     ----        9  dev/debuglog -> /dev/ser3
     ----       14  artifact.json -> /fs/os/artifact.json
     ----       12  fs/images -> /fs/storage/images
     ----        6  fs/mp -> /fs/os
     ----        a  usr -> /fs/os/usr
     ----        a  sql -> /fs/os/sql
     ----       15  sbin -> /fs/os/aarch64le/sbin
     ----        b  root -> /fs/os/root
     ----        a  opt -> /fs/os/opt
     ----        c  menlo -> /fs/os/menlo
     ----       14  lib -> /fs/os/aarch64le/lib
     ----        e  fordhmi -> /fs/os/fordhmi
     ----        a  etc -> /fs/os/etc
     ----        9  db -> /fs/os/db
     ----       14  bin -> /fs/os/aarch64le/bin
     ----       10  aarch64le -> /fs/os/aarch64le
    5fe94      16b  fs/os/etc/startup/scripts/run_with_delay.sh
    60000    c7000  proc/boot/procnto-smp-instr
   127000    13a93  proc/boot/devc-sermx8-dma
   13aa93      56d  fs/os/aarch64le/lib/terminfo/q/qnxtmono
   13b000     6265  proc/boot/libdma-edma.so.1
   141265      d88  fs/os/etc/startup/module/diagnostics.xml
   141fed        b  fs/os/etc/ftpusers
   141ff8        5  fs/os/etc/qversion
   142000     6265  proc/boot/libdma-edma.so
   148265      d64  fs/os/etc/startup/scripts/logCaptureRoutine.sh
   148fc9       2b  fs/os/etc/startup/module/hmi_classic_members.xml
   149000    1ddba  proc/boot/devb-sdmmc-mx8x
   166dba      246  fs/os/etc/startup/module/recovery_prep.xml
   167000    de170  proc/boot/libc++.so.1
   245170      e7f  fs/os/sync4-qxp.script
     ----        b  proc/boot/libc++.so.1.0 -> libc++.so.1
   246000    3a3fd  proc/boot/libm.so.3
   2803fd      b09  fs/os/etc/startup/module/theme_service.xml
   280f06       f6  fs/os/etc/sysconfig/pps_acl
     ----        9  proc/boot/libm.so -> libm.so.3
   281000     557a  proc/boot/mount-os
   28657a      a2a  fs/os/aarch64le/bin/clearPersistedData.sh
   286fa4       56  fs/os/etc/profile_boot_mode
   287000     41a9  proc/boot/libsync_critical_logger.so.1
   28b1a9      e00  fs/os/sync4-qm.script
   28bfa9       4b  fs/os/aarch64le/bin/zcat
     ----       1c  proc/boot/libsync_critical_logger.so -> libsync_critical_logger.so.1
   28c000    a0e84  proc/boot/libc.so.4
   32ce84      17c  fs/os/etc/startup/module/bptservice.xml
     ----        9  proc/boot/libc.so -> libc.so.4
   32d000    19261  proc/boot/libz.so.2
   346261      d51  fs/os/etc/profile
   346fb2       4a  fs/os/aarch64le/bin/gunzip
     ----        9  proc/boot/libz.so -> libz.so.2
   347000    5a505  proc/boot/charset.so
   3a1505      af4  fs/os/etc/dashcard_system_apps.json
   3a2000    392fb  proc/boot/fs-qnx6.so
   3db2fb      cc6  fs/os/aarch64le/lib/terminfo/x/xterm
   3dbfc1       3e  proc/boot/graphics_root
   3dc000    4edc1  proc/boot/io-blk.so
   42adc1      230  fs/os/aarch64le/lib/terminfo/q/qvt102
   42b000    2aaae  proc/boot/libfsshared.so.1
   455aae      549  fs/os/etc/sysconfig/alm_permissions.conf
     ----       10  proc/boot/libfsshared.so -> libfsshared.so.1
   456000    1c196  proc/boot/libcam.so.2
   472196      c66  fs/os/sync42-qm.script
   472dfc      200  fs/os/etc/mbr-ramdisk-stock.img
     ----        b  proc/boot/libcam.so -> libcam.so.2
   473000     5976  proc/boot/cam-disk.so
   478976      677  fs/os/etc/startup/module/vehicle_config.xml
   479000     569b  proc/boot/devb-ram
   47e69b      954  fs/os/etc/keymgr/keys/dev_cert_ivsu_root.pem
   47f000     d900  proc/boot/fsevmgr
   48c900      700  proc/boot/pci_hw-imx8-external.cfg
   48d000     c736  proc/boot/random
   499736      8a8  fs/os/etc/startup/module/storage.xml
   499fde       21  fs/os/aarch64le/sbin/shutdown
   49a000     428d  proc/boot/libsecpol.so.1
   49e28d      c5c  fs/os/sync42-qxp.script
   49eee9      101  fs/os/etc/startup/module/dsp.dtd
     ----        e  proc/boot/libsecpol.so -> libsecpol.so.1
   49f000     5994  proc/boot/mount
   4a4994      66c  fs/os/etc/sysconfig/skeletor_recovery_prep.json
   4a5000    3e3a1  proc/boot/ksh
   4e33a1      c11  fs/os/aarch64le/bin/igawk
   4e3fb2       4a  fs/os/aarch64le/bin/uncompress
   4e4000     33dc  proc/boot/cat
   4e73dc      a20  fs/os/etc/startup/scripts/sirius_upgrade.sh
   4e7dfc      200  fs/os/etc/startup/module/wlan_driver.xml
   4e8000     755c  proc/boot/pipe
   4ef55c      9f2  fs/os/etc/startup/module/software_update.xml
   4eff4e       b1  fs/os/etc/nsswitch.conf
   4f0000     c879  proc/boot/login
   4fc879      76e  fs/os/etc/startup/scripts/mount_test_pkg.sh
   4fcfe7       18  fs/os/etc/sysconfig/alm_abilities.conf
   4fd000     94db  proc/boot/waitfor
   5064db      9ef  fs/os/etc/startup/module/applink.xml
   506eca      12a  fs/os/etc/startup/module/cluster.xml
   507000     94db  proc/boot/on
   5104db      9ee  fs/os/etc/startup/module/applink_Applink.xml
   510ec9      118  fs/os/etc/startup/module/recovery.xml
   511000   140546  proc/boot/squashfs
   651546      9db  fs/os/etc/startup/module/vehicle_network.xml
   651f21       d2  fs/os/etc/startup/module/alm.dtd
   652000    aa9fa  proc/boot/libssl1_1.so.2.1
   6fc9fa      5f5  fs/os/aarch64le/lib/terminfo/v/vs100
     ----       10  proc/boot/libssl1_1.so -> libssl1_1.so.2.1
   6fd000   2ec2d2  proc/boot/libcrypto1_1.so.2.1
   9e92d2      93f  fs/os/etc/keymgr/keys/cert_ivsu_root.pem
   9e9c11      3ee  fs/os/etc/default/passwd
     ----       13  proc/boot/libcrypto1_1.so -> libcrypto1_1.so.2.1
   9ea000    375da  proc/boot/libsocket.so.3
   a215da      928  fs/os/etc/keymgr/keys/dev_cert_ota_tls_ca.pem
   a21f02       f2  fs/os/recovery/reboot.sh
     ----        e  proc/boot/libsocket.so -> libsocket.so.3
   a22000    1189c  proc/boot/fs-qtd.so
   a3389c      764  fs/os/etc/startup/module/main_recovery.xml
   a34000    1fb7f  proc/boot/dumper
   a53b7f      453  fs/os/etc/startup/module/display.xml
   a53fd2       2b  fs/os/etc/default/login
   a54000    12799  proc/boot/slogger2
   a66799      84c  fs/os/aarch64le/lib/terminfo/q/qansi-w
   a67000     82a6  proc/boot/libslog2.so.1
   a6f2a6      924  fs/os/etc/keymgr/keys/cert_ota_tls_ca.pem
   a6fbca      436  fs/os/etc/startup/scripts/start_serial-qxp-4.0.sh
     ----        d  proc/boot/libslog2.so -> libslog2.so.1
   a70000    102c4  proc/boot/libnxp_imx8_sci.so.1
   a802c4      921  fs/os/etc/startup/module/fnv2_logging.xml
   a80be5      3f6  fs/os/etc/startup/module/apple_auth.xml
   a80fdb       25  fs/os/etc/networks
     ----       14  proc/boot/libnxp_imx8_sci.so -> libnxp_imx8_sci.so.1
   a81000     b1b7  proc/boot/sc-imx8
   a8c1b7      902  fs/os/etc/startup/scripts/fdp_prereset_logs.sh
   a8cab9      544  fs/os/etc/startup/module/usb.xml
   a8d000     6b8b  proc/boot/hwinfo-resmgr
   a93b8b      447  fs/os/etc/startup/module/audio_driver_no_a2b.xml
   a93fd2       28  fs/os/etc/Version.inf
   a94000     32fe  proc/boot/timestamp
   a972fe      8e9  fs/os/etc/startup/module/voice.xml
   a97be7      3f1  fs/os/etc/startup/module/apple_auth_Applink.xml
   a97fd8       23  fs/os/etc/shadow
   a98000    1721d  proc/boot/sync4-hwinfo-cfg.so
   aaf21d      8cf  fs/os/aarch64le/lib/bclib.b
   aafaec      500  fs/os/aarch64le/lib/terminfo/q/qnxt2
     ----       13  proc/boot/hwinfo-cfg.so -> sync4-hwinfo-cfg.so
   ab0000     6844  proc/boot/devb-loopback
   ab6844      7b7  fs/os/aarch64le/lib/terminfo/q/qansi-t
   ab7000    2b1c8  proc/boot/libtcmalloc_minimal.so.9
   ae21c8      86f  fs/os/etc/startup/module/sirius.xml
   ae2a37      5c9  fs/os/aarch64le/lib/terminfo/a/ansi
     ----       18  proc/boot/libtcmalloc_minimal.so -> libtcmalloc_minimal.so.9
   ae3000     4040  fs/os/aarch64le/bin/dvfs_client
   ae7040      fb0  fs/os/etc/keymgr/keys/dev_cert_ecg_soa_tls_ca.pem
   ae8000     2040  fs/os/aarch64le/bin/true
   aea040      86e  fs/os/etc/startup/module/sirius_profile.xml
   aea8ae      72e  fs/os/etc/keymgr/keys/dev_cert_sync_ota_signing_sw.pem
   aeb000    14ebc  fs/os/aarch64le/bin/diskimage
   affebc      144  fs/os/etc/startup/module/commonapi.xml
   b00000     3040  fs/os/aarch64le/bin/genmac-random
   b03040      86d  fs/os/etc/startup/module/multimedia.xml
   b038ad      72a  proc/boot/keys/dev_cert_sync_app_signing.pem
   b04000     9040  fs/os/aarch64le/bin/mmc_util
   b0d040      86c  fs/os/etc/startup/module/multimedia_Applink.xml
   b0d8ac      72a  fs/os/keys/dev_cert_sync_app_signing.pem
   b0e000     4050  fs/os/aarch64le/bin/wdtkick-sc-imx8
   b12050      86c  fs/os/etc/startup/module/multimedia_M.xml
   b128bc      702  proc/boot/start_pcie.sh
   b13000     c110  fs/os/aarch64le/bin/dvfsmgr-imx8x
   b1f110      84a  fs/os/aarch64le/lib/terminfo/q/qansi-m
   b1f95a      68f  fs/os/etc/ssh/ssh_host_rsa_key
   b20000     8044  fs/os/aarch64le/bin/dev-can-mx7x-mx8x
   b28044      81b  fs/os/etc/startup/module/vmcu-4.0.xml
   b2885f      79b  fs/os/aarch64le/lib/terminfo/q/qansi-g
   b29000     8060  fs/os/aarch64le/bin/mkmerklefs
   b31060      808  fs/os/etc/startup/module/alm.xml
   b31868      76a  proc/boot/start_i2c.sh
   b32000     7050  fs/os/aarch64le/bin/slog2info
   b39050      801  fs/os/etc/startup/module/media_support.xml
   b39851      7ad  fs/os/etc/Sync4Ev2.conf
   b3a000    113b0  fs/os/aarch64le/bin/slogger2
   b4b3b0      7c4  fs/os/etc/startup/scripts/slog_log_manager.sh
   b4bb74      433  fs/os/etc/startup/module/welcome.xml
   b4bfa7       44  fs/os/etc/resolv.conf
   b4c000     4040  fs/os/aarch64le/bin/uesh
   b50040      7b1  fs/os/aarch64le/lib/terminfo/q/qansi
   b507f1      76a  fs/os/etc/startup/scripts/start_i2c-qm-4.0.sh
   b50f5b       90  fs/os/etc/startup/scripts/mount_and_format_ramdisk.sh
   b51000    11318  fs/os/aarch64le/bin/elfnote
   b62318      75f  fs/os/etc/pf_sync.conf
   b62a77      563  fs/os/etc/startup/module/os.xml
   b63000     3040  fs/os/aarch64le/bin/bmetricsclient
   b66040      702  fs/os/etc/startup/scripts/start_pcie-qm-4.0.sh
   b66742      700  fs/os/etc/pci_hw-imx8-external.cfg
   b66e42      1b4  fs/os/etc/sysconfig/alm_groups.conf
   b67000     3040  fs/os/aarch64le/bin/umount
   b6a040      6fe  proc/boot/pci_hw-imx8-internal.cfg
   b6a73e      6fe  fs/os/etc/pci_hw-imx8-internal.cfg
   b6ae3c      1c4  fs/os/sync42-qm.bootstrap
   b6b000     4058  fs/os/aarch64le/bin/join
   b6f058      6d1  fs/os/etc/startup/module/hmi_classic_components.xml
   b6f729      6cc  proc/boot/.script
   b6fdf5      1f8  fs/os/etc/startup/module/connectivity.xml
   b70000     504c  fs/os/aarch64le/bin/bmetricsserver
   b7504c      6c3  fs/os/etc/startup/module/vmcu-4.2.xml
   b7570f      6af  fs/os/etc/sysconfig/skeletor_recovery_setup_dirs.json
   b75dbe      224  fs/os/etc/startup/scripts/app_reset_prio.sh
   b76000     3040  fs/os/aarch64le/bin/de-inspect
   b79040      676  fs/os/etc/startup/module/vehicle_config-4.2.xml
   b796b6      663  fs/os/etc/protocols
   b79d19      2e5  fs/os/etc/startup/module/text_input.xml
   b7a000     3040  fs/os/aarch64le/bin/timestamp
   b7d040      658  fs/os/etc/startup/module/usb.dtd
   b7d698      5f5  fs/os/aarch64le/lib/terminfo/x/xterms
   b7dc8d      36c  fs/os/etc/startup/module/dvfs_services.dtd
   b7e000    65aa8  fs/os/aarch64le/bin/SS_PowerManager
   be3aa8      555  fs/os/aarch64le/lib/terminfo/q/qnx
   be4000    1b2c8  fs/os/aarch64le/bin/sed
   bff2c8      5e6  fs/os/etc/startup/module/audio.xml
   bff8ae      5c4  fs/os/etc/startup/module/dvfs_services.xml
   bffe72      186  fs/os/etc/startup/module/clock.xml
   c00000    850b0  fs/os/aarch64le/bin/deviceabstraction_unittest
   c850b0      5c2  fs/os/aarch64le/lib/terminfo/q/qnxm
   c85672      5c1  fs/os/etc/startup/module/debug.xml
   c85c33      3c4  proc/boot/start_serial.sh
   c86000     c058  fs/os/aarch64le/bin/split
   c92058      5b8  fs/os/aarch64le/lib/terminfo/q/qnxw
   c92610      5b7  fs/os/etc/startup/module/system_monitor.xml
   c92bc7      428  fs/os/etc/startup/scripts/start_serial-qxp-4.2.sh
   c93000     2040  fs/os/aarch64le/bin/env
   c95040      5b5  fs/os/aarch64le/lib/nto.link
   c955f5      5ae  fs/os/etc/startup/module/boot_complete.xml
   c95ba3      3f1  fs/os/etc/startup/module/apple_auth_profile.xml
   c95f94       6a  fs/os/etc/chromium-standby-hmi-descriptor.json
   c96000    16068  fs/os/aarch64le/bin/soaadapter_basic_test
   cac068      560  fs/os/etc/stabilitymonitor/startup_sync.sh
   cac5c8      559  fs/os/aarch64le/lib/terminfo/q/qnxt
   cacb21      4dc  fs/os/etc/startup/module/sirius_dl_launch.xml
   cad000     2040  fs/os/aarch64le/bin/sync
   caf040      559  fs/os/aarch64le/lib/terminfo/q/qnxt4
   caf599      555  fs/os/aarch64le/lib/terminfo/q/qnx4
   cafaee      4f7  fs/os/etc/startup/module/soa.xml
   cb0000    2de50  fs/os/aarch64le/bin/less
   cdde50      196  fs/os/etc/startup/module/vpu.xml
   cde000    11068  fs/os/aarch64le/bin/soaadapter_publisher_test
   cef068      538  fs/os/etc/startup/module/hmi_dashcard.dtd
   cef5a0      4f5  proc/boot/start_console.sh
   cefa95      4f5  fs/os/etc/startup/scripts/start_console.sh
   ceff8a       6e  fs/os/etc/sysconfig/cpmd.conf
   cf0000     6290  fs/os/aarch64le/bin/slay
   cf6290      4eb  fs/os/etc/fonts/fonts.conf
   cf677b      4e3  fs/os/etc/startup/module/boot_complete-4.2.xml
   cf6c5e      385  fs/os/etc/startup/module/fleetmanagement.xml
   cf7000     2040  fs/os/aarch64le/bin/errno
   cf9040      4d7  fs/os/etc/startup/module/audio_driver_a2b.xml
   cf9517      4d4  fs/os/etc/startup/module/security.xml
   cf99eb      4aa  fs/os/aarch64le/lib/terminfo/v/vt100
   cf9e95      164  fs/os/etc/startup/module/recovery_setup.xml
   cfa000    14078  fs/os/aarch64le/bin/soaadapter_soak_test
   d0e078      4aa  fs/os/aarch64le/lib/terminfo/v/vt100-am
   d0e522      4a4  fs/os/aarch64le/lib/terminfo/v/vt102
   d0e9c6      49f  fs/os/etc/dvfs/config/imx8/dvfs_pwrtbl_imx8qm.conf
   d0ee65      192  fs/os/etc/ssh/ssh_host_rsa_key.pub
   d0f000     4040  fs/os/aarch64le/bin/script
   d13040      49f  fs/os/etc/dvfs/config/imx8/dvfs_sync42_pwrtbl_imx8qm.conf
   d134df      48d  fs/os/aarch64le/lib/terminfo/q/qnx2
   d1396c      48d  fs/os/aarch64le/lib/terminfo/q/qnxs2
   d13df9      1f4  fs/os/etc/startup/scripts/network_configure.sh
   d14000     2040  fs/os/aarch64le/bin/link
   d16040      3f1  fs/os/etc/startup/module/apple_auth_projection.xml
   d16431      3e6  fs/os/etc/startup/scripts/start_i2c-qm-4.2.sh
   d16817      3e2  fs/os/etc/startup/module/navigation_projection.xml
   d16bf9      3e2  fs/os/etc/startup/module/navigation.xml
   d17000    10068  fs/os/aarch64le/bin/soaadapter_status_change_test
   d27068      3e1  fs/os/etc/dhcpd.conf
   d27449      3e1  fs/os/etc/startup/module/navigation_profile.xml
   d2782a      3c4  fs/os/etc/startup/scripts/start_serial-qm-4.0.sh
   d27bee      363  fs/os/etc/Sync42HighMaster.conf
   d27f51       af  fs/os/etc/startup/scripts/mount_and_dd_run.sh
   d28000     3040  fs/os/aarch64le/bin/mesg
   d2b040      35d  fs/os/etc/startup/sync4-modular.xml
   d2b39d      358  fs/os/etc/startup/module/hardware.xml
   d2b6f5      352  fs/os/etc/startup/module/hardware-4.2.xml
   d2ba47      351  fs/os/etc/ssh/setup_client_ssh_keypair.sh
   d2bd98      261  fs/os/etc/startup/module/diagnostics.dtd
   d2c000    10068  fs/os/aarch64le/bin/soaadapter_subscriber_test
   d3c068      347  fs/os/etc/startup/scripts/start_pcie-qxp-4.0.sh
   d3c3af      347  fs/os/etc/startup/scripts/start_pcie-qxp-4.2.sh
   d3c6f6      346  fs/os/etc/startup/scripts/start_pcie-qm-4.2.sh
   d3ca3c      327  fs/os/etc/startup/module/system_services.xml
   d3cd63      297  fs/os/etc/startup/scripts/start_i2c-qxp-4.0.sh
   d3d000     c34c  fs/os/aarch64le/bin/su
   d4934c      320  fs/os/etc/startup/module/recovery_hmi.xml
   d4966c      319  fs/os/etc/startup/module/quick_rvc.xml
   d49985      318  fs/os/etc/sysconfig/skeletor_dirs.json
   d49c9d      315  fs/os/etc/startup/module/audio-4.2.xml
   d4a000     4040  fs/os/aarch64le/bin/etfsctl
   d4e040      315  fs/os/etc/dvfs/config/imx8/dvfs_pwrtbl_imx8qxp.conf
   d4e355      306  fs/os/etc/Sync42HighRemote.conf
   d4e65b      304  fs/os/etc/startup/module/fnv2_logging.dtd
   d4e95f      300  fs/os/etc/startup/module/receiver_traffic.xml
   d4ec5f      2fb  fs/os/etc/startup/module/ccsmd.xml
   d4ef5a       82  fs/os/etc/skel/.profile
   d4f000   17e0a8  fs/os/aarch64le/bin/soaadapter_unittest
   ecd0a8      2f0  fs/os/etc/startup/module/multimedia_config.xml
   ecd398      2ef  fs/os/etc/startup/module/a2b.xml
   ecd687      2eb  fs/os/etc/startup/module/multimedia_config_Applink.xml
   ecd972      2eb  fs/os/etc/startup/module/multimedia_config_profile.xml
   ecdc5d      2eb  fs/os/etc/startup/module/multimedia_config_projection.xml
   ecdf48       af  fs/os/etc/dvfs/config/imx8/dvfs_imx8qxp.conf
   ece000    b1078  fs/os/aarch64le/bin/soagateway
   f7f078      2ce  fs/os/etc/startup/scripts/fix_bootloader_emmc.sh
   f7f346      2cb  fs/os/etc/startup/scripts/start_i2c-qxp-4.2.sh
   f7f611      2c6  fs/os/etc/startup/module/pasa_framework.xml
   f7f8d7      2bf  fs/os/aarch64le/lib/terminfo/v/vi200-f
   f7fb96      2b5  fs/os/etc/startup/module/soa.dtd
   f7fe4b      18f  fs/os/root/.ssh/authorized_keys
   f80000    517e8  fs/os/aarch64le/bin/fordslm
   fd17e8      2ad  fs/os/etc/startup/module/location_service.xml
   fd1a95      284  fs/os/etc/startup/module/vehicle_services.xml
   fd1d19      281  fs/os/etc/startup/module/wlan_services.xml
   fd1f9a       62  fs/os/etc/hosts
   fd2000     3040  fs/os/aarch64le/bin/nice
   fd5040      27d  fs/os/etc/sysconfig/sysctl.conf
   fd52bd      277  fs/os/etc/startup/module/receiver_dab.xml
   fd5534      277  fs/os/etc/startup/module/stolen_vehicle_service.xml
   fd57ab      26e  fs/os/etc/startup/module/dsp.xml
   fd5a19      258  fs/os/etc/startup/module/chime.xml
   fd5c71      245  fs/os/aarch64le/lib/terminfo/q/qvt101
   fd5eb6      148  fs/os/etc/startup/scripts/dashcard_install_apps.sh
   fd6000    c8100  fs/os/aarch64le/bin/ipclite-lib-test
  109e100      245  fs/os/aarch64le/lib/terminfo/q/qvt108
  109e345      244  fs/os/etc/startup/module/software_update.dtd
  109e589      222  fs/os/etc/startup/module/connectivity.dtd
  109e7ab      21f  fs/os/etc/startup/module/vehicle_config.dtd
  109e9ca      21d  fs/os/etc/startup/module/tuner-4.2.xml
  109ebe7      217  fs/os/etc/startup/module/vehicle_config-4.2.dtd
  109edfe      1f4  fs/os/etc/startup/module/bezel_diagnostics.xml
  109f000     3080  fs/os/aarch64le/bin/MQTTVersion
  10a2080      213  fs/os/etc/config/gettytab
  10a2293      20e  fs/os/etc/dhclient.conf
  10a24a1      1f4  fs/os/etc/startup/module/rvc.xml
  10a2695      1ee  fs/os/etc/dvfs/config/imx8/dvfs_imx8qm.conf
  10a2883      1ee  fs/os/etc/dvfs/config/imx8/dvfs_sync42_imx8qm.conf
  10a2a71      1e4  fs/os/etc/startup/scripts/start_serial-qm-4.2.sh
  10a2c55      1e4  fs/os/etc/soa/gateway/config/config_menlo.json
  10a2e39      1c3  proc/boot/keys/dev-sync-image-codesign-s1.pub.key
  10a3000    12018  fs/os/aarch64le/bin/SS_IPC_Adapter
  10b5018      1d7  fs/os/aarch64le/lib/terminfo/v/viewpoint
  10b51ef      1d6  fs/os/aarch64le/lib/terminfo/v/vt52
  10b53c5      1d5  fs/os/etc/soa/gateway/config/config.json
  10b559a      1ca  fs/os/etc/startup/module/hmi.xml
  10b5764      1c3  proc/boot/keys/prod-sync-image-codesign-s1.pub.key
  10b5927      1c3  proc/boot/keys/cert_sync_app_qipfs_signing.pem
  10b5aea      1c3  proc/boot/keys/dev_cert_sync_app_qipfs_signing.pem
  10b5cad      1c3  fs/os/etc/keymgr/keys/cert_sync_debugtoken_signing.pem
  10b5e70      183  fs/os/etc/startup/scripts/mount_emmc.sh
  10b6000     3040  fs/os/aarch64le/bin/fordslmctl
  10b9040      1c3  fs/os/etc/keymgr/keys/dev_cert_sync_debugtoken_signing.pem
  10b9203      1c3  fs/os/keys/prod-sync-image-codesign-s1.pub.key
  10b93c6      1c3  fs/os/keys/dev-sync-image-codesign-s1.pub.key
  10b9589      1c3  fs/os/keys/cert_sync_app_qipfs_signing.pem
  10b974c      1c3  fs/os/keys/dev_cert_sync_app_qipfs_signing.pem
  10b990f      1c2  fs/os/sync4-qxp.bootstrap
  10b9ad1      1c2  fs/os/sync42-qxp.bootstrap
  10b9c93      1c0  fs/os/sync4-qm.bootstrap
  10b9e53      183  fs/os/etc/startup/scripts/mount_var.sh
  10ba000     3040  fs/os/aarch64le/bin/op
  10bd040      182  fs/os/etc/startup/module/stability_monitor_setup.xml
  10bd1c2      176  fs/os/etc/startup/module/evservice.xml
  10bd338      170  fs/os/etc/startup/module/receiver.xml
  10bd4a8      13f  fs/os/etc/startup/module/hmi_dashcard_members.xml
  10bd5e7      13c  fs/os/etc/startup/module/persistency.xml
  10bd723       ea  fs/os/etc/startup/device_config_defaults.dtd
  10bd80d       c9  fs/os/etc/startup/module/vehicle_services.dtd
  10bd8d6       7b  fs/os/etc/startup/module/bsp-high-4.2.dtd
  10bd951       7b  fs/os/etc/startup/module/bsp-low-4.0.dtd
  10bd9cc       7b  fs/os/etc/startup/module/bsp-low-4.2.dtd
  10bda47       78  fs/os/etc/startup/module/bsp-high-4.0.dtd
  10bdabf       5f  fs/os/etc/autoconnect
  10bdb1e       5e  fs/os/etc/syslog.conf
  10be000    24068  fs/os/aarch64le/bin/sm-crashdetector
  10e3000     8108  fs/os/aarch64le/bin/pr
  10ec000    17068  fs/os/aarch64le/bin/sm-heartbeat
  1104000     9068  fs/os/aarch64le/bin/sm-looper
  110e000     a4e0  fs/os/aarch64le/bin/blkfs_resmgr
  1119000    13130  fs/os/aarch64le/bin/pted
  112d000    180c8  fs/os/aarch64le/bin/sm-processlauncher
  1146000    21090  fs/os/aarch64le/bin/sm-sysmgr
  1168000    16068  fs/os/aarch64le/bin/sm-util
  117f000    10db0  fs/os/aarch64le/bin/bc
  1190000     7088  fs/os/aarch64le/bin/sort
  1198000    573e8  fs/os/aarch64le/bin/fnv2ipcd
  11f0000     a068  fs/os/aarch64le/bin/NS_SharedMem
  11fb000    62030  fs/os/aarch64le/bin/NS_NPPService
  125e000    18068  fs/os/aarch64le/bin/skeletor
  1277000     3040  fs/os/aarch64le/bin/basename
  127b000    a6218  fs/os/aarch64le/bin/bsdtar
  1322000     3044  fs/os/aarch64le/bin/expand
  1326000     3040  fs/os/aarch64le/bin/cat
  132a000     9080  fs/os/aarch64le/bin/bzip2
  1334000     3040  fs/os/aarch64le/bin/chemod
  1338000     4040  fs/os/aarch64le/bin/bzip2recover
  133d000     4040  fs/os/aarch64le/bin/chmod
  1342000     3040  fs/os/aarch64le/bin/cfgopen
  1346000     4048  fs/os/aarch64le/bin/chown
  134b000     c2e8  fs/os/aarch64le/bin/expr
  1358000     b0d0  fs/os/aarch64le/bin/cp
  1364000     705c  fs/os/aarch64le/bin/chat
  136c000     4040  fs/os/aarch64le/bin/tail
  1371000     c1f0  fs/os/aarch64le/bin/csplit
  137e000    2a358  fs/os/aarch64le/bin/file
  13a9000     5428  fs/os/aarch64le/bin/dd
  13af000    13718  fs/os/aarch64le/bin/find
  13c3000     4044  fs/os/aarch64le/bin/df
  13c8000     4040  fs/os/aarch64le/bin/fmt
  13cd000     4044  fs/os/aarch64le/bin/du
  13d2000     3040  fs/os/aarch64le/bin/fold
  13d6000     2040  fs/os/aarch64le/bin/echo
  13d9000     4040  fs/os/aarch64le/bin/fsysinfo
  13de000    17218  fs/os/aarch64le/bin/ed
  13f6000     3430  fs/os/aarch64le/bin/chattr
  13fa000    890e0  fs/os/aarch64le/bin/elvis
  1484000     3048  fs/os/aarch64le/bin/fullpath
  1488000     6040  fs/os/aarch64le/bin/esh
  148f000     5060  fs/os/aarch64le/bin/cksum
  1495000     2040  fs/os/aarch64le/bin/false
  1498000     7040  fs/os/aarch64le/bin/funzip
  14a0000     7040  fs/os/aarch64le/bin/fesh
  14a8000     2040  fs/os/aarch64le/bin/clear
  14ab000     30a0  fs/os/aarch64le/bin/hostname
  14af000    4f2f8  fs/os/aarch64le/bin/gawk
  14ff000     3260  fs/os/aarch64le/bin/kill
  1503000     9400  fs/os/aarch64le/bin/getconf
  150d000    3dd80  fs/os/aarch64le/bin/ksh
  154b000     d1e8  fs/os/aarch64le/bin/grep
  1559000     4040  fs/os/aarch64le/bin/ln
  155e000     6048  fs/os/aarch64le/bin/ls
  1565000     7058  fs/os/aarch64le/bin/cmp
  156d000     3044  fs/os/aarch64le/bin/tee
  1571000     4040  fs/os/aarch64le/bin/mkdir
  1576000     3058  fs/os/aarch64le/bin/comm
  157a000     f050  fs/os/aarch64le/bin/tic
  158a000     3040  fs/os/aarch64le/bin/mktemp
  158e000     7368  fs/os/aarch64le/bin/coreinfo
  1596000     5060  fs/os/aarch64le/bin/mount
  159c000    11fe4  fs/os/aarch64le/bin/gzip
  15ae000     4040  fs/os/aarch64le/bin/mv
  15b3000     5158  fs/os/aarch64le/bin/hd
  15b9000     7368  fs/os/aarch64le/bin/ps
  15c1000     3040  fs/os/aarch64le/bin/head
  15c5000     2040  fs/os/aarch64le/bin/pwd
  15c8000     3040  fs/os/aarch64le/bin/id
  15cc000     4040  fs/os/aarch64le/bin/rm
  15d1000    540d0  fs/os/aarch64le/bin/tar
  1626000     3040  fs/os/aarch64le/bin/uname
  162a000     6320  fs/os/aarch64le/bin/aps
  1631000     3040  fs/os/aarch64le/bin/cpu_load
  1635000     3120  fs/os/aarch64le/bin/confstr
  1639000     4048  fs/os/aarch64le/bin/crontab
  163e000     90e8  fs/os/aarch64le/bin/dumpifs
  1648000     5040  fs/os/aarch64le/bin/cut
  164e000     3040  fs/os/aarch64le/bin/time
  1652000     3040  fs/os/aarch64le/bin/getfacl
  1656000     504c  fs/os/aarch64le/bin/date
  165c000     60f8  fs/os/aarch64le/bin/top
  1663000     c358  fs/os/aarch64le/bin/login
  1670000     6050  fs/os/aarch64le/bin/deflate
  1677000     2040  fs/os/aarch64le/bin/logout
  167a000    12768  fs/os/aarch64le/bin/indent
  168d000     8040  fs/os/aarch64le/bin/on
  1696000    1a05c  fs/os/aarch64le/bin/diff
  16b1000     6040  fs/os/aarch64le/bin/tr
  16b8000     3040  fs/os/aarch64le/bin/pathtrust
  16bc000    1a550  fs/os/aarch64le/bin/pidin
  16d7000     3040  fs/os/aarch64le/bin/dirname
  16db000     3040  fs/os/aarch64le/bin/restart
  16df000     5040  fs/os/aarch64le/bin/setfacl
  16e5000     e070  fs/os/aarch64le/bin/infocmp
  16f4000     3058  fs/os/aarch64le/bin/lessecho
  16f8000     5598  fs/os/aarch64le/bin/lesskey
  16fe000     4044  fs/os/aarch64le/bin/lzmadec
  1703000     3040  fs/os/aarch64le/bin/lzmainfo
  1707000     7048  fs/os/aarch64le/bin/newgrp
  170f000     3040  fs/os/aarch64le/bin/nohup
  1713000    cf660  fs/os/aarch64le/bin/openssl
  17e3000     e31c  fs/os/aarch64le/bin/passwd
  17f2000     3040  fs/os/aarch64le/bin/paste
  17f6000     f470  fs/os/aarch64le/bin/patch
  1806000     3040  fs/os/aarch64le/bin/printf
  180a000     3040  fs/os/aarch64le/bin/renice
  180e000     2040  fs/os/aarch64le/bin/rmdir
  1811000     3040  fs/os/aarch64le/bin/sleep
  1815000     3044  fs/os/aarch64le/bin/strings
  1819000     5078  fs/os/aarch64le/bin/termdef
  181f000     3044  fs/os/aarch64le/bin/textto
  1823000     30a8  fs/os/aarch64le/bin/touch
  1827000    26050  fs/os/aarch64le/bin/traceprinter
  184e000     4040  fs/os/aarch64le/bin/tsort
  1853000     3040  fs/os/aarch64le/bin/tty
  1857000     3040  fs/os/aarch64le/bin/umask
  185b000     3044  fs/os/aarch64le/bin/unexpand
  185f000     4080  fs/os/aarch64le/bin/unifdef
  1864000     3040  fs/os/aarch64le/bin/uniq
  1868000     2040  fs/os/aarch64le/bin/unlink
  186b000    35088  fs/os/aarch64le/bin/unzip
  18a1000    1a080  fs/os/aarch64le/bin/unzipsfx
  18bc000     3040  fs/os/aarch64le/bin/uptime
  18c0000    13168  fs/os/aarch64le/bin/use
  18d4000     6070  fs/os/aarch64le/bin/usemsg
  18db000     6040  fs/os/aarch64le/bin/uud
  18e2000     4058  fs/os/aarch64le/bin/uue
  18e7000     3044  fs/os/aarch64le/bin/wc
  18eb000     4040  fs/os/aarch64le/bin/which
  18f0000     4054  fs/os/aarch64le/bin/xargs
  18f5000    100bc  fs/os/aarch64le/bin/xz
  1906000     4044  fs/os/aarch64le/bin/xzdec
  190b000    303b0  fs/os/aarch64le/bin/zip
  193c000    1461c  fs/os/aarch64le/bin/zipcloak
  1951000    35088  fs/os/aarch64le/bin/zipinfo
  1987000    12528  fs/os/aarch64le/bin/zipnote
  199a000    13528  fs/os/aarch64le/bin/zipsplit
  19ae000     b044  fs/os/aarch64le/lib/libusb.so.1
  19ba000     6074  fs/os/aarch64le/lib/libdma-edma.so.1
  19c1000     4040  fs/os/aarch64le/lib/libmu-mx8.so.1
  19c6000    1e068  fs/os/aarch64le/lib/libmenlosuggestionsidl.so
  19e5000    2a878  fs/os/aarch64le/lib/libfsshared.so.1
  1a10000     60e8  fs/os/aarch64le/lib/libwipe.so.1
  1a17000    1b404  fs/os/aarch64le/lib/libcam.so.2
  1a33000    12068  fs/os/aarch64le/lib/libtimeidl.so
  1a46000     806c  fs/os/aarch64le/lib/libslog2.so.1
  1a4f000     f068  fs/os/aarch64le/lib/libtokenmanageridl.so
  1a5f000     8040  fs/os/aarch64le/lib/libslog2parse.so.1
  1a68000    19068  fs/os/aarch64le/lib/libtvdmidl.so
  1a82000     4044  fs/os/aarch64le/lib/libslog2shim.so.1
  1a87000    40068  fs/os/aarch64le/lib/libucfidl.so
  1ac8000    373ac  fs/os/aarch64le/lib/libsocket.so.3
  1b00000    88068  fs/os/aarch64le/lib/libvimidl.so
  1b89000     5128  fs/os/aarch64le/lib/libutlfd.so.1
  1b8f000     9068  fs/os/aarch64le/lib/libSS_SystemIf.so.1
  1b99000    a0c58  fs/os/aarch64le/lib/libc.so.4
  1c3a000    27068  fs/os/aarch64le/lib/libvnmidl.so
  1c62000   249150  fs/os/aarch64le/lib/libprotobuf.so
  1eac000     304c  fs/os/aarch64le/lib/libdevice-publisher.so.1
  1eb0000    1d068  fs/os/aarch64le/lib/libwlanidl.so
  1ece000     8040  fs/os/aarch64le/lib/libhiddi.so.1
  1ed7000     9040  fs/os/aarch64le/lib/libjson.so.1
  1ee1000    22078  fs/os/aarch64le/lib/libsm-api.so
  1f04000     7040  fs/os/aarch64le/lib/libpps.so.1
  1f0c000    11608  fs/os/aarch64le/lib/libSS_Region_Lib.so.1
  1f1e000    10088  fs/os/aarch64le/lib/libNS_Logger.so.1
  1f2f000     5040  fs/os/aarch64le/lib/libNS_MessageCenter.so.1
  1f35000     4040  fs/os/aarch64le/lib/libNS_MessageQueue.so.1
  1f3a000     e068  fs/os/aarch64le/lib/libNS_SharedMemIf.so.1
  1f49000    540b0  fs/os/aarch64le/lib/libprotobuf-lite.so
  1f9e000     3040  fs/os/aarch64le/lib/libbmetrics.so
  1fa2000    18068  fs/os/aarch64le/lib/libfnvapplicationprocess.so
  1fbb000    10068  fs/os/aarch64le/lib/libfdtokenclient.so
  1fcc000     c068  fs/os/aarch64le/lib/libfnvdaemonprocess.so
  1fd9000    590b0  fs/os/aarch64le/lib/libtelemetry.so
  2033000    d5068  fs/os/aarch64le/lib/libalmidl.so
  2109000    2b1c8  fs/os/aarch64le/lib/libtcmalloc_minimal.so.9
  2135000    42068  fs/os/aarch64le/lib/libanalyticsidl.so
  2178000   895068  fs/os/aarch64le/lib/libftcpidl.so
  2a0e000    96068  fs/os/aarch64le/lib/libanalyticsinternalidl.so
  2aa5000    2e068  fs/os/aarch64le/lib/libsoaadapter.so
  2ad4000    cb068  fs/os/aarch64le/lib/libbtidl.so
  2ba0000   20c0f0  fs/os/aarch64le/lib/libsoaframework.so
  2dad000    59068  fs/os/aarch64le/lib/libcbzidl.so
  2e07000    66080  fs/os/aarch64le/lib/libfnvthread.so
  2e6e000    23068  fs/os/aarch64le/lib/libccsmdhmiidl.so
  2e92000    1f068  fs/os/aarch64le/lib/libipptidl.so
  2eb2000    31068  fs/os/aarch64le/lib/libcellularctrlidl.so
  2ee4000    14070  fs/os/aarch64le/lib/libsoaftestutil.so
  2ef9000   140068  fs/os/aarch64le/lib/libcmidl.so
  303a000    13068  fs/os/aarch64le/lib/libcpmipcidl.so
  304e000    19098  fs/os/aarch64le/lib/libipclite.so
  3068000    11068  fs/os/aarch64le/lib/libdcmidl.so
  307a000    2a068  fs/os/aarch64le/lib/libfciidl.so
  30a5000   17d068  fs/os/aarch64le/lib/libfnvsyncappsidl.so
  3223000    18068  fs/os/aarch64le/lib/libmplogmgridl.so
  323c000     3054  fs/os/aarch64le/lib/imx8x-gpu-scfw-set-power.so
  3240000    11068  fs/os/aarch64le/lib/libosssmidl.so
  3252000    58068  fs/os/aarch64le/lib/libswumswuamp.so
  32ab000    ac068  fs/os/aarch64le/lib/libreconn-hmi-mqtt.so
  3358000    15068  fs/os/aarch64le/lib/libtelemetryidl.so
  336e000   112068  fs/os/aarch64le/lib/libservicemanageridl.so
  3481000    12068  fs/os/aarch64le/lib/libsmsidl.so
  3494000    97068  fs/os/aarch64le/lib/libsoaidl.so
  352c000    8c108  fs/os/aarch64le/lib/libNS_FrameworkUnified.so.1
  35b9000    ff068  fs/os/aarch64le/lib/libspcmvdmidl.so
  36b9000    11068  fs/os/aarch64le/lib/libsvssproxyidl.so
  36cb000    4b068  fs/os/aarch64le/lib/libswuhmiidl.so
  3717000     c068  fs/os/aarch64le/lib/libNS_NPServiceIf.so.1
  3724000     d068  fs/os/aarch64le/lib/libVS_CANShadowIf.so.1
  3732000    20068  fs/os/aarch64le/lib/libsoaframeworklite.so
  3753000    1a270  fs/os/aarch64le/lib/libpaho-mqtt3a.so.1
  376e000    1f278  fs/os/aarch64le/lib/libpaho-mqtt3as.so.1
  378e000    18268  fs/os/aarch64le/lib/libpaho-mqtt3c.so.1
  37a7000    1c270  fs/os/aarch64le/lib/libpaho-mqtt3cs.so.1
  37c4000     a230  fs/os/aarch64le/lib/libkeyclient.so
  37cf000    8c0e8  fs/os/aarch64le/lib/libanalytics.so
  385c000    1b078  fs/os/aarch64le/lib/libsm-cpumonitor.so
  3878000    21078  fs/os/aarch64le/lib/libsm-internal.so
  389a000    17070  fs/os/aarch64le/lib/libsm-memmonitor.so
  38b2000    22068  fs/os/aarch64le/lib/libsm-resetcause.so
  38d5000    1ffc0  fs/os/aarch64le/lib/libsm-sysif.so
  38f5000     7040  fs/os/aarch64le/lib/libelfcore.so.1
  38fd000     7058  fs/os/aarch64le/lib/libtracelog.so.1
  3905000     2040  fs/os/aarch64le/lib/libcobs.so
  3908000     7040  fs/os/aarch64le/lib/librpmsg_lite-imx.so.1
  3910000     8060  fs/os/aarch64le/lib/libtar.so.1
  3919000     a0b0  fs/os/aarch64le/lib/libaoi.so.1
  3924000   2ec09c  fs/os/aarch64le/lib/libcrypto1_1.so.2.1
  3c11000    3c511  fs/os/aarch64le/lib/libedit.so.0
  3c4e000    17428  fs/os/aarch64le/lib/libformw.so.1
  3c66000     5048  fs/os/aarch64le/lib/libfsmerkle.so.1
  3c6c000     6a48  fs/os/aarch64le/lib/libgpt.so.1
  3c73000     d068  fs/os/aarch64le/lib/libintl.so.1
  3c81000     8170  fs/os/aarch64le/lib/libmenuw.so.1
  3c8a000     3058  fs/os/aarch64le/lib/libmq.so.1
  3c8e000    220d8  fs/os/aarch64le/lib/libncurses++w.so.1
  3cb1000    544d0  fs/os/aarch64le/lib/libncursesw.so.1
  3d06000    10160  fs/os/aarch64le/lib/libnxp_imx8_sci.so.1
  3d17000     4040  fs/os/aarch64le/lib/libpanelw.so.1
  3d1c000    19060  fs/os/aarch64le/lib/libpcre.so.3
  3d36000    aa7c0  fs/os/aarch64le/lib/libssl1_1.so.2.1
  3de1000     4048  fs/os/aarch64le/lib/libstrm.so.1
  3de6000     4040  fs/os/aarch64le/lib/libsync_critical_logger.so.1
  3deb000     7040  fs/os/aarch64le/lib/libtraceparser.so.1
  3df3000     a2bc  fs/os/aarch64le/lib/dll/devu-dcd-iap2ncm-mx8-ci.so
  3dfe000     9214  fs/os/aarch64le/lib/dll/devu-dcd-mx8-ci.so
  3e08000     a2bc  fs/os/aarch64le/lib/dll/devu-dcd-usbncm-mx8-ci.so
  3e13000     a364  fs/os/aarch64le/lib/dll/devu-dcd-usbser-mx8-ci.so
  3e1e000     9254  fs/os/aarch64le/lib/dll/devu-dcd-usbumass-mx8-ci.so
  3e28000    102d4  fs/os/aarch64le/lib/dll/devu-hcd-ehci-mx28.so
  3e39000    17350  fs/os/aarch64le/lib/dll/devu-hcd-imx8-xhci.so
  3e51000     7130  fs/os/aarch64le/lib/dll/deva-ctrl-dsp_aif.so
  3e59000     a190  fs/os/aarch64le/lib/dll/deva-ctrl-mxesai-mx8.so
  3e64000     a148  fs/os/aarch64le/lib/dll/deva-ctrl-mxsai-mx8.so
  3e6f000     5598  fs/os/aarch64le/lib/dll/deva-mixer-cs4234.so
  3e75000     5e00  fs/os/aarch64le/lib/dll/deva-mixer-cs42xx8.so
  3e7b000     6728  fs/os/aarch64le/lib/dll/deva-mixer-wm8960.so
  3e82000    18200  fs/os/aarch64le/lib/dll/devnp-mx8xp.so
  3e9b000     a170  fs/os/aarch64le/lib/dll/spi-imx8lpspi.so
  3ea6000    5a1c0  fs/os/aarch64le/lib/dll/charset.so
  3f01000    18198  fs/os/aarch64le/lib/dll/fs-dos.so
  3f1a000     e050  fs/os/aarch64le/lib/dll/fs-ext2.so
  3f29000    11050  fs/os/aarch64le/lib/dll/fs-mac.so
  3f3b000     a070  fs/os/aarch64le/lib/dll/fs-nt.so
  3f46000    38900  fs/os/aarch64le/lib/dll/fs-qnx6.so
  3f7f000    11064  fs/os/aarch64le/lib/dll/fs-qtd.so
  3f91000    170b8  fs/os/aarch64le/lib/dll/fs-udf.so
  3fa9000     7050  fs/os/aarch64le/lib/dll/fsf-merkle.so
  3fb1000    4db28  fs/os/aarch64le/lib/dll/io-blk.so
  3fff000     6088  fs/os/aarch64le/lib/dll/cam-cdrom.so
  4006000     5088  fs/os/aarch64le/lib/dll/cam-disk.so
  400c000     4088  fs/os/aarch64le/lib/dll/cam-optical.so
  4011000    1704c  fs/os/aarch64le/lib/dll/sync4-hwinfo-cfg.so
  4029000    12ab0  fs/os/aarch64le/lib/dll/pci/pci_hw-nxp-imx8-cpu.so.2.4
  403c000     5040  fs/os/aarch64le/sbin/tcpm-imx8qm
  4042000     b058  fs/os/aarch64le/sbin/sc-imx8
  404e000     3044  fs/os/aarch64le/sbin/dsp-env-example
  4052000     5044  fs/os/aarch64le/sbin/dsp-envctrl-example
  4058000     3044  fs/os/aarch64le/sbin/dsp-event-example
  405c000     6044  fs/os/aarch64le/sbin/dsp-hw-example
  4063000     e040  fs/os/aarch64le/sbin/dsp-imx
  4072000     3044  fs/os/aarch64le/sbin/dsp-pipe-example
  4076000    10658  fs/os/aarch64le/sbin/devb-ahci-mx8x
  4087000    1c460  fs/os/aarch64le/sbin/devb-sdmmc-mx8x
  40a4000    1222c  fs/os/aarch64le/sbin/devc-sermx8
  40b7000    1322c  fs/os/aarch64le/sbin/devc-sermx8-dma
  40cb000    2b3c0  fs/os/aarch64le/sbin/devf-flexspi-imx8
  40f7000     a048  fs/os/aarch64le/sbin/i2c-imx8
  4102000     50c0  fs/os/aarch64le/sbin/spi-slave
  4108000     60c0  fs/os/aarch64le/sbin/spi-master
  410f000     d3e8  fs/os/aarch64le/sbin/rtc-imx8x
  411d000     9050  fs/os/aarch64le/sbin/chkdosfs
  4127000     b188  fs/os/aarch64le/sbin/chkqnx6fs
  4133000     4088  fs/os/aarch64le/sbin/dloader
  4138000     c0a8  fs/os/aarch64le/sbin/fdisk
  4145000    14158  fs/os/aarch64le/sbin/fs-etfs-ram
  415a000     d140  fs/os/aarch64le/sbin/fsevmgr
  4168000     6040  fs/os/aarch64le/sbin/mkdosfs
  416f000    13140  fs/os/aarch64le/sbin/mkqnx6fs
  4183000     70e8  fs/os/aarch64le/sbin/wipe
  418b000    126e8  fs/os/aarch64le/sbin/devb-ahci
  419e000     60d0  fs/os/aarch64le/sbin/devb-loopback
  41a5000    135e0  fs/os/aarch64le/sbin/devb-nvme
  41b9000     5090  fs/os/aarch64le/sbin/devb-ram
  41bf000     d4c8  fs/os/aarch64le/sbin/devb-umass
  41cd000     8058  fs/os/aarch64le/sbin/inflator
  41d6000     b1f0  fs/os/aarch64le/sbin/io-hid
  41e2000     5048  fs/os/aarch64le/sbin/mq
  41e8000     6040  fs/os/aarch64le/sbin/mqueue
  41ef000     7048  fs/os/aarch64le/sbin/pipe
  41f7000     406c  fs/os/aarch64le/sbin/tinit
  41fc000    138a8  fs/os/aarch64le/sbin/de-service-resmgr
  4210000     604c  fs/os/aarch64le/sbin/hwinfo-resmgr
  4217000     7058  fs/os/aarch64le/sbin/imx8_stop_sleep
  421f000     50b8  fs/os/aarch64le/sbin/mount-os
  4225000     894c  fs/os/aarch64le/sbin/getty
  422e000     6054  fs/os/aarch64le/sbin/cron
  4235000     5050  fs/os/aarch64le/sbin/dcheck
  423b000    1f068  fs/os/aarch64le/sbin/dumper
  425b000     5050  fs/os/aarch64le/sbin/fileset
  4261000     427c  fs/os/aarch64le/sbin/logger
  4266000    1a0b8  fs/os/aarch64le/sbin/pps
  4281000     c088  fs/os/aarch64le/sbin/random
  428e000     7448  fs/os/aarch64le/sbin/syslogd
  4296000     5048  fs/os/aarch64le/sbin/tracelogger
  429c000     2040  fs/os/aarch64le/usr/libexec/awk/grcat
  429f000     2040  fs/os/aarch64le/usr/libexec/awk/pwcat
  42a2000    1db42  fs/os/aarch64le/boot/sys/ipl-imx8qm-cpu-card
  42c0000    1db42  fs/os/aarch64le/boot/sys/ipl-imx8qm-cpu-mek
  42de000    25020  fs/os/aarch64le/boot/sys/startup-imx8x-qm-cpu-card
  4304000    25130  fs/os/aarch64le/boot/sys/startup-imx8x-qm-cpu-mek
  432a000    23ab0  fs/os/aarch64le/boot/sys/startup-imx8x-qm-sync4
  434e000    26618  fs/os/aarch64le/boot/sys/startup-imx8x-qm-sync42
  4375000    b7870  fs/os/aarch64le/boot/sys/procnto-smp-instr
  442c870   800000  fs/os/etc/fs-var-stock.img
  4c2c870   500000  fs/os/etc/fs-run-stock.img
  512c870    d4e33  fs/os/usr/share/misc/magic
  52016a3    1cdbe  proc/boot/artifact.json
  521e461    1cdbe  fs/os/artifact.json
  523b21f     fd08  fs/os/etc/sysconfig/skeletor_queues_tempdirs.json
  524af27     f8c0  fs/os/etc/mig4nto.tab
  525a7e7     6547  fs/os/aarch64le/boot/build/qm-sync4.build
  5260d2e     6547  fs/os/aarch64le/boot/build/qm-sync42.build
  5267275     638a  fs/os/etc/stabilitymonitor/sm.cfg
  526d5ff     5dfd  proc/boot/group
  52733fc     5dfd  fs/os/etc/group
  52791f9     3b76  fs/os/etc/startup/sync4-modular.dtd
  527cd6f     3508  proc/boot/passwd
  5280277     3508  fs/os/etc/passwd
  528377f     1a5d  fs/os/etc/termcap
  52851dc     17ac  fs/os/etc/keymgr/keys/cert_sync_ota_signing_sw.pem
  5286988     17a6  proc/boot/keys/cert_sync_app_signing.pem
  528812e     17a6  fs/os/keys/cert_sync_app_signing.pem
  52898d4     175d  fs/os/etc/keymgr/keys/dev_cert_ford_apim_tls_root_ca.pem
  528b031     16bc  fs/os/etc/startup/module/display.dtd
  528c6ed     167f  fs/os/etc/startup/module/bluetooth.xml
  528dd6c     14b0  fs/os/etc/startup/module/projection.xml
  528f21c     1485  fs/os/etc/startup/module/main_navigation_profile.xml
  52906a1     147b  fs/os/etc/startup/module/bsp-high-4.2.xml
  5291b1c     1479  fs/os/etc/startup/module/projection_projection.xml
  5292f95     1479  fs/os/etc/startup/module/projection_Applink.xml
  529440e     146c  fs/os/etc/startup/module/main_Applink.xml
  529587a     13ce  fs/os/etc/startup/module/main_projection.xml
  5296c48     13bf  fs/os/etc/startup/module/main_multimedia_profile.xml
  5298007     1267  fs/os/etc/keymgr/keys/cert_ecg_soa_tls_ca.pem
  529926e     1256  fs/os/etc/startup/module/main.xml
  529a4c4     122c  fs/os/etc/startup/module/bsp-low-4.2.xml
  529b6f0     11ef  fs/os/etc/keymgr/keys/cert_ford_apim_tls_root_ca.pem
  529c8df     11ec  fs/os/etc/startup/module/bsp-low-4.0.xml
  529dacb     11a2  fs/os/etc/startup-imx8.sh
  529ec6d     116c  fs/os/etc/startup/module/bluetooth-4.2.xml
  529fdd9     1100  fs/os/etc/startup/module/hmi_dashcard_components.xml
  52a0ed9     1050  fs/os/etc/startup/module/bsp-high-4.0.xml
  52a1f29     102a  fs/os/etc/services
Checksums: image=0xba80ec1d startup=0x88f6cf6f00000000
Image startup_size: 332040 (0x51108)
Image stored_size: 23996008 (0x16e2668)
Compressed size: 23663964 (0x169155c)

```



# Part 7

```

Decompressed 4303641 bytes-> 11033984 bytes
   Offset     Size  Name
        0      100  Startup-header flags1=0xd flags2=0x1 paddr_bias=0x7fff00000000
      100    51008  startup.*
    51108       5c  Image-header mountpoint=/
    51164     13c4  Image-directory
     ----     ----  Root-dirent
     ----       12  fs/rwdata -> /fs/storage/rwdata
     ----       23  proc/boot/gpio_mapping.so -> /lib/dll/sync4_imx8_gpio_mapping.so
     ----        4  var/log -> /tmp
     ----        9  dev/console -> /dev/null
     ----        9  dev/debuglog -> /dev/ser3
     ----       14  artifact.json -> /fs/os/artifact.json
     ----       12  fs/images -> /fs/storage/images
     ----        6  fs/mp -> /fs/os
     ----        a  usr -> /fs/os/usr
     ----        a  sql -> /fs/os/sql
     ----       15  sbin -> /fs/os/aarch64le/sbin
     ----        b  root -> /fs/os/root
     ----        a  opt -> /fs/os/opt
     ----        c  menlo -> /fs/os/menlo
     ----       14  lib -> /fs/os/aarch64le/lib
     ----        e  fordhmi -> /fs/os/fordhmi
     ----        a  etc -> /fs/os/etc
     ----        9  db -> /fs/os/db
     ----       14  bin -> /fs/os/aarch64le/bin
     ----       10  aarch64le -> /fs/os/aarch64le
    52528      76a  proc/boot/start_i2c.sh
    52c92      1c3  proc/boot/keys/dev-sync-image-codesign-s1.pub.key
    52e55       3e  proc/boot/graphics_root
    53000    c8000  proc/boot/procnto-smp-instr
   11b000    13a93  proc/boot/devc-sermx8-dma
   12ea93      4f5  proc/boot/start_console.sh
   12f000     6265  proc/boot/libdma-edma.so.1
   135265      702  proc/boot/start_pcie.sh
   135967      3c4  proc/boot/start_serial.sh
   135d2b      1c3  proc/boot/keys/prod-sync-image-codesign-s1.pub.key
   136000     6265  proc/boot/libdma-edma.so
   13c265      700  proc/boot/pci_hw-imx8-external.cfg
   13c965      1c3  proc/boot/keys/cert_sync_app_qipfs_signing.pem
   13cb28      1c3  proc/boot/keys/dev_cert_sync_app_qipfs_signing.pem
   13d000    1ddba  proc/boot/devb-sdmmc-mx8x
   15b000    de170  proc/boot/libc++.so.1
   239170      6fe  proc/boot/pci_hw-imx8-internal.cfg
   23986e      6ec  proc/boot/.script
     ----        b  proc/boot/libc++.so.1.0 -> libc++.so.1
   23a000    3a3fd  proc/boot/libm.so.3
     ----        9  proc/boot/libm.so -> libm.so.3
   275000     557a  proc/boot/mount-os
   27b000    a1e74  proc/boot/libc.so.4
     ----        9  proc/boot/libc.so -> libc.so.4
   31d000    19261  proc/boot/libz.so.2
     ----        9  proc/boot/libz.so -> libz.so.2
   337000    5a505  proc/boot/charset.so
   392000    392fb  proc/boot/fs-qnx6.so
   3cc000    4ddc1  proc/boot/io-blk.so
   41a000    2aaae  proc/boot/libfsshared.so.1
     ----       10  proc/boot/libfsshared.so -> libfsshared.so.1
   445000    1b13a  proc/boot/libcam.so.2
     ----        b  proc/boot/libcam.so -> libcam.so.2
   461000     5981  proc/boot/cam-disk.so
   467000     569b  proc/boot/devb-ram
   46d000     c706  proc/boot/random
   47a000     5994  proc/boot/mount
   480000    3e3a1  proc/boot/ksh
   4bf000     33dc  proc/boot/cat
   4c3000     7492  proc/boot/pipe
   4cb000     c879  proc/boot/login
   4d8000     94db  proc/boot/waitfor
   4e2000     94db  proc/boot/on
   4ec000   13f546  proc/boot/squashfs
   62c000    71ed0  proc/boot/libssl.so.2
     ----        b  proc/boot/libssl.so -> libssl.so.2
   69e000   1cd3e3  proc/boot/libcrypto.so.2
     ----        e  proc/boot/libcrypto.so -> libcrypto.so.2
   86c000    375e0  proc/boot/libsocket.so.3
     ----        e  proc/boot/libsocket.so -> libsocket.so.3
   8a4000   17ed98  proc/boot/fs-qtd.so
   a23000    1fb7f  proc/boot/dumper
   a43000    1279b  proc/boot/slogger2
   a56000     628a  proc/boot/libslog2.so.1
     ----        d  proc/boot/libslog2.so -> libslog2.so.1
   a5d000    102c4  proc/boot/libnxp_imx8_sci.so.1
     ----       14  proc/boot/libnxp_imx8_sci.so -> libnxp_imx8_sci.so.1
   a6e000     b1b7  proc/boot/sc-imx8
   a7a000     6b8f  proc/boot/hwinfo-resmgr
   a81000     32fe  proc/boot/timestamp
   a85000    16215  proc/boot/sync4-hwinfo-cfg.so
     ----       13  proc/boot/hwinfo-cfg.so -> sync4-hwinfo-cfg.so
   a9c000     57e2  proc/boot/devb-loopback
   aa2000    2b1c8  proc/boot/libtcmalloc_minimal.so.9
     ----       18  proc/boot/libtcmalloc_minimal.so -> libtcmalloc_minimal.so.9
   ace000     59f4  proc/boot/group
   ad39f4     348f  proc/boot/passwd
Checksums: image=0x7807d3be startup=0x9d88d40600000000
Image startup_size: 332040 (0x51108)
Image stored_size: 4636028 (0x46bd7c)
Compressed size: 4303984 (0x41ac70)

```


# Part 8 


```

Decompressed 4086884 bytes-> 11117712 bytes
   Offset     Size  Name
        0      100  Startup-header flags1=0xd flags2=0x1 paddr_bias=0x7ffc00000000
      100    51008  startup.*
    51108       5c  Image-header mountpoint=/
    51164     160c  Image-directory
     ----     ----  Root-dirent
     ----       12  fs/rwdata -> /fs/storage/rwdata
     ----       23  proc/boot/gpio_mapping.so -> /lib/dll/sync4_imx8_gpio_mapping.so
     ----        4  var/log -> /tmp
     ----        9  dev/console -> /dev/null
     ----        9  dev/debuglog -> /dev/ser3
     ----       14  artifact.json -> /fs/os/artifact.json
     ----       12  fs/images -> /fs/storage/images
     ----        6  fs/mp -> /fs/os
     ----        a  usr -> /fs/os/usr
     ----        a  sql -> /fs/os/sql
     ----       15  sbin -> /fs/os/aarch64le/sbin
     ----        b  root -> /fs/os/root
     ----        a  opt -> /fs/os/opt
     ----        c  menlo -> /fs/os/menlo
     ----       14  lib -> /fs/os/aarch64le/lib
     ----        e  fordhmi -> /fs/os/fordhmi
     ----        a  etc -> /fs/os/etc
     ----        9  db -> /fs/os/db
     ----       14  bin -> /fs/os/aarch64le/bin
     ----       10  aarch64le -> /fs/os/aarch64le
    52770      76a  proc/boot/start_i2c.sh
    52eda       3e  proc/boot/graphics_root
    53000    c7000  proc/boot/procnto-smp-instr
   11a000    13a93  proc/boot/devc-sermx8-dma
   12da93      4f5  proc/boot/start_console.sh
   12e000     6265  proc/boot/libdma-edma.so.1
   134265      72a  proc/boot/keys/dev_cert_sync_app_signing.pem
   13498f      3c4  proc/boot/start_serial.sh
   134d53      1c3  proc/boot/keys/dev-sync-image-codesign-s1.pub.key
   135000     6265  proc/boot/libdma-edma.so
   13b265      702  proc/boot/start_pcie.sh
   13b967      1c3  proc/boot/keys/prod-sync-image-codesign-s1.pub.key
   13bb2a      1c3  proc/boot/keys/cert_sync_app_qipfs_signing.pem
   13bced      1c3  proc/boot/keys/dev_cert_sync_app_qipfs_signing.pem
   13c000    1ddba  proc/boot/devb-sdmmc-mx8x
   15a000    de170  proc/boot/libc++.so.1
   238170      700  proc/boot/pci_hw-imx8-external.cfg
   238870      6fe  proc/boot/pci_hw-imx8-internal.cfg
     ----        b  proc/boot/libc++.so.1.0 -> libc++.so.1
   239000    3a3fd  proc/boot/libm.so.3
   2733fd      6cc  proc/boot/.script
     ----        9  proc/boot/libm.so -> libm.so.3
   274000     557a  proc/boot/mount-os
   27a000     41a9  proc/boot/libsync_critical_logger.so.1
     ----       1c  proc/boot/libsync_critical_logger.so -> libsync_critical_logger.so.1
   27f000    a0e84  proc/boot/libc.so.4
     ----        9  proc/boot/libc.so -> libc.so.4
   320000    19261  proc/boot/libz.so.2
     ----        9  proc/boot/libz.so -> libz.so.2
   33a000    5a505  proc/boot/charset.so
   395000    392fb  proc/boot/fs-qnx6.so
   3cf000    4edc1  proc/boot/io-blk.so
   41e000    2aaae  proc/boot/libfsshared.so.1
     ----       10  proc/boot/libfsshared.so -> libfsshared.so.1
   449000    1c196  proc/boot/libcam.so.2
     ----        b  proc/boot/libcam.so -> libcam.so.2
   466000     5976  proc/boot/cam-disk.so
   46c000     569b  proc/boot/devb-ram
   472000     d900  proc/boot/fsevmgr
   480000     c736  proc/boot/random
   48d000     428d  proc/boot/libsecpol.so.1
     ----        e  proc/boot/libsecpol.so -> libsecpol.so.1
   492000     5994  proc/boot/mount
   498000    3e3a1  proc/boot/ksh
   4d7000     33dc  proc/boot/cat
   4db000     755c  proc/boot/pipe
   4e3000     c879  proc/boot/login
   4f0000     94db  proc/boot/waitfor
   4fa000     94db  proc/boot/on
   504000   140546  proc/boot/squashfs
   645000    aa9fa  proc/boot/libssl1_1.so.2.1
     ----       10  proc/boot/libssl1_1.so -> libssl1_1.so.2.1
   6f0000   2ec2d2  proc/boot/libcrypto1_1.so.2.1
     ----       13  proc/boot/libcrypto1_1.so -> libcrypto1_1.so.2.1
   9dd000    375da  proc/boot/libsocket.so.3
     ----        e  proc/boot/libsocket.so -> libsocket.so.3
   a15000    1189c  proc/boot/fs-qtd.so
   a27000    1fb7f  proc/boot/dumper
   a47000    12799  proc/boot/slogger2
   a5a000     82a6  proc/boot/libslog2.so.1
     ----        d  proc/boot/libslog2.so -> libslog2.so.1
   a63000    102c4  proc/boot/libnxp_imx8_sci.so.1
     ----       14  proc/boot/libnxp_imx8_sci.so -> libnxp_imx8_sci.so.1
   a74000     b1b7  proc/boot/sc-imx8
   a80000     6b8b  proc/boot/hwinfo-resmgr
   a87000     32fe  proc/boot/timestamp
   a8b000    1721d  proc/boot/sync4-hwinfo-cfg.so
     ----       13  proc/boot/hwinfo-cfg.so -> sync4-hwinfo-cfg.so
   aa3000     6844  proc/boot/devb-loopback
   aaa000    2b1c8  proc/boot/libtcmalloc_minimal.so.9
     ----       18  proc/boot/libtcmalloc_minimal.so -> libtcmalloc_minimal.so.9
   ad6000     aae7  proc/boot/artifact.json
   ae0ae7     5dfd  proc/boot/group
   ae68e4     3508  proc/boot/passwd
   ae9dec     17a6  proc/boot/keys/cert_sync_app_signing.pem
Checksums: image=0x2bf309ae startup=0x93187a8f00000000

```