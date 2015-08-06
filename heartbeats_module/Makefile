obj-m := heartbeat.o
#obj-m := heartbeat.o
PWD := $(shell pwd)

ARCH=arm
CROSS_COMPILE=arm-eabi-

KERNEL_DIR := /home/casionwoo/odroid_XU3/linux/linux

all:
	    make -C $(KERNEL_DIR) M=$(PWD)  modules
clean:
	    make -C $(KERNEL_DIR) M=$(PWD) clean
