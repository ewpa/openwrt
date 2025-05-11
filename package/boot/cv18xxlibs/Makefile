# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024 OpenWrt.org
#

include $(TOPDIR)/rules.mk

PKG_NAME:=cv18xxlibs
#PKG_RELEASE:=1.4

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL=https://github.com/milkv-duo/milkv-duo-buildroot-libraries
PKG_SOURCE_DATE:=2024-05-12
PKG_SOURCE_VERSION:=f359994bd497f942bb67734280d81f6640c7c168
PKG_MIRROR_HASH:=59a13cd9ca2424b771599d3cf77c2ba02fd74dd880ffba53fadb15850a1bd432

#PKG_BUILD_DIR=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)

PKG_TARGETS:=bin
PKG_FLAGS:=nonshared
#PKG_LICENSE:=BSD-2-Clause
#PKG_LICENSE_FILES:=COPYING.BSD
#PKG_BUILD_PARALLEL:=1

#PKG_MAINTAINER:=Zoltan HERPAI <wigyori@uid0.hu>

include $(INCLUDE_DIR)/package.mk

define Package/cv18xxlibs
    SECTION:=boot
    CATEGORY:=Boot Loaders
    DEPENDS:=@(TARGET_cv18x0)
    URL:=https://github.com/milkv-duo/milkv-duo-buildroot-libraries
#    VARIANT:=$(subst _,/,$(subst opensbi_,,$(1)))
    TITLE:=CV18xx libraries (BL2, chipconf)
endef

export GCC_HONOUR_COPTS=s

MAKE_VARS = \
	CROSS_COMPILE="$(TARGET_CROSS)"

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/firmware \
		ARCH=riscv BOOT_CPU=riscv CHIP_ARCH=cv180x \
		PROJECT_FULLNAME=cv1800b_milkv_duo_sd FREE_RAM_SIZE=64MB \
		CROSS_COMPILE=$(TARGET_CROSS) \
		bl2
endef

define Build/InstallDev
	$(INSTALL_DIR) $(STAGING_DIR_IMAGE)
	$(CP) $(PKG_BUILD_DIR)/firmware/build/cv180x/bl2.bin $(STAGING_DIR_IMAGE)/bl2_cv1800b_milkv_duo_sd.bin
	$(CP) $(PKG_BUILD_DIR)/firmware/plat/cv180x/chip_conf.bin $(STAGING_DIR_IMAGE)/cv180x_chip_conf.bin
	$(CP) $(PKG_BUILD_DIR)/firmware/plat/cv180x/multi.its $(STAGING_DIR_IMAGE)/cv180x_multi.its
	$(CP) $(PKG_BUILD_DIR)/firmware/test/cv181x/ddr_param.bin $(STAGING_DIR_IMAGE)/cv181x_ddr_param.bin
	$(CP) $(PKG_BUILD_DIR)/firmware/plat/cv180x/fiptool.py $(STAGING_DIR_HOST)/bin/
endef

$(eval $(call BuildPackage,cv18xxlibs))
