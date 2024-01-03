# Password

```

#Pasa3Fo


```


```

ssh root@192.168.88.11

Password #Pasa3Fo

```


# Commands 


```shell

/fs/rwdata/dev/remount_rw.sh && sleep 1


/fs/rwdata/dev/reboot.sh


cat /fs/rwdata/fordlogs/pas_debug.log | grep QML

/fs/rwdata/dev/remount_ro.sh


```

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

# LOG

`nslogutil -q IPC_MID -m 0xFFFFFF -t rxontxon -s 1`



# FTP
```
slay inetd
pidin ar | grep ftp
```




# Reboot


```

slay APP_SUM
slay -s 9 NAV_Manager
slay -s 9 fordhmi
slay HMI_AL	

```



# pidin info

```
CPU:ARM Release:6.5.0  FreeMem:683Mb/2032Mb BootTime:Mar 19 17:35:42 UTC 2023
Processes: 139, Threads: 644
Processor1: 1093648626 Cortex A15 1000MHz FPU
Processor2: 1093648626 Cortex A15 1000MHz FPU
```



# LAN 


```shell

usb -vvv -N /dev/otg/io-usb-otg

cd /fs/mp/etc/usblauncher/
cat netstart.sh

edit : IPADDR=192.168.88.11



```


# USB


```

usb -vvv -N /dev/otg/io-usb-otg

> Vendor                     : 0x0bda (Realtek)
> Product                    : 0x8151 (USB 10/100 LAN)


Vendor                     : 0x0b95 (ASIX Elec. Corp.)
Product                    : 0x772b (AX88772C)


```

```shell

1. pidin ar | grep usblauncher_otg
2. usblauncher_otg -S1 -c /fs/mp/etc/usblauncher/rules_otg.lua -M /fs/mp/etc/mcd.mnt -s /fs/mp/lib/dll/pubs/ -n/dev/otg/io-usb-otg -vvvv -b -eErtl


/fs/mp/etc/usblauncher/netstart.sh busnum=$(busno),devnum=$(devno),path=$(USB_PATH),ign_remove /fs/mp/lib/dll/devnp-asix.so

cat /fs/mp/etc/usblauncher/rules_otg.lua | grep ASIX

/fs/mp/etc/usblauncher/netstart.sh busnum=0x01,devnum=0x2,path=/dev/otg/io-usb-otg,ign_remove /fs/mp/lib/dll/devnp-asix.so

```