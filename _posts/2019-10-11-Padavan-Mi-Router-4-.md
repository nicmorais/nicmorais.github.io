---
layout: post
title: Como instalar o Padavan no Mi Router 4
---

Nesse artigo mostrarei como instalar o firmware [Padavan](https://bitbucket.org/padavan/rt-n56u/src/master/) no Xiaomi Mi Router 4

![Image](https://i.imgur.com/ZKHu5u3.jpg)

Tentei também instalar o [Pandorabox](https://downloads.pangubox.com/pandorabox/), um Openwrt com drivers proprietários. No entanto, o boot não terminava.

Esse aparelho tem o mesmo processador do Mi R3G, e os arquivos que iremos usar são para esse aparelho.

## 1. Abrindo o aparelho
Na parte de baixo há um selo, onde você deve furar o adesivo para poder retirar um parafuso. Depois disso, basta retirar a tampa de baixo, destravando-a da parte superior.
![Aparelho Aberto](https://i.imgur.com/jL15axD.jpg)

## 2. Servidor TFTP e Firmware
Para substituir o Bootloader de fábrica, o Uboot, pelo Breed será preciso configurar um servidor de TFTP. Se você usa Windows, o [TFTP32](http://tftpd32.jounin.net/tftpd32_download.html) é a opção mais usada.

Se você usa Linux, terá de usar o tftp na linha de comando. Siga [esse](http://priede.bf.lu.lv/ftp/pub/OS/ruuteri/Lynksys/WRT54G/tftp.htm#linuxbsd) tutorial, colocando o uboot.bin na pasta `/var/lib/tftpboot`

### Arquivos
**Breed Bootloader**: Clique [aqui](https://breed.hackpascal.net/breed-mt7621-xiaomi-r3g.bin) para baixar o arquivo. Renomeie para `uboot.bin`

**Padavan**: Clique [aqui](https://www.mediafire.com/file/xhimniefjidm7cp/MI-R3G_3.4.3.9L-100.trx/file) para baixar o arquivo. Você também pode compilar o Padavan por conta própria, e depois gravar o arquivo que você compilou para gravar no roteador. Dá na mesma, mas baixar pré-compilado é bem mais rápido.

Configure seu IP como fixo em `192.168.31.33`, e coloque o tftp rodando nesse mesmo IP.
## 3. Conectando o UART
Na parte esquerda da placa, estão os 3 pinos que você deve conectar. Uma legenda dos pinos está impressa logo abaixo deles. A baud rate é 115200.

![UART](https://i.imgur.com/glQVvy5.jpg)

Dica: Você pode soldar pinos de Arduino, pois além de caberem dentro do aparelho eles também facilitam bastante caso você tenha que acessar o serial posteriormente
Caso você use Windows, use o [PuTTY](https://www.putty.org/) para poder se conectar ao serial.

Caso você use Linux, basta digitar o seguinte no terminal:
`sudo screen /dev/ttyUSB0 115200`


**Importante:** Para poder usar esse terminal é preciso antes dar um reset de fábrica no seu roteador. Depois disso, conecte o UART e prepare-se para gravar o Bootloader. Enquando dá boot, aparecerá o seguinte:
```
Please choose the operation: 
   1: Load system code to SDRAM via TFTP. 
   2: Load system code then write to Flash via TFTP. 
   3: Boot system code via Flash (default).
   4: Entr boot command line interface.
   7: Load Boot Loader code then write to Flash via Serial. 
   9: Load Boot Loader code then write to Flash via TFTP.
```

Nesse momento, aperte  o número 9. Vai aparecer o seguinte:
```
9: System Load Boot Loader then write to Flash via TFTP. 
 Warning!! Erase Boot Loader in Flash then burn new one. Are you sure?(Y/N)
 Please Input new ones /or Ctrl-C to discard
        Input device IP (192.168.31.1) ==:192.168.31.1
        Input server IP (192.168.31.33) ==:192.168.31.33
        Input Uboot filename (uboot.bin) ==:uboot.bin
```
Insira as informações acima. Note que a opção "server IP" deve ser o IP fixo configurado no seu computador.
Após receber o bootloader, o aparelho vai automaticamente gravar e reiniciar.

---
Feito isso, configure sua conexão no computador como **DHCP** (automática). Preste atenção quando o aparelho estiver iniciando para a seguinte mensagem:
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
Nesse momento, aperte qualquer tecla no teclado para parar a inicialização automática. Você verá então o seguinte:

`breed>
`

Nesse momento o boot parou, e você tem acesso ao terminal do Breed. Abra seu navegador e digite `192.168.1.1`. Você verá o seguinte:

![Breed Homepage](https://user-images.githubusercontent.com/20933693/64078899-fea74500-ccb6-11e9-9394-67e2d6b33f42.png)

---

Feito isso, vá na seguinte opção:

![https://i.imgur.com/28pq86W.png](https://i.imgur.com/28pq86W.png)

E carregue o arquivo `MI-R3G_3.4.3.9L-100.trx` (clique [aqui](https://www.mediafire.com/file/f4pmypmwnefla6o/MI-R3G_3.4.3.9L-100.trx/file) para baixar. Não será preciso mudar nenhuma configuração, apenas carregue o arquivo e confirme a gravação.

## Pronto!
Feito isso, você poderá usar livremente o Padavan. Ou, se você achou complexo demais, me envie um e-mail: nmorais.st@gmail.com

A interface estará, por padrão, em inglês. No entanto, há uma opção para mudar o idioma e deixar em português: Administration > Miscellaneous > Select WebUI Language

![Padavan Homepage](https://i.imgur.com/EhncuV8.png)


## Portas trocadas
Um problema nessa instalação é que a porta WAN fica trocada com uma porta LAN, como mostra a imagem. Funcionam normalmente, inclusive em gigabit, mas ficam trocadas.

![Portas](https://i.imgur.com/z4zTNQp.png)
