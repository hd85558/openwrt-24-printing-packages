#
# Copyright (C) 2025 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=foomatic-filters
PKG_VERSION:=distrotech-foomatic-filters-4.0.17
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_VERSION).tar.gz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/Distrotech/$(PKG_NAME)/archive/refs/tags/
PKG_HASH:=d2e507f9de4d6806097e97cf353e40b76544c6dbee5574ff30b27e6903b4c9de

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1
CONFIGURE_ARGS += --disable-texttops
include $(INCLUDE_DIR)/package.mk
$(call include_mk, autotools.mk)

define Package/foomatic-filters
  PROVIDES:=foomatic-rip
  SECTION:=Net
  CATEGORY:=Network
  TITLE:=Foomatic printer filter and database tools
  URL:=https://github.com/Distrotech/foomatic-filters
  DEPENDS:=+cups
  SUBMENU:=Printing
endef

define Package/foomatic-filters/description
 Foomatic is a database-driven system for integrating free software printer
 drivers with common spoolers under Unix. This package contains the filter
 binaries and support scripts used by CUPS and other spoolers to process print
 jobs for a wide variety of printers.
endef

define Build/Configure
	$(call Build/Configure/Default)
endef

define Build/Compile
	$(call Build/Compile/Default)
endef


define Package/foomatic-filters/install
	$(INSTALL_DIR) $(1)
	$(MAKE) -C $(PKG_BUILD_DIR) \
		DESTDIR=$(1) \
		install-cups
endef

define Package/postinst
#!/bin/sh
/etc/init.d/cupsd stop
/etc/init.d/cupsd start
endef


$(eval $(call BuildPackage,foomatic-filters))
