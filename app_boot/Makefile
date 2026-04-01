# Toolchain
TOOLCHAIN_PATH=/buildroot/output/host/usr/bin/
TOOLCHAIN=$(TOOLCHAIN_PATH)aarch64-linux-

# Compiler/linker
CC=$(TOOLCHAIN)gcc
LD=$(TOOLCHAIN)gcc
AR=$(TOOLCHAIN)ar

# Flags
CFLAGS+=-Wall -Wextra -g -mcpu=cortex-a53 -O0 -std=gnu11

# Targets
all: app_boot

app_boot: app_boot.o
	$(LD) -o $@ $^

app_boot.o: app_boot.c
	$(CC) $(CFLAGS) -c -o $@ $<

clean:
	rm -f *.o app_boot