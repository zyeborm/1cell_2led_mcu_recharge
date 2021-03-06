

SRC = $(wildcard *.c) 
OBJS = $(SRC:%.c=%.o)
HEADERS = $(SRC:%.c=%.h)

PRG				= 1cell_2LED
OBJ				= program.o 

### Edit this line to change the target controller
MCU_TARGET		= attiny84a
##MCU_TARGET		= at90usb162
### MCU_TARGET		= atmega8
OPTIMIZE		= -O2

# DEFS           = -W1,-u,vprintf -lprintf_flt -lm
DEFS			=
LIBS			= -lm

ASRC			= 

# You should not have to change anything below here.

CC				= avr-gcc
CXX				= avr-g++

# Override is only needed by avr-lib build system.

override CFLAGS			= -g -Wall $(OPTIMIZE) -mmcu=$(MCU_TARGET) $(DEFS) -I /usr/local/avr/avr/include
override CXXFLAGS		= -g -Wall $(OPTIMIZE) -mmcu=$(MCU_TARGET) $(DEFS)
override LDFLAGS		= -lm -Wl,-u,vfprintf -lprintf_flt
# -Wl, -Map,$(PRG).map
OBJCOPY			= avr-objcopy
OBJDUMP			= avr-objdump

all: $(PRG).elf lst text eeprom

$(PRG).elf: $(SRC) $(HEADERS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $(PRG).elf $(SRC) $(LIBS)

clean:
	rm -rf *.o $(PRG).elf *.eps *.png *.pdf *.bak 
	rm -rf *.lst *.map $(EXTRA_CLEAN_FILES)

install: 
	avrdude -p m168 -e -U flash:w:$(PRG).hex

lst:  $(PRG).lst

%.lst: %.elf
	$(OBJDUMP) -h -S $< > $@

# Rules for building the .text rom images

text: hex bin srec

hex:  $(PRG).hex
bin:  $(PRG).bin
srec: $(PRG).srec

%.hex: %.elf
	$(OBJCOPY) -j .text -j .data -O ihex $< $@

%.srec: %.elf
	$(OBJCOPY) -j .text -j .data -O srec $< $@

%.bin: %.elf
	$(OBJCOPY) -j .text -j .data -O binary $< $@

# Rules for building the .eeprom rom images

eeprom: ehex ebin esrec

ehex:  $(PRG)_eeprom.hex
ebin:  $(PRG)_eeprom.bin
esrec: $(PRG)_eeprom.srec

%_eeprom.hex: %.elf
	$(OBJCOPY) -j .eeprom --change-section-lma .eeprom=0 -O ihex $< $@

%_eeprom.srec: %.elf
	$(OBJCOPY) -j .eeprom --change-section-lma .eeprom=0 -O srec $< $@

%_eeprom.bin: %.elf
	$(OBJCOPY) -j .eeprom --change-section-lma .eeprom=0 -O binary $< $@

# Every thing below here is used by avr-libc's build system and can be ignored
# by the casual user.

FIG2DEV                 = fig2dev
EXTRA_CLEAN_FILES       = *.hex *.bin *.srec

dox: eps png pdf

eps: $(PRG).eps
png: $(PRG).png
pdf: $(PRG).pdf

%.eps: %.fig
	$(FIG2DEV) -L eps $< $@

%.pdf: %.fig
	$(FIG2DEV) -L pdf $< $@

%.png: %.fig
	$(FIG2DEV) -L png $< $@

