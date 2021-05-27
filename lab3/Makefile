obj-m :=virt_net_if.o
all :
	make -C /lib/modules/$(shell uname -r)/build M=$(shell pwd) modules
clean :
	make -C /lib/modules/$(shell uname -r)/build M=$(shell pwd) clean
load :
	sudo insmod virt_net_if.ko
unload :
	sudo rmmod virt_net_if