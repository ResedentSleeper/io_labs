obj-m += lab1.o

all:
	make -C /lib/modules/`uname -r`/build M=$(PWD) modules
clean:
	make -C /lib/modules/`uname -r`/build M=$(PWD) clean
	
install:
	sudo insmod lab1.ko
	
remove:
	sudo rmmod lab1