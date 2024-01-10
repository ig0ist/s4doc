# Credentials
```
root:#Pasa3Fo
```
ℹ️ Works only for old fw versions and reformat. Because `/etc/shadow` was removed in the new OS versions


# General info

```shell
# pidin info
CPU:ARM Release:6.5.0  FreeMem:683Mb/2032Mb BootTime:Mar 19 17:35:42 UTC 2023
Processes: 139, Threads: 644
Processor1: 1093648626 Cortex A15 1000MHz FPU
Processor2: 1093648626 Cortex A15 1000MHz FPU
```

```shell
# hwid
MY=17
RADIO_TYPE=NAVI
NVRAM_SIZE=64GB
RAM_SIZE=2GB
DRVC=0
CPU_FREQ=1000
HW_REVISION=A
```

`hwid bt`  

```shell
# uname -a
QNX localhost 6.5.0 2019/12/12-16:00:38CST TI-OMAP5432-Pasa-Ford-Sync3 armle
```

# LOG subsystem
`nslogutil -q IPC_MID -m 0xFFFFFF -t rxontxon -s 1`  
`cat /fs/rwdata/fordlogs/pas_debug.log | grep QML` 

Sync services common cmdline options:

`CustomCommandLineOption`

```
optstr: "l:m:t:c:p:qr:s"

-l (log output option)
    Allowed values: "slogger", "console", "shmem" (And always to fs/rwdata/fordlogs/)
    This vales may be combined with '|' Ex: -l "slogger|console"

-m (mask value) This log level masks(Up to 16 masks). Each mask represent certain subsystem.
                Ex: -m "0xFFFFFFFF,0xFFFFFFFF,0xFFFFFFFF,0xFFFFFFFF"

-r [plog output option (byte)] Enable PLOG
-s Enable sys event log
-q <??? %s>
-p (set priority) <??? %s>
-c (set config file) <filepath %s>
-t (set transmit logging) <??? %s>

EX: VS_CANShadow -m "0xFFFFFFFF,0xFFFFFFFF,0xFFFFFFFF,0xFFFFFFFF" -s -l "console"
```


# Network subsystem
## FTP
```
slay inetd
pidin ar | grep ftp
```


# USB subsystem

**usblauncher**
```shell
/fs/mp/etc/usblauncher/
├── iap2ea.lua
├── iap2.lua
├── iap2ncm.lua
├── netstart.sh
├── netstop.sh
├── override.lua
├── rules.lua
├── rules_otg.lua
├── startncm-molex.sh
├── startncm.sh
├── stopncm.sh
└── umass.lua
```

Useful commands:  
`usb -vvv -N /dev/otg/io-usb-otg`


## USB-LAN ethernet
List of supported NICs from `/fs/mp/etc/usblauncher/rules_otg.lua`

```lua
nic_devices = {
    device(0x0b95, 0x7720); -- CISCO linksys USB300M ethernet dongle
    device(0x0b95, 0x772b); -- Insignia NS-PU98505 ethernet dongle
    device(0x2001, 0x3c05); -- D-Link DUB-E100 (revB1) ethernet dongle
    device(0x2001, 0x1a02); -- D-Link DUB-E100 (revC1) ethernet dongle
}

foreach (nic_devices) {
    start"/fs/mp/etc/usblauncher/netstart.sh busnum=$(busno),devnum=$(devno),path=$(USB_PATH),ign_remove /fs/mp/lib/dll/devnp-asix.so";
    removal"/fs/mp/etc/usblauncher/netstop.sh";
}
```

Content of `/fs/mp/etc/usblauncher/netstart.sh`:
```shell
#!/bin/sh

IFACE=ax0
IPADDR=192.168.1.26
mount -T io-pkt -o $1 $2
ifconfig $IFACE $IPADDR netmask 255.255.255.0
echo "debug $IFACE done."
```

ℹ️ To get a working usb network on "production" OS, copy `devnp-asix.so` from reformat package to `/fs/mp/lib/dll/devnp-asix.so`


