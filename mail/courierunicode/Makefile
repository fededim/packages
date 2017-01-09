#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=courier-unicode
PKG_VERSION:=1.4
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=@SF/courier
PKG_MD5SUM:=c1bfff8b538339cd7316c273caa4a3b7

#include $(INCLUDE_DIR)/uclibc++.mk   # $(CXX_DEPENDS)
include $(INCLUDE_DIR)/package.mk

define Package/courier-unicode
  SECTION:=mail
  CATEGORY:=Mail
  DEPENDS:=+libstdcpp
  TITLE:=courier-unicode is a library used by most other Courier packages.
  URL:=http://www.courier-mta.org/unicode/
  MAINTAINER:=Federico Di Marco <fededim@gmail.com>
endef



define Package/courier-unicode/description
This library implements several algorithms related to the Unicode Standard:

Look up uppercase, lowercase, and titlecase equivalents of a unicode character.

Implementation of grapheme and work breaking rules.

Implementation of line breaking rules.

Several ancillary functions, like looking up the unicode character that corresponds to some HTML 4.0 entity (such as “&amp;”, for example), and determining the normal width or a double-width status of a unicode character. Also, an adaptation of the iconv(3) API for this unicode library.

Look up the Unicode script property.

Look up the category property.

This library also implements C++ bindings for these algorithms.

endef


define Package/courier-unicode/conffiles
#/etc/courier-unicode.auth
endef


#CONFIGURE_ARGS += \
#	--with-pam=no \
#	--enable-testing=no

#CONFIGURE_VARS += \
#	CXXFLAGS="$$$$CXXFLAGS -fno-rtti"	


define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/{lib,include}
	$(INSTALL_DIR) $(1)/usr/include/courier

	$(CP) $(PKG_BUILD_DIR)/.libs/*so*  $(1)/usr/lib
	$(CP) -L $(PKG_BUILD_DIR)/.libs/*.la  $(1)/usr/lib

	$(INSTALL_DATA) $(PKG_BUILD_DIR)/*.h $(1)/usr/include/courier
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/.libs/*so*  $(1)/usr/lib

#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/enttest $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/graphemetest $(1)/usr/bin
endef


$(eval $(call BuildPackage,courier-unicode))
