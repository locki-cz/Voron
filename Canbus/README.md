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

### Zapojení
Ujistěte se, že jste na SB2040 zapojili jumper/propojku pro zakončovací odpor CANBUS 120 ohmů. Na FLY-UTOC-1 nedáváte žádné propojky.

Zapojíte podle popisu pokud máte verzi jen s paticemi, pokud máte tu s microfit konektory tak takto:

![utoc](img/fly-utoc-1.png)

![zapojeni](img/zapojeni.png) 

### Vytvoření Canbus interface:

Doinstalujeme bylíčky které budeme potřebovat:

    sudo apt update && sudo apt install nano wget -y


vyrobíme konfiguraci interfacu, copykopírujte a vložte do konzole najednou a zmáčkněte Enter:

    sudo /bin/sh -c "cat > /etc/network/interfaces.d/can0" << EOF
    allow-hotplug can0
    iface can0 can static
        bitrate 500000
        up ifconfig \$IFACE txqueuelen 1024
    EOF

Automatické zapnutí CanBus interfacu při bootu RPI provedete příkazy:

    sudo wget https://cdn.mellow.klipper.cn/shell/can-enable -O /usr/bin/can-enable > /dev/null 2>&1 && sudo chmod +x /usr/bin/can-enable || echo "The operation failed"
    sudo cat /etc/rc.local | grep "exit 0" > /dev/null || sudo sed -i '$a\exit 0' /etc/rc.local
    sudo sed -i '/^exit\ 0$/i \can-enable -d can0 -b 500000 -t 1024' /etc/rc.local

Restartujeme RPI:

    sudo reboot


### Zjištění canbuss uuid

Zjistíme příkaze?

    python3 lib/canboot/flash_can.py -q

 Dostaneme odpověď:

    pi@Voron:~/klipper $ python3 lib/canboot/flash_can.py -q
    Resetting all bootloader node IDs...
    Checking for canboot nodes...
    Detected UUID: 211e59ecf887, Application: Klipper
    Query Complete

Zjištěné mé CanBus UUID je: **211e59ecf887** vaše bude jiné !!!!


### Nastavení
Nastavení configu printer.cfg

Zde si přidáme canbus mcu:

    [mcu sb2040]
    uuid: vase-id-napisete-sem     # vase uu id