```shell
1. pidin ar | grep usblauncher_otg
2. usblauncher_otg -S1 -c /fs/mp/etc/usblauncher/rules_otg.lua -M /fs/mp/etc/mcd.mnt -s /fs/mp/lib/dll/pubs/ -n/dev/otg/io-usb-otg -vvvv -b -eErtl

/fs/mp/etc/usblauncher/netstart.sh busnum=0x01,devnum=0x2,path=/dev/otg/io-usb-otg,ign_remove /fs/mp/lib/dll/devnp-asix.so
```

# Storage/FS subsystem
## Bootloader + Startup IFS
```
Block#    Offset in bytes          Name
---------------------------------------------------
0x0000    0x0000000                MBR
0x0002    0x0000400 (1024)         boot bank info
0x0100    0x0020000 (131072)       MLO
0x0184    0x0030800 (198656)       IFS first bank
0x7cd2    0x0F9A400 (16360448)     IFS second bank
```

## Partition table
```shell
# fdisk /dev/hd0 show -l

     _____OS_____     Start       End       ______Number______   Size    Boot  
     name    type     Block       Block     Cylinders   Blocks                 

1.   DOS        6          32       65535        32       65504     31 MB   *
2.   QNX6     177       65536     3211263      1536     3145728   1536 MB
3.   QNX6     178     3211264     6356991      1536     3145728   1536 MB
4.   Extd'd     5     6356992   122142719     56536   115785728  56536 MB
4.1  nonQNX   180     6357024   119595007     55292   113237984  55291 MB
4.2  BSD      181   119595040   122138623      1242     2543584   1241 MB
4.3  nonQNX   182   122138656   122142719         2        4064      1 MB
```

## Mount hd0t180: /fs/images
Called from startup script  
```shell
mount -b -t qnx6 -o noatime,alignio /dev/hd0t180 /fs/images
```

## Mount hd0t177 or hd0t178: /fs/mp
Called from `/etc/script_mnt.sh`
```shell
mmc_get_act /dev/hd0 0 > /tmp/mmcinfo
mount -b -r -t qnx6 -o snapshot=0,noatime,alignio $MMC_ACT_FS_PART /fs/mp
```

```shell
# cat /tmp/mmcinfo 
MMC_ACT_FS_PART=/dev/hd0t177
MMC_PAS_FS_PART=/dev/hd0t178
MMC_ACT_IFS=0
MMC_PAS_IFS=1
ACT_IFS_ERROR=0
ACT_FS_ERROR=0
STARTUP_IFS_ERROR=0
```

## Mount hd0t181: /fs/rwdata
Called from startup script
```shell
mount -b -t qnx6 -o alignio /dev/hd0t181 /fs/rwdata &
```

## Mount ifs: /fs/mp/ifs
Called from startup script
```
mount_ifs -b -f /fs/mp/ifs/renderer-ifs -m / &
mount_ifs -b -f /fs/mp/ifs/quickapps-ifs -m / &
mount_ifs -b -f /fs/mp/ifs/second-ifs -m / &
```

## Mount images
Called from `sh /etc/mount_all_images.sh`

## Some info
```shell
ls -la /fs/images/ 
total 41277473
drwxrwxr-x  5 root      root           4096 Jan 01  1970 .
drwxr-xr-t  2 root      root             10 Jan 01  1970 ..
drwx------  2 root      root           4096 Jan 01  1970 .boot
drwxrwxrwx  4 root      root           4096 Jan 01 00:10 ivsu_cache
drwxrwxrwx  2 root      root           4096 Jan 01 00:10 ivsu_installcache
-rw-rw-rw-  1 root      root      3351511040 Jan 01  1970 map_comn.img
-rw-rw-rw-  1 root      root      3456892928 Jan 01  1970 map_euc.img
-rw-rw-rw-  1 root      root      3985637376 Jan 01  1970 map_eue.img
-rw-rw-rw-  1 root      root      3018063872 Jan 01  1970 map_eus.img
-rw-rw-rw-  1 root      root      1243611136 Jan 01  1970 map_ext.img
-rw-rw-rw-  1 root      root       76283904 Jan 01  1970 station_logos.img
-rw-rw-rw-  1 root      root      2072248320 Jan 01  1970 voice.img
-rw-rw-rw-  1 root      root      3929800704 Jan 01  1970 voice_nav_eucpr.img
```

