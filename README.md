# Upgrade Firmware for Mini Digital Oscilloscope DSO138 in linux
> The same as stm32 (because it's exactly a stm32 mcu)

### Step 0
```bash
# add user to group dialout, plugdev
sudo usermod -a -G dialout $USER
sudo usermod -a -G plugdev $USER

# install flash tool
sudo apt-get install stm32flash
```

### Step1
Close the jumpers JP1 and JP2 on the bottom
side of DSO138 board with solder. This will make the
MCU enter bootloader instead of the main firmware at next
power up.

### Step 2
Connect TTL-UART-to-USB converter to the DSO138

```
DSO138  -- TTL-USB -- PC
  TXD   ->   RXD 
  RXD   <-   TXD
  GND   --   GND
```

Power up the oscilloscope.

### Step 3
```bash
# Unlock the STM32:
# - Turn of the read protection
sudo stm32flash /dev/ttyUSB0 -k -b 115200
# - (Optional) clear the memory:
sudo stm32flash /dev/ttyUSB0 -o
# - Turn off the flash write protection
sudo stm32flash /dev/ttyUSB0 -u -b 115200

# flash hex
sudo stm32flash /dev/ttyUSB0 -w ./113-13801-061.hex -b 115200
```

### Step 4

- Power off DSO138 and disconnect the connection.
- Remove the solders on JP1 and JP2.
- Check the resistor R11. If it is 1.5KΩ, replace it by a 150Ω.

### Step 5
Apply power to the oscilloscope again. Check if the oscilloscope boots up properly. Verify if the
firmware versions are correct. If everything is good the upgrading is done.

# MISC
https://github.com/ardyesp/DLO-138
