KERNEL_VERSION = $(shell uname -r)
KERNEL_MODLIB  = /lib/modules/$(KERNEL_VERSION)
KERNEL_BUILD   = $(KERNEL_MODLIB)/build
KERNEL_SOURCE  = $(KERNEL_MODLIB)/source

SUB=c64x

export CONFIG_DAVINCI_C64X=m

ifeq ($(shell test -e $(KERNEL_BUILD)/include/linux/autoconf.h && echo yes),yes)
  AUTOCONF_H = -include $(KERNEL_BUILD)/include/linux/autoconf.h
endif

ifeq ($(shell test -e $(KERNEL_BUILD)/include/linux/config.h && echo yes),yes)
  CPPFLAGS += -DHAVE_LINUX_CONFIG_H
endif

CPPFLAGS += -DMODULE

.PHONY: all install clean

it all:
	$(MAKE) -C $(KERNEL_BUILD) \
		KCPPFLAGS="$(CPPFLAGS) -I$(shell pwd)/include -I$(KERNEL_BUILD)/include" \
		SUBDIRS=$(shell pwd)/$(SUB) ARCH=arm CROSS_COMPILE=arm-v4t-linux-gnueabi- modules

install: all

clean:
	$(RM) -r $(SUB)/.tmp_versions $(SUB)/Module.symvers
	find $(SUB) -name *.o -o -name *.ko -o -name .*.cmd -o -name *.mod.* | xargs rm -f

so: it
	find $(SUB) -name "*.ko" | xargs strip -x -R .comment -R .note
