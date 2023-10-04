# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024 Toco Technologies <info@toco.ae>
#
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/image.mk

FAT32_BLOCK_SIZE=1024
FAT32_BLOCKS=$(shell echo $$(($(CONFIG_CV18X0_SD_BOOT_PARTSIZE)*1024*1024/$(FAT32_BLOCK_SIZE))))

KERNEL_LOADADDR:=0x80200000

define Build/riscv-sdcard
	rm -f $@.boot #$(KDIR_TMP)/$(IMG_PREFIX)-$(PROFILE)-boot.img
	mkfs.fat $@.boot  -C $(FAT32_BLOCKS)

	$(STAGING_DIR_HOST)/bin/fiptool --fsbl $(STAGING_DIR_IMAGE)/bl2_cv1800b_milkv_duo_sd.bin \
		--opensbi $(STAGING_DIR_IMAGE)/fw_dynamic-generic.bin \
		--uboot $(STAGING_DIR_IMAGE)/$(UBOOT)-u-boot.bin \
		--ddr_param $(STAGING_DIR_IMAGE)/$(DDRPARAM) \
		--rtos /dev/null \
		$(STAGING_DIR_IMAGE)/fip_milkv_duo.bin

	mcopy -i $@.boot $(STAGING_DIR_IMAGE)/fip_milkv_duo.bin ::fip.bin
	mcopy -i $@.boot $(IMAGE_KERNEL) ::sd.boot

	./gen_cv18x0_sdcard_img.sh \
		$@ \
		$@.boot \
		$(IMAGE_ROOTFS) \
		$(CONFIG_CV18X0_SD_BOOT_PARTSIZE) \
		$(CONFIG_TARGET_ROOTFS_PARTSIZE)
endef

define Device/Default
  PROFILES := Default
  KERNEL_NAME := Image
  DTS_DIR := $(DTS_DIR)/sophgo
  DEVICE_DTS := $$(SOC)-$(subst _,-,$(1))
  IMAGES := sdcard.img.gz
  IMAGE/sdcard.img.gz := riscv-sdcard | append-metadata | gzip
endef

define Device/FitImageLzma
	KERNEL_SUFFIX := -fit-uImage.itb
	KERNEL = kernel-bin | lzma | fit lzma $$(DTS_DIR)/$$(DEVICE_DTS).dtb
	KERNEL_NAME := Image
endef

define Device/milkv_duo
  $(call Device/FitImageLzma)
  DEVICE_VENDOR := MilkV
  DEVICE_MODEL := Duo
  SOC := cv1800b
  DEVICE_DTS := $$(SOC)-milkv-duo
  DEVICE_PACKAGES := opensbi_generic
  UBOOT := milkv_duo
  DDRPARAM := cv181x_ddr_param.bin
endef
TARGET_DEVICES += milkv_duo

$(eval $(call BuildImage))
