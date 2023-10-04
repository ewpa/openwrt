# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024 Toco Technologies <info@toco.ae>
#
include $(TOPDIR)/rules.mk

ARCH:=riscv64
BOARD:=cv18x0
BOARDNAME:=Cvitek CV180x boards
FEATURES:=ext4
KERNELNAME:=Image dtbs
SUBTARGETS:=generic

KERNEL_PATCHVER:=6.6

include $(INCLUDE_DIR)/target.mk

define Target/Description
	Build firmware images for the Cvitek CV1800/1810 boards
endef

$(eval $(call BuildTarget))
