# Basic makefile K Sarkies

PROJECT		= buck-pmos-data-capture
PREFIX		= arm-none-eabi
CC			= $(PREFIX)-gcc
LD			= $(PREFIX)-gcc
OBJCOPY		= $(PREFIX)-objcopy
OBJDUMP		= $(PREFIX)-objdump
GDB			= $(PREFIX)-gdb
NM 			= $(PREFIX)-nm

DRIVERS_DIR	= /home/ksarkies/Development-Software/arm-library/libopencm3-examples/libopencm3
DRIVERS_SRC	= $(DRIVERS_DIR)/lib/stm32/f1
DRIVERS_INC	= $(DRIVERS_DIR)/include

VPATH += $(DRIVERS_SRC)/ $(DRIVERS_SRC)/../

CFLAGS		+= -Os -g -Wall -Wextra -I$(DRIVERS_INC) \
		   	   -fno-common -mcpu=cortex-m3 -mthumb -msoft-float -MD -DSTM32F1
LDSCRIPT	 = stm32-h103RBT6.ld
LDFLAGS		+= -I . -Wl,--start-group -lc -lgcc -lnosys -Wl,--end-group \
			   -T$(LDSCRIPT) -L$(DRIVERS_DIR)/lib -lopencm3_stm32f1 \
               -nostartfiles -Wl,--gc-sections \
		   	   -mthumb -march=armv7 -mfix-cortex-m3-ldrd -msoft-float

# The libopencm3 library is assumed to exist in libopencm3/lib, otherwise add files here
CFILES		= $(PROJECT).c buffer.c stringlib.c commslib.c

OBJS		= $(CFILES:.c=.o)

all: $(PROJECT).elf $(PROJECT).bin $(PROJECT).hex $(PROJECT).list $(PROJECT).sym

$(PROJECT).elf: $(OBJS)
	$(CC) -o $@ $^ $(CFLAGS) $(LDFLAGS)

$(PROJECT).hex: $(PROJECT).elf
	$(OBJCOPY) -O ihex  $< $@

$(PROJECT).bin: $(PROJECT).elf
	$(OBJCOPY) -O binary  $< $@

$(PROJECT).list: $(PROJECT).elf
	$(OBJDUMP) -S $< > $@

$(PROJECT).sym: $(PROJECT).elf
	$(NM) -n $< > $@

clean:
	rm *.elf *.o *.d *.hex *.list *.sym *.bin


