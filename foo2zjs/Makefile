#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=foo2zjs
PKG_VERSION:=20200505dfsg0-3
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).zip
#PKG_SOURCE_URL:=https://github.com/OpenPrinting/foo2zjs/archive/refs/tags/debian/
#PKG_HASH:=061749f0066c17372e36b6d714871ef643083bd8d77cc55aa259e372e62af17e
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PATCH_DIR:=./patches

include $(INCLUDE_DIR)/package.mk

define Package/foo2zjs
	SECTION:=net
	CATEGORY:=Network
	TITLE:=A linux printer driver for QPDL protocol
	URL:=https://github.com/koenkooi/foo2zjs
	DEPENDS:=+ghostscript +libjbigkit +cups +foomatic-rip
	SUBMENU:=Printing
endef

define Package/foo2zjs/description
	foo2zjs is a linux printer driver for QPDL protocol and other related printer streams.
	It converts pbm (B/W) images and cmyk images to printer-specific formats.
endef

define Build/Prepare
	# 创建构建目录
	mkdir -p $(PKG_BUILD_DIR)
	# 手动下载源码到 dl/ 目录，使用 PKG_SOURCE 指定的文件名（加入 -L 适应 GitHub 重定向）
	@echo "Downloading foo2zjs source..."
	@if [ ! -f $(DL_DIR)/$(PKG_SOURCE) ]; then \
		curl -fL -o $(DL_DIR)/$(PKG_SOURCE) \
			--connect-timeout 20 --retry 5 \
			https://github.com/OpenPrinting/foo2zjs/archive/refs/tags/debian/$(PKG_VERSION).zip || \
		( echo "Download failed"; rm -f $(DL_DIR)/$(PKG_SOURCE); exit 1 ); \
	else \
		echo "Using existing $(DL_DIR)/$(PKG_SOURCE)"; \
	fi
	# 解压到构建目录
	unzip -q -d $(PKG_BUILD_DIR) $(DL_DIR)/$(PKG_SOURCE)
	# 使用通配符匹配解压后的顶层目录，防止硬编码导致找不到路径
	mv $(PKG_BUILD_DIR)/foo2zjs-debian-*/* $(PKG_BUILD_DIR)/
	rmdir $(PKG_BUILD_DIR)/foo2zjs-debian-*
	# --- 应用 patches 目录下的所有补丁文件 ---
	$(call PatchDir,$(PKG_BUILD_DIR),$(PATCH_DIR))
endef

define Build/Compile
	$(call Build/Compile/Default)
	$(MAKE) -C $(PKG_BUILD_DIR)/icc2ps \
		CC="$(TARGET_CC)" \
		CFLAGS="$(TARGET_CFLAGS)" \
		LDFLAGS="$(TARGET_LDFLAGS)" \
		all
endef

define Package/foo2zjs/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/share/cups/model
	$(INSTALL_DIR) $(1)/usr/lib/cups/filter
	$(INSTALL_DIR) $(1)/usr/share/foo2zjs
#	$(MAKE) -C $(PKG_BUILD_DIR) DESTDIR=$(1) install-test
	$(MAKE) -C $(PKG_BUILD_DIR) DESTDIR=$(1) install-prog
#	$(MAKE) -C $(PKG_BUILD_DIR) DESTDIR=$(1) install-icc2ps
	$(MAKE) -C $(PKG_BUILD_DIR)/icc2ps DESTDIR=$(1) install
	$(MAKE) -C $(PKG_BUILD_DIR) DESTDIR=$(1) install-crd
	$(MAKE) -C $(PKG_BUILD_DIR) DESTDIR=$(1) install-foo
	$(MAKE) -C $(PKG_BUILD_DIR) DESTDIR=$(1) install-ppd
	$(MAKE) -C $(PKG_BUILD_DIR) DESTDIR=$(1) install-filter
endef

define Package/postinst
#!/bin/sh
/etc/init.d/cupsd stop
/etc/init.d/cupsd start
endef

$(eval $(call BuildPackage,foo2zjs))
