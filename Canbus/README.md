# CanBus, Canboot a jak na to

Jak nahrát Canboot a klipper do CanBus desky Mellow SB2040 v1

![klipper](img/sb2040.png) 

## 1. Canboot
Tento krok můžete přeskočit a není úplně nutný k plné funkčnosti, ale do budoucna je fajn pak mít možnost nahrát nový klipper firmware skrz CanBus interface. **Nemusíte připojovat USB kabel do extruderu.**

Vlezeme do své home složky:

    cd ~

Stáheneme poslední verzi CanBoot z gitu:

    git clone https://github.com/Arksine/CanBoot
    cd CanBoot

  Odstraníme případné předešlé kompilace:

    make clean

Provedeme nastavení HW pro který to kompilujeme:

    make menuconfig

Nastavíme takto:  

![canboot](img/canboot.png) 

### Zkompilujeme:

    make -j4

### Teď nahrajeme námi zkompilovaný CanBoot firmware do SB2040 desky:

Připojíme usb kabel do RPI, zmáčkneme tlačítko u USB-C na SB2040 a až pak připojíme USB kabel, pustíme tlačítko.
### Vypíšeme si usb zařízení:

    lsusb

Mělo by se zobrazit toto:

    Bus 001 Device 014: ID 2e8a:0003

### Nahrajeme CanBoot firmware

    make flash FLASH_DEVICE=2e8a:0003


## 2. Klipper firmware
### Zkompilujeme firmware

Vlezeme do klipper složky a stáhneme poslední aktualizaci z gitu:

    cd ~/klipper && git pull

Odstraníme předešlé kompilace:

    make clean

Provedeme nastavení HW pro který to kompilujeme:

    make menuconfig

Nastavíme takto:

![klipper](img/klipper.png) 

Nezapomeňte dopsat: rychlost **500000, nebo až 1000000 a gpio24**

Zmáčkneme **q** pro uložení a **y** pro potvrzení

### Zkompilujeme:

    make -j4

### Teď nahrajeme námi zkompilovaný firmware do SB2040 desky:

Připojíme usb kabel do RPI, zmáčkneme tlačítko u USB-C na SB2040 a až pak připojíme USB kabel, pustíme tlačítko.
### Vypíšeme si usb zařízení:

    lsusb

Mělo by se zobrazit toto:

    Bus 001 Device 014: ID 2e8a:0003

### Nahrajeme klipper firmware

    make flash FLASH_DEVICE=2e8a:0003

### Zkontrolujeme

    lsusb

a měli bychom vidět:

    Bus 001 Device 013: ID 1d50:614e OpenMoko, Inc.

Rozsvítí se status led:
![status](img/statusled.png)

## 3. Zapojení a nastavení

Ujistěte se, že jste na SB2040 zapojili jumper/propojku pro zakončovací odpor CANBUS 120 ohmů.


![utoc](img/fly-utoc-1.png)


![zapojeni](img/zapojeni.png) 

