# Copyright (c) 2014 The Linux Foundation. All rights reserved.
#
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/image.mk

define Device/Default
	PROFILES := Default
	KERNEL_DEPENDS = $$(wildcard $(DTS_DIR)/$$(DEVICE_DTS).dts)
	KERNEL_LOADADDR = 0x42208000
	DEVICE_DTS = $$(SOC)-$(lastword $(subst _, ,$(1)))
	DEVICE_DTS_CONFIG := config@1
	IMAGES := sysupgrade.bin
	IMAGE/sysupgrade.bin = sysupgrade-tar | append-metadata
	IMAGE/sysupgrade.bin/squashfs :=
endef

define Device/LegacyImage
	KERNEL_SUFFIX := -uImage
	KERNEL = kernel-bin | append-dtb | uImage none
	KERNEL_NAME := zImage
endef

define Device/FitImage
	KERNEL_SUFFIX := -fit-uImage.itb
	KERNEL = kernel-bin | gzip | fit gzip $$(DTS_DIR)/$$(DEVICE_DTS).dtb
	KERNEL_NAME := Image
endef

define Device/FitImageLzma
	KERNEL_SUFFIX := -fit-uImage.itb
	KERNEL = kernel-bin | lzma | fit lzma $$(DTS_DIR)/$$(DEVICE_DTS).dtb
	KERNEL_NAME := Image
endef

define Device/UbiFit
	KERNEL_IN_UBI := 1
	IMAGES := nand-factory.bin nand-sysupgrade.bin
	IMAGE/nand-factory.bin := append-ubi
	IMAGE/nand-sysupgrade.bin := sysupgrade-tar | append-metadata
endef

define Device/extreme_ap3935i
	$(call Device/LegacyImage)
	$(call Device/UbiFit)
	DEVICE_VENDOR := Extreme Networks
	DEVICE_MODEL := AP3935i
	SOC := qcom-ipq8068
	PAGESIZE := 2048
	BLOCKSIZE := 128k
	KERNEL = kernel-bin | append-dtb | uImage none | append-uImage-fakehdr filesystem
	KERNEL_LOADADDR = 0x41408000
	KERNEL_SIZE := 14400k
	KERNEL_SUFFIX := -uImage
	UBINIZE_OPTS := -E 5
	DEVICE_PACKAGES := ath10k-firmware-qca99x0-ct \
	 ath10k-board-qca99x0-2g-high ath10k-board-qca99x0-5g-high
endef
TARGET_DEVICES += extreme_ap3935i

include $(SUBTARGET).mk

$(eval $(call BuildImage))
