# Canbus, canboot a ja na to
my voron mods and custom par

## Canboot

## Klipper
### Zkompilujeme klipper firmware

    cd ~/klipper && git pull
    make menuconfig

   nastavíme takto:
   ![klipper](/img/klipper.png) 
    make -j4

Teď nahrajeme d
    lsusb
to nám vypíše všechna id usb zařízení, nás zajímá toto:

    Bus 001 Device 014: ID 2e8a:0003

mělo by to vypsat toto:

    cd ~/klipper/
    make flash FLASH_DEVICE=2e8a:0003


## Zapojení

