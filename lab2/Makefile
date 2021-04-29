  
obj-m :=block_driver.o
all :
	make -C /lib/modules/$(shell uname -r)/build M=$(shell pwd) modules
clean :
	make -C /lib/modules/$(shell uname -r)/build M=$(shell pwd) clean
ins :
	sudo insmod block_driver.ko
rm :
	sudo rmmod block_driver.ko