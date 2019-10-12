---
layout: post
title: How to install Padavan on a Mi Router 4
---

In this article, I'm gonna show you how you can install [Padavan](https://bitbucket.org/padavan/rt-n56u/src/master/) on a Mi Router 4

![Image](https://i.imgur.com/ZKHu5u3.jpg)

I've tried to install [Pandorabox](https://downloads.pangubox.com/pandorabox/) as well, an Openwrt with proprietary drivers, however, booting did not finish

This router and Mi R3G have the same SoC, and the files we are going to flash were made for the other router.

## 1. Disassembling the router
Under the device there's a sticker, where you must open a hole in order to remove a screw that's under it. Then, just take off the bottom cover.

![Aparelho Aberto](https://i.imgur.com/jL15axD.jpg)

## 2. TFTP server and firmware
To replace the OEM Bootloader, you must set up a TFTP server. If you run Windows, [TFTP32](http://tftpd32.jounin.net/tftpd32_download.html) is the best option for this.

If you run Linux, follow [this](http://priede.bf.lu.lv/ftp/pub/OS/ruuteri/Lynksys/WRT54G/tftp.htm#linuxbsd) tutorial. Put the uboot.bin file (see below) into `/var/lib/tftpboot`


### Arquivos
**Breed Bootloader**: Click [here](https://breed.hackpascal.net/breed-mt7621-xiaomi-r3g.bin) to download the file. Rename it to `uboot.bin`

**Padavan**: Click [here](https://www.mediafire.com/file/f4pmypmwnefla6o/MI-R3G_3.4.3.9L-100.trx/file) to download the file. You can also build it by yourself with [Prometheus](http://prometheus.freize.net/), but downloading it from that link is way faster.

Set your IP address as `192.168.31.33`, and set the TFTP server to run in this IP.

## 3. Connecting the UART port
In the left side of the PCB there are 3 pins you should connect to an UART-USB converter. The description of each pin is printed right below them. The baud rate is 115200 


![UART](https://i.imgur.com/glQVvy5.jpg)

If you run Windows, download [PuTTY](https://www.putty.org/) to connect to the UART port.

If you run Linux, run `sudo screen /dev/ttyUSB0 115200`


**Important:** To unlock this console you must do a factory reset in your router.

After connecting the UART port, you are going to see this:

```
Please choose the operation: 
   1: Load system code to SDRAM via TFTP. 
   2: Load system code then write to Flash via TFTP. 
   3: Boot system code via Flash (default).
   4: Entr boot command line interface.
   7: Load Boot Loader code then write to Flash via Serial. 
   9: Load Boot Loader code then write to Flash via TFTP.
```

Press the number 9 in your keyboard.

```
9: System Load Boot Loader then write to Flash via TFTP. 
 Warning!! Erase Boot Loader in Flash then burn new one. Are you sure?(Y/N)
 Please Input new ones /or Ctrl-C to discard
        Input device IP (192.168.31.1) ==:192.168.31.1
        Input server IP (192.168.31.33) ==:192.168.31.33
        Input Uboot filename (uboot.bin) ==:uboot.bin
```

Insert the variables above. The router will download the Bootloader from your TFTP server, flash it, and then reboot.

---
After this, set your computer to **DHCP** connection. Pay attention to the serial output for the following:
```
DRAM: 128MB
Platform: MediaTek MT7621A ver 1, eco 3
Board: Xiaomi R3G
Clocks: CPU: 880MHz, DDR: 1200MHz, Bus: 293MHz, Ref: 40MHz
Environment variables @ 00060000 on flash bank 0, size 00020000
Flash: ESMT NAND 128MiB 3.3V 8-bit (128MB) on mt7621-nfi.0
mt7621-nfi.0: Found Fact BBT at block 1023 (offset 0x07fe0000)
rt2880-eth: MAC address from EEPROM is invalid, using default settings.
rt2880-eth: Using MAC address 00:0c:43:00:00:01
eth0: MediaTek MT7530 Gigabit switch

Network started on eth0, inet addr 192.168.1.1, netmask 255.255.255.0

Press any key to interrupt autoboot ... 0
```
Press any key in your keyboard. You will see this:

`breed>
`

The boot stopped, and now you can access the Breed's web interface. Fire up your browser and go to `192.168.1.1`


![Breed Homepage](https://user-images.githubusercontent.com/20933693/64078899-fea74500-ccb6-11e9-9394-67e2d6b33f42.png)

---

Click this option:

![https://i.imgur.com/28pq86W.png](https://i.imgur.com/28pq86W.png)

And load the `MI-R3G_3.4.3.9L-100.trx` file (click [here](https://www.mediafire.com/file/f4pmypmwnefla6o/MI-R3G_3.4.3.9L-100.trx/file) to download it. 

## Done!
And now your router runs Padavan! Do you think this is too complex? e-mail me: nmorais.st@gmail.com

The web interface is, by default, in russian. However, you can change it: go to Administration > Miscellaneous > Select WebUI Language


![Padavan Homepage](https://i.imgur.com/Q9XN7zJ.png)


## Swapped ports
A gotcha in this firmware is the fact that the WAN port gets swapped with a LAN port. They work like a charm, but they're are swapped.


![Portas](https://i.imgur.com/z4zTNQp.png)
