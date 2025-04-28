CC = arm-none-eabi-gcc
CFLAGS = -mcpu=cortex-m4 -mthumb -O2 -Wall
TARGET = main
SRC = $(wildcard src/*.c)
OBJ = $(SRC:.c=.o)
LDFLAGS = -T linker/STM32F401RE.ld -Wl,--gc-sections
all: $(TARGET).elf
	$(CC) $(CFLAGS) $(OBJ) $(LDFLAGS) -o $@
%.o: src/%.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f src/*.o $(TARGET).elf

