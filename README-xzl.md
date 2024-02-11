how to create fat32 disk imgs for test file/dir funcs

use mtool, does NOT require root. 

NB: mtool will refuse to create FAT32 for disk <=32MB. therefore create a 64MB disk img
0C_directory ex seems only to work with FAT32 (not FAT16)

# cr disk
dd if=/dev/zero of=disk.img bs=1M count=64

# format, -F forces to FAT32
mformat -F -i disk.img ::

# create dir
mmd -i disk.img ::subdir1
mmd -i disk.img ::subdir2
mmd -i disk.img ::subdir3

# copy files into img
mcopy -i disk.img README.md ::hello.md

# disk.img can be renamed as test.dd for 0C_directory


RELATED -- QEMU CAN PRESENT A  HOST DIR AS A FAT23 IMAGE...

qemu-system-aarch64 -M raspi3 -kernel kernel8.img -drive file=fat:rw:${PWD},if=sd,format=raw -serial stdio

https://en.wikibooks.org/wiki/QEMU/Devices/Storage#:~:text=Qemu%20can%20emulate%20a%20virtual,rw%3A%20to%20the%20aforementioned%20prefix.

but 0C_directory ex seems not recognizing the partition type. maybe a limitation. 

ERROR: Wrong partition type
FAT partition not found???
qemu-system-aarch64: terminating on signal 2
