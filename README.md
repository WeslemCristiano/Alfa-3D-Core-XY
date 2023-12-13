                                                      ## AFLA-3D

# Como usar o Klipper no Manta M8P e M4P

## OBSERVAÇÃO:

* Esta placa-mãe vem com bootloader que permite atualização de firmware através do cartão SD MCU.

## Construir imagem de firmware

1. Crie seu próprio firmware<br/>
    1. Consulte a [instalação oficial do klipper](https://www.klipper3d.org/Installation.html) para baixar o código-fonte do klipper para raspberry pi.
    2. `Construindo o microcontrolador` com a configuração mostrada abaixo. (Se o seu klipper não puder selecionar a seguinte configuração, atualize o código-fonte do seu klipper)
       * [*] Habilite opções extras de configuração de baixo nível
       * Arquitetura do microcontrolador = `STMicroelectronics STM32`
       * Modelo do processador = `STM32G0B1`
       *Deslocamento do bootloader = `bootloader de 8KiB`
       * Referência de relógio = `cristal de 8 MHz`
       * Interface de comunicação = `USB (em PA11/PA12)`
       * Se você planeja usar a ponte USB para CAN (somente com M8P V1.1+), use as duas linhas a seguir em vez da linha acima:
       * Interface de comunicação = `Ponte de barramento USB para CAN (USB (em PA11/PA12))`
       * Interface de barramento CAN = `(barramento CAN (em PD12/PD13))`

       <img src=Images/menuconfig.png width="800" /><br/>
    3. Assim que a configuração for selecionada, pressione `q` para sair e "Sim" quando solicitado para salvar a configuração.
    4. Execute o comando `make`
    5. O arquivo `klipper.bin` será gerado na pasta `home/pi/kliiper/out` quando o comando `make` for concluído. E você pode usar o computador Windows na mesma LAN do Raspberry Pi para copiar `klipper.bin` do Ra`M4P printer .cfg`.spberry Pi para o computador com o comando `pscp` no terminal CMD. como `pscp -C pi@192.168.0.101:/home/pi/klipper/out/klipper.bin c:\klipper.bin` (O terminal pode solicitar que `A chave do host do servidor não está armazenada em cache` e perguntar `Armazenar chave no cache?((s/n)`, digite `y` para armazenar. E então ele solicitará uma senha, digite a senha padrão `raspberry` para raspberry pi)

##Instalação de Firmware
1. Você pode usar o método em [Build Firmware Image 2.5](#build-firmware-image) ou usar uma ferramenta como `cyberduck` ou `winscp` para copiar o arquivo `klipper.bin` do seu pi para o seu computador .
2. Renomeado `firmware-USB.bin` ou `klipper.bin` (na pasta `home/pi/kliiper/out` criada por você mesmo) para `firmware.bin`<br/>
**Importante:** Se o arquivo não for renomeado, o bootloader não será atualizado corretamente.
3. Copie o `firmware.bin` para o diretório raiz do cartão SD MCU (certifique-se de que o cartão SD esteja no formato FAT32)
4. desligue a placa-mãe
5. insira o cartão SD MCU
6. ligue a placa-mãe
7. após alguns segundos, a placa-mãe deve piscar
8. você pode confirmar se o flash foi bem-sucedido executando `ls /dev/serial/by-id`. se o flash foi bem-sucedido, agora deverá mostrar um dispositivo klipper, semelhante a:

    <img src=Images/stm32g0b1_id.png width="600" /><br/>

## Configure os parâmetros da impressora
### Configuração básica
1. Consulte [instalação oficial do klipper](https://www.klipper3d.org/Installation.html) para `Configurando o OctoPrint para usar o Klipper`
2. Consulte [instalação oficial do klipper](https://www.klipper3d.org/Installation.html) para `Configurando o Klipper`. E use o arquivo de configuração 1). [generic-bigtreetech-manta-m8p.cfg](./CCT_UENP_M8P/generic-bigtreetech-manta-m8p-V1_1.cfg)  2). [generic-bigtreetech-manta-m4p.cfg](./M4P/generic-bigtreetech-manta-m4p.cfg) como o `printer.cfg` subjacente, que inclui toda a pinagem correta para Octopus
3. Consulte [Config_Reference oficial do klipper](https://www.klipper3d.org/Config_Reference.html) para configurar os recursos desejados.
4. Se você usar USB para se comunicar com o raspberry pi, execute o comando `ls /dev/serial/by-id/*` no raspberry pi para obter o número de ID correto da placa-mãe e defina o número de ID correto em `M8P e M4P printer .cfg`.
     ```
     M8P
     [mcu]
     serial: /dev/serial/by-id/usb-Klipper_stm32g0b1xx_3E0017000F504B4633373520-if00
     ```
     ```
     M4P
     [mcu]
     serial: /dev/serial/by-id/usb-Klipper_stm32g0b1xx_hurakan-if00
     ```
