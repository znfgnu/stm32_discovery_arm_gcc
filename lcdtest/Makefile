
# Binary will be generated with this name (.elf, etc)
PROJ_NAME=lcdtest

# Put your STM32F4 library code directory here
STM_COMMON=../STM32F4-Discovery_FW_V1.1.0
TM_DIR=../TMLibraries/

# Source files
SRCS=main.c system_stm32f4xx.c
SRCS += $(TM_DIR)/tm_stm32f4_delay.c
SRCS += $(TM_DIR)/tm_stm32f4_disco.c
SRCS += $(TM_DIR)/tm_stm32f4_fonts.c
SRCS += $(TM_DIR)/tm_stm32f4_ssd1306.c
SRCS += $(TM_DIR)/tm_stm32f4_timer_properties.c
SRCS += $(TM_DIR)/tm_stm32f4_gpio.c
SRCS += $(TM_DIR)/tm_stm32f4_i2c.c
SRCS += $(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/src/stm32f4xx_rcc.c
SRCS += $(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/src/stm32f4xx_i2c.c

# gcc-arm-none-eabi bin directory here
GCC_DIR=/home/konrad/STMWorkspace/toolchain/gcc-arm-none-eabi-4_9-2015q2/bin

# Normally you shouldn't need to change anything below this line!
#######################################################################################

CC=$(GCC_DIR)/arm-none-eabi-gcc
SIZE=$(GCC_DIR)/arm-none-eabi-size
GDB=$(GCC_DIR)/arm-none-eabi-gdb

#Debug
CFLAGS  = -ggdb -O0 -Wall -Tstm32_flash.ld

CFLAGS += -mlittle-endian -mthumb -mcpu=cortex-m4 -mthumb-interwork
CFLAGS += -I.
CFLAGS += -lc -lm -DUSE_FULL_ASSERT=1 -Dassert_param\(expr\)=\(\(void\)0\)

# Include files from STM libraries (from blinky project)
CFLAGS += -I$(STM_COMMON)/Utilities/STM32F4-Discovery
CFLAGS += -I$(STM_COMMON)/Libraries/CMSIS/Include -I$(STM_COMMON)/Libraries/CMSIS/ST/STM32F4xx/Include
CFLAGS += -I$(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/inc
CFLAGS += -I$(TM_DIR)

# add startup file to build
SRCS += $(STM_COMMON)/Libraries/CMSIS/ST/STM32F4xx/Source/Templates/TrueSTUDIO/startup_stm32f4xx.s
OBJS = $(SRCS:.c=.o)

SEMIHOSTING_FLAGS = --specs=rdimon.specs -lc -lrdimon



.PHONY: all

all: $(PROJ_NAME).elf

$(PROJ_NAME).elf: $(SRCS)
	$(CC) $(SEMIHOSTING_FLAGS) $(CFLAGS) $^ -o $@
	$(SIZE) $@

clean:
	rm -f $(PROJ_NAME).elf


#######################################################
# Debugging targets
#######################################################
gdb: all
	$(GDB) $(PROJ_NAME).elf

# Start OpenOCD GDB server (supports semihosting)
openocd:
	openocd -f board/stm32f4discovery.cfg
