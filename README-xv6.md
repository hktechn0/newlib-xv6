# Newlib for xv6

## configure
You should use 32bit compiler. 
(I can't compile gcc-multilib which is switch -m32)

Example (on x86_64)

    /path/to/newlib/configure --target=i686-elf-xv6 \
    CC_FOR_TARGET=i686-elf-gcc AS_FOR_TARGET=i686-elf-as LD_FOR_TARGET=i686-elf-ld \
    AR_FOR_TARGET=i686-elf-ar RANLIB_FOR_TARGET=i686-elf-ranlib

## How to use

    gcc -fno-pic -static -static-libgcc -nostartfiles -nostdlib -ffreestanding -nodefaultlibs -fno-builtin -m32 -fno-omit-frame-pointer -fno-stack-protector -I/path/to/newlib/newlib/libc/include -o test.o test.c
    gcc -fno-pic -static -static-libgcc -nostartfiles -nostdlib -ffreestanding -nodefaultlibs -fno-builtin -m32 -e main -Ttext 0 -L/path/to/build/i686-elf-xv6/newlib/ -L/path/to/build/i686-elf-xv6/libgloss/libnosys -lc -lm -lnosys -o test test.o

## Notes

The xv6 file system don't support big files (> 70KB) by default.
Almost newlib linked binary size will be above 100KB.

Solutions

1. Static link the binary (use `ld -b binary` switch) to kernel, and modified exec().
2. Modify the file system. (i-node, block size...)

I was execute [mruby](https://github.com/mruby/mruby) binary successful by solution 1.
