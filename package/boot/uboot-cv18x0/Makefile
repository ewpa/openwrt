# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024 OpenWrt.org
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_RELEASE:=1
PKG_VERSION:=2024.07-rc5
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/u-boot/u-boot
PKG_SOURCE_VERSION:=9e9f78f7aa0124ef0e622532043acf87e84008dc

UBOOT_USE_INTREE_DTC:=1

include $(INCLUDE_DIR)/u-boot.mk
include $(INCLUDE_DIR)/package.mk

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.zst
PKG_MIRROR_HASH:=90bee1daad0ac6bfffbcecdff672a8a0d325dc4786a6d3cbefb6def423ed8983

define U-Boot/Default
  BUILD_TARGET:=cv18x0
  BUILD_DEVICES=$(1)
  UBOOT_IMAGE:=u-boot.bin
  DTS_DIR:=arch/riscv/dts
  UENV:=default
  DEFAULT:=y
endef

define U-Boot/milkv_duo
  NAME:=MilkV Duo
  OPENSBI:=generic
  DEPENDS:=+cv18xxlibs
  UBOOT_DTS:=cv1800b-milkv-duo.dtb
  BUILD_DEVICES:=milkv_duo
endef

UBOOT_TARGETS := \
	milkv_duo

#UBOOT_MAKE_FLAGS += \
#        OPENSBI=$(STAGING_DIR_IMAGE)/fw_dynamic-${OPENSBI}.bin

define Build/Configure
	$(call Build/Configure/U-Boot)
	sed -i 's/CONFIG_TOOLS_LIBCRYPTO=y/# CONFIG_TOOLS_LIBCRYPTO is not set/' $(PKG_BUILD_DIR)/.config
endef

define Build/InstallDev
	$(INSTALL_DIR) $(STAGING_DIR_IMAGE)
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(UBOOT_IMAGE) $(STAGING_DIR_IMAGE)/$(BUILD_VARIANT)-$(UBOOT_IMAGE)
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(DTS_DIR)/$(UBOOT_DTS) $(STAGING_DIR_IMAGE)/$(UBOOT_DTS)
endef

$(eval $(call BuildPackage/U-Boot))
