# Polling Rate Protocol

Polling rate é uma opção bastante usada em mouses, basicamente é a quantidade de vezes que o mouse vai conversar com o
host (computador) - por exemplo quando você move o mouse o mouse envia as cordenadas do seu movimento para o computador
e então o computador move o mouse de acordo com esse movimento, mas para ter um movimento suave ao mover o curso o mouse
pode por exemplo registra e mandar para o computador 1000 eventos por segundo para que o computador possa ter uma
precissão extrema do seu movimento.

O Attack Shark X11 disponibliza 4 opções de polling rate: `125Hz`, `250Hz`, `500Hz`, `1000Hz` e como dito anteriormente
cada opção diz a quantidade de eventos ele vai mandar para o host por segundo.

## Wireshark

Para que você possa rastrear os pacotes de polling rate entre o host e o mouse você pode usar o filtro
`usbhid.setup.wValue == 0x0306`

## Protocol

aqui os pacotes de cada opção:

```shell
06 09 01 08 f7 00 00 00 00 # 125Hz
06 09 01 04 fb 00 00 00 00 # 250Hz
06 09 01 02 fd 00 00 00 00 # 500Hz
06 09 01 01 fe 00 00 00 00 # 1000Hz
```

Bem simples né? o que muda em cada opção é o bytes 3 e 4 - o byte 3 é referente a opção escolhida e o byte 4 é o
checksum, aqui vai a tabela para simplificar:

| Rate (Hz) | Hex Value | Checksum | Profile Name |
|-----------|-----------|----------|--------------|
| 125 Hz    | `0x08`    | `0xf7`   | Power Saving |
| 250 Hz    | `0x04`    | `0xfb`   | Office       |
| 500 Hz    | `0x02`    | `0xfd`   | Gaming       |
| 1000 Hz   | `0x01`    | `0xfe`   | eSports      |

## Amostras

A amostra dos packets para cada um das opções pode ser obtida [aqui](/samples/x11/polling-rate-sample.pcapng) assim
como a response para cada request.
