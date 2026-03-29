# MCU and clock
MCU = atmega8
F_CPU = 8000000UL

# Compiler & tools
CC = avr-gcc
OBJCOPY = avr-objcopy

# Files
TARGET = main
SRC = main.c

# Flags
CFLAGS = -Wall -g -Os -mmcu=$(MCU) -DF_CPU=$(F_CPU) -std=gnu99
LDFLAGS = -mmcu=$(MCU)

# Default target
all: $(TARGET).hex

# Compile to ELF
$(TARGET).elf: $(SRC)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ $^

# Convert ELF to HEX
$(TARGET).hex: $(TARGET).elf
	$(OBJCOPY) -j .text -j .data -O ihex $< $@

# Clean build files
clean:
	rm -f *.elf *.hex *.bin

# Set fuses
fuses: $(TARGET).hex
	avrdude -c usbasp -p m8 -U lfuse:w:0xC4:m -U hfuse:w:0xD9:m

# Flash (optional, requires avrdude)
flash: $(TARGET).hex
	avrdude -c usbasp -p m8 -U flash:w:$(TARGET).hex:i

read:
	avrdude -c usbasp -p m8 -U eeprom:r:eeprom.hex:i