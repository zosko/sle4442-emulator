# SLE4442 Emulator
This is a smartcard emulator for ATMEGA8 microcontroller.

## Schematic
![Schematic](scheme.png)

## Schematic
![Board](board.png)

## Instructions
- Modify the "memory.h" file with the data of the card that you want to emulate
- If you compile the file with "#define BACKDOOR" uncommented, you will emulate a card that will be unlocked using any PSC code. Also, the PSC code sent by the reader will be updated immediately on the Security memory.  
- **WARNING**  
The maximum input clock frequency will be around 35KHz, not the 50KHz specified in the SLE4442 datasheet. I've tried my best to optimize the code but this is the main limitation of this project, you can try to use an external 16MHz clock for a better performance.

![Old](initial-scheme.png)
Real scheme used from https://github.com/sonovice/sle4442.

# Programming instruction
For Windows, download WinAVR from https://sourceforge.net/projects/winavr/  
For Linux, install the required files using this command:
```
sudo apt-get install gcc-avr avr-libc avrdude
```

## Compile the code
Put all the files (**main.c memory.h sle4442.h**) in one folder. Execute this command:
```
avr-gcc -Wall -g -Os -mmcu=atmega8 -std=gnu99 -o main.bin main.c
```

## Generate .hex file
```
avr-objcopy -j .text -j .data -O ihex main.bin main.hex
```

<br/>The following commands are for USBasp programmer. For other programmers, look at the avrdude documentation.  

## Writing fuses
Execute those commands only one time, no need to write fuses every time you burn the flash.
```
avrdude -p atmega8 -c usbasp -U lfuse:w:0xC4:m
avrdude -p atmega8 -c usbasp -U hfuse:w:0xD9:m
```

## Burning
```
avrdude -p atmega8 -c usbasp -U flash:w:main.hex:i -F -P usb
```

## Read EEPROM
Once the PSC is grabbed, you can read the microcontroller EEPROM using this command:
```
avrdude -p atmega8 -c usbasp -U eeprom:r:eeprom.hex:i
```

Open **eeprom.hex** with a text editor, PSC is present here:
![](https://i.imgur.com/WLMl7ar.png)
