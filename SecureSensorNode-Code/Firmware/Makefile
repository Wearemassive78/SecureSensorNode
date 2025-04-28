# 1) compiler
CC = arm-none-eabi-gcc

# 2) set a target for the  .elf file
TARGET = main

# 3) compiler flags
CFLAGS = -mcpu=cortex-m4 -mthumb -O2 -Wall \
    -DSTM32F401xE \
    -IDrivers/STM32CubeF4/Drivers/STM32F4xx_HAL_Driver/Inc \
    -IDrivers/STM32CubeF4/Drivers/CMSIS/Device/ST/STM32F4xx/Include \
    -IDrivers/STM32CubeF4/Drivers/CMSIS/Include \
    -Iinc

# 4) linker flags
LDFLAGS = -T linker/STM32F401RE.ld -Wl,--gc-sections

# 5) sources and objects
SRC = $(wildcard src/*.c)
OBJ = $(SRC:.c=.o) Drivers/startup/startup.o

# 6) default target
all: $(TARGET).elf

# 7) linkage
$(TARGET).elf: $(OBJ)
	$(CC) $(CFLAGS) $(OBJ) $(LDFLAGS) -o $@

# 8) compiler .c → .o
src/%.o: src/%.c
	$(CC) $(CFLAGS) -c $< -o $@

# 9) assembly .s → .o
Drivers/startup/%.o: Drivers/startup/%.s
	$(CC) $(CFLAGS) -x assembler-with-cpp -c $< -o $@

# 10) clean
clean:
	rm -f src/*.o Drivers/startup/*.o $(TARGET).elf


