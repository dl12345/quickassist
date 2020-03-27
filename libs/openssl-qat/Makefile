#
# Copyright (C) DL <revelstone@yahoo.com>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
# 
#
include $(TOPDIR)/rules.mk

PKG_NAME:=libopenssl-qat
PKG_VERSION:=1
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://github.com/intel/QAT_Engine.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2020-03-15
PKG_SOURCE_VERSION:=d3d5df4b49d2498c71a1a4f2773dcf9224de2065
PKG_MIRROR_HASH:=b0d82c3eb5780e5fc2b8b5ca9f2b404bfa91681727b9f4419e55cd8ae1bd01a5

PKG_BUILD_DEPENDS:=openssl quickassist
PKG_FIXUP:=autoreconf
PKG_REMOVE_FILES:=autogen.sh
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/kernel.mk

define Package/libopenssl-qat
  SECTION:=libs
  CATEGORY:=Libraries
  SUBMENU:=SSL
  TITLE:=Intel Quick Assist OpenSSL Engine
  URL:=https://github.com/intel/QAT_Engine.git
  DEPENDS:=@USE_GLIBC +libopenssl +quickassist
endef

define Package/libopenssl-qat/description
 Intel Quick Assist hardware acceleration engine for OpenSSL
endef

OPENSSL_BASE = usr/lib/
OPENSSL_ENGINES = engines-1.1
QAT_CONFIG = usr/share/quickassist/QAT_Engine/config

CONFIGURE_ARGS += \
	--with-qat_dir=$(KERNEL_BUILD_DIR)/quickassist-1.5 \
	--with-openssl_install_dir=$(STAGING_DIR_ROOT)/usr \
	--enable-qat16_driver \
	--disable-qat_ciphers \
	--disable-qat_auto_engine_init_on_fork

MAKE_VARS += \
	CROSS_COMPILE="$(KERNEL_CROSS)" \
	KERNEL_SOURCE_ROOT="$(LINUX_DIR)" \
	PATH_AUTOCNF="include/generated/autoconf.h" \
	WITH_ICP_TARGET="1" 

define Build/Prepare
	$(call Build/Prepare/Default)
	(cd $(PKG_BUILD_DIR); cp .tools/* . );
endef

define Package/libopenssl-qat/install
	$(INSTALL_DIR) $(1)/$(OPENSSL_BASE)/$(OPENSSL_ENGINES)
	$(INSTALL_DIR) $(1)/$(QAT_CONFIG)
	$(CP) $(PKG_INSTALL_DIR)/usr $(1)
	( cd $(1)/$(OPENSSL_BASE)/$(OPENSSL_ENGINES); ln -s ../libqat.so qat.so)
	$(CP) $(PKG_BUILD_DIR)/qat/config/c2xxx $(1)/$(QAT_CONFIG)
endef

$(eval $(call BuildPackage,libopenssl-qat))
