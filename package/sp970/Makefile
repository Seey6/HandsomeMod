##############################################
# 
# OpenWrt Makefile for sp970 program
#
##############################################
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME := sp970
PKG_VERSION := 0.1
PKG_RELEASE := 1
PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)
STRIP:=:

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
  SECTION:=firmware
  CATEGORY:=SP970 Package
  SUBMENU:=SP970 modules
  TITLE:=This is SP970 Package
  MAINTAINER:=Seey6
endef

define Package/$(PKG_NAME)/description
	If you can't figure out what this program does, you're probably
	brain-dead and need immediate medical attention.
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./* $(PKG_BUILD_DIR)/
endef

define Build/Configure
endef

define Build/Compile
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_DIR) $(1)/lib/firmware
	$(INSTALL_DIR) $(1)/lib/firmware/wlan/prima

	$(INSTALL_BIN) $(PKG_BUILD_DIR)/sim/sim $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/sim/simservice $(1)/etc/init.d/

	$(INSTALL_DATA) $(PKG_BUILD_DIR)/firmware/mba.mbn $(1)/lib/firmware/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/firmware/modem.* $(1)/lib/firmware/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/firmware/wcnss.* $(1)/lib/firmware/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/firmware/nv.bin  $(1)/lib/firmware/wlan/prima/WCNSS_qcom_wlan_nv.bin
endef

define Package/$(PKG_NAME)/preinst
	#!/bin/bash
	echo 'installing $(PKG_NAME)'
endef

define Package/$(PKG_NAME)/postinst
	#!/bin/sh
	# check if we are on real system
	if [ -z "$${IPKG_INSTROOT}" ]; then
		echo "Enabling rc.d symlink for $(PKG_NAME)"
		/etc/init.d/simservice enable
	fi

	echo '$(PKG_NAME) installed successed !'
	exit 0
endef

define Package/$(PKG_NAME)/prerm
	#!/bin/sh
	# check if we are on real system
	if [ -z "$${IPKG_INSTROOT}" ]; then
		echo "Removing rc.d symlink for $(PKG_NAME)"
		/etc/init.d/simservice disable
	fi
	echo 'removeing $(PKG_NAME)'
	exit 0
endef

define Package/$(PKG_NAME)/postrm
	#!/bin/bash
	echo '$(PKG_NAME) remove successed !'
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
