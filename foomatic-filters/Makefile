#
# Copyright (C) 2007-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=foomatic-filters
PKG_VERSION:=4.0.17
PKG_RELEASE:=1

PKG_SOURCE:=distrotech-$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/Distrotech/foomatic-filters/archive/refs/tags/
PKG_HASH:=d2e507f9de4d6806097e97cf353e40b76544c6dbee5574ff30b27e6903b4c9de

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/autotools.mk

# 欺骗 configure，使其认为系统已存在这些文本转 PS 工具
CONFIGURE_VARS += \
	A2PS="/bin/true" \
	ENSCRIPT="/bin/true" \
	MPAGE="/bin/true" \
	TEXTTOPS="/bin/true"

define Package/foomatic-filters
  PROVIDES:=foomatic-rip
  SECTION:=net
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

$(eval $(call BuildPackage,foomatic-filters))
