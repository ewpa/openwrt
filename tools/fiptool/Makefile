#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=fiptool
PKG_VERSION:=2024-05-23

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/sophgo/fiptool
PKG_SOURCE_DATE:=2024-05-23
PKG_SOURCE_VERSION:=7fb26aa36f75dbea331b74f2857af864f187d800
PKG_MIRROR_HASH:=0e8562d3b0c65f4637b618d5214012c1c2956a6759d9494280eb5fc3a02ca4a4

include $(INCLUDE_DIR)/host-build.mk

define Host/Compile
endef

define Host/Install
	$(INSTALL_BIN) $(HOST_BUILD_DIR)/fiptool $(STAGING_DIR_HOST)/bin/fiptool
endef

$(eval $(call HostBuild))
