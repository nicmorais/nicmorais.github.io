---
layout: post
title: Sobre a câmera Intelbras Mibo IC3
---

Recentemente estava mexendo com a câmera Intelbras Mibo IC3 (clone da EZVIZ C2C), e como não há muita base sobre ela na internet, resolvi escrever este artigo.

![Image](https://i.imgur.com/xziOEC4.jpg)

## Assistir ao vivo
O primeiro fato interessante da câmera é o fato de ter transmissão RTSP ativada e com criptografia, compatível com o [VLC](https://www.videolan.org/index.pt-BR.html) e com o [MPV]([https://mpv.io/](https://mpv.io/))
Para assistir à transmissão no VLC, basta ir na seguinte opção:

![Image](https://i.imgur.com/RC6SqUo.png)

E digitar o seguinte:
rtsp://admin:<SENHA>@<IP da camera>

Exemplo: se o IP da sua câmera é `192.168.1.137`, e a senha (está escrito em baixo do aparelho) for "WJUGCN", você deverá digitar:
`rtsp://admin:WJUGCN@192.168.1.137`
## Imagem
![](https://i.imgur.com/NbVynZI.png)
Essa é uma amostra da imagem da câmera, em 720p. Note que a data e a hora podem mudar de cor para que seja possível ler tanto em ambientes claros como em ambientes escuros.
## Cartão de memória
Um problema que achei nessa câmera é que ela grava os arquivos no cartão de uma maneira bastante peculiar:

![Image](https://i.imgur.com/hiWuAlJ.png)
Esses arquivos, ainda que tenham a extensão mp4, não são compatíveis com o VLC. Vale ressaltar também que todos esses arquivos ocupam o cartão de memória inteiro, mesmo que apenas alguns minutos ou segundos de vídeo tenham sido de fato gravados.

## Abrindo a câmera
[![Camera Aberta](https://i.imgur.com/Gttb1jU.png)](https://imgur.com/a/Kf6RrMa)


### Clique na imagem para abrir o álbum de fotos
Dentro do aparelho há 4 pinos com o terminal serial (o UART), com baud rate de 115200. 

Há vários comandos disponíveis no terminal, no entanto, a maioria apenas retorna um erro quando executados. Comandos simples, como por exemplo `ls` não estão disponíveis. 
Não consegui achar qual processador é o dela, talvez seja ARM
Outro fato estranho é que também não consegui achar o chip de RAM.
### Bootlog:

[Pastebin](https://pastebin.com/W2fcXd5P)

Note na linha 21 que o sistema procura um arquivo chamado "ezviz.dav" no cartão de memória. Como não o encontra, continua a iniciar normalmente.
É provavel que tal arquivo corresponda ao firmware para atualizar o sistema.
## Firmware
Com um leitor de memórias SPI, pude fazer um dump da ROM da câmera, que depois extraí com o binwalk. 
Clique [aqui]() para baixar.
Atenção: se você gravar esse arquivo na câmera (com um gravador SPI) lembre-se de dar um reset no aparelho, já que a câmera estará configurada para conectar na minha rede de wifi.
Abaixo estão os arquivos encontrados após a extração:
![Image](https://i.imgur.com/VtYC5XK.png)