```shell
df -h
/fs/rwdata/ppsp                0         0         0     100%  /ppsp           
/dev/blk/ram-0-allo         128M      1.2M      127M       1%  /fs/tmpfs/      
/fs/images/map_ext.         1.1G      1.1G      892K     100%  /fs/sd/MAP/     
/fs/images/map_eus.         2.8G      2.7G       14M     100%  /fs/sd/MAP/     
/fs/images/map_eue.         3.7G      3.6G       21M     100%  /fs/sd/MAP/     
/fs/images/map_euc.         3.2G      3.2G      5.7M     100%  /fs/sd/MAP/     
/fs/images/map_comn         3.1G      3.0G       24M     100%  /fs/sd/MAP/     
/fs/images/voice_na         3.6G      3.6G       35M     100%  /fs/Nuance/     
/fs/images/voice.im         1.9G      1.9G      8.4M     100%  /fs/Nuance/     
/dev/hd0t181                1.2G      849M      393M      69%  /fs/rwdata/     
/dev/diagscratch0            35M       30K       35M       1%  /fs/rwdata/quip/
/dev/diagevents0             90M       68K       90M       1%  /fs/rwdata/quip/
/dev/hd0t177                1.4G      1.3G      126M      92%  /fs/mp/         
/fs/images/station_          73M       72M      432K     100%  /fs/mp/resources
/dev/hd0t180                 54G       22G       32G      41%  /fs/images/     
/var/pps                       0         0         0     100%  /pps            
/dev/hd0                    1.9M      1.9M         0     100%  /dev/hd0t182    
/dev/hd0                    1.5G      1.5G         0     100%  /dev/hd0t178    
/dev/hd0                     32M       32M         0     100%  /dev/hd0t6      
/dev/hd0                     58G       58G         0     100%  
```

```shell
mount
/fs/rwdata/ppsp on /ppsp type PPS 
/dev/blk/ram-0-alloc-0-tmp on /fs/tmpfs type tmp 
/fs/images/map_ext.img on /fs/sd/MAP type qnx6 
/fs/images/map_eus.img on /fs/sd/MAP type qnx6 
/fs/images/map_eue.img on /fs/sd/MAP type qnx6 
/fs/images/map_euc.img on /fs/sd/MAP type qnx6 
/fs/images/map_comn.img on /fs/sd/MAP type qnx6 
/fs/images/voice_nav_eucpr.img on /fs/Nuance type qnx6 
/fs/images/voice.img on /fs/Nuance type qnx6 
/dev/hd0t181 on /fs/rwdata type qnx6 
/dev/diagscratch0 on /fs/rwdata/quip/forensics type qnx4 
/dev/diagevents0 on /fs/rwdata/quip/events type qnx4 
/dev/hd0t177 on /fs/mp type qnx6 
/fs/images/station_logos.img on /fs/mp/resources/station_logo/Logo type qnx6 
/dev/hd0t180 on /fs/images type qnx6 
/var/pps on /pps type PPS 
```

## Commands 

```shell
/fs/rwdata/dev/remount_rw.sh
/fs/rwdata/dev/remount_ro.sh
```

# Misc
*Некоторые полезные команды для сбора информации*:
```shell
# pidin mem
# pci
# pidin fd
# pidin ar
# hwid
# use hwid
# pidin -P qconn mem
# ldd `which qconn`
# cpld display off
# cpld display on
# cpld status
# cat /ppsp/services/reconn/vehicle
# ls -lR /dev/mq/ 
# use -ir
# use cpld
```

Reboot board  
```shell
slay APP_SUM
slay -s 9 NAV_Manager
slay -s 9 fordhmi
slay HMI_AL	
```
/fs/rwdata/dev/reboot.sh
