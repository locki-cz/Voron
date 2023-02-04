# Canbus, canboot a ja na to
my voron mods and custom par

## Canboot

## Klipper
### Zkompilujeme klipper firmware

cd ~/klipper && git pull
make menuconfig
make -j4

Teď nahrajeme do SB2040
lsusb

cd ~/klipper/
make flash FLASH_DEVICE=2e8a:0003


## Zapojení

