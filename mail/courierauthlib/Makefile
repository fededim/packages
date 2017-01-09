#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=courier-authlib
PKG_VERSION:=0.66.4
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=@SF/courier
PKG_MD5SUM:=f759d092ee80f25ae6eefb4c1b9ee7e9

#include $(INCLUDE_DIR)/uclibc++.mk   # $(CXX_DEPENDS)
include $(INCLUDE_DIR)/package.mk

define Package/courier-authlib
  SECTION:=mail
  CATEGORY:=Mail
  DEPENDS:=+courier-unicode +libltdl
  TITLE:=courier-authlib provides a generic authentication API to validate account passwords
  URL:=http://www.courier-mta.org/authlib/
  MAINTAINER:=Federico Di Marco <fededim@gmail.com>
endef



define Package/courier-authlib/description
The Courier authentication library provides alternative implementations of these authentication services:

Use the traditional system password files: /etc/passwd and /etc/shadow, possibly in conjunction with the PAM library.
Maintain all this information in a GDBM or a DB database. The GDBM or the DB database is compiled from plain text files. Perl scripts provide a simple interface for creating and editing the authentication information, then a script compiles the plain text files into a database.
Use an LDAP server for authentication.
Use a table in a MySQL database for authentication.
Use a table in a PostgreSQL database for authentication.
Use a table in an SQLite file for authentication.
All Courier components that use this authentication library, therefore, will be able to authenticate E-mail accounts using any of the above methods.

endef


define Package/courier-authlib/conffiles
endef


TARGET_CFLAGS += -I$(STAGING_DIR)/usr/include/courier

CONFIGURE_ARGS += \
	--with-mailuser=daemon --with-mailgroup=daemon --with-locking-method=fcntl --without-authuserdb --without-authsqlite --without-authmysql --without-authpgsql

#CONFIGURE_VARS += \
#	CXXFLAGS="$$$$CXXFLAGS -fno-rtti"	


define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/{lib,include,bin}
	$(INSTALL_DIR) $(1)/usr/include/courier

	$(CP) $(PKG_BUILD_DIR)/.libs/*so*  $(1)/usr/lib
	$(CP) -L $(PKG_BUILD_DIR)/.libs/*.la  $(1)/usr/lib
	$(CP) -L $(PKG_BUILD_DIR)/userdb/.libs/*.la  $(1)/usr/lib

	$(INSTALL_DATA) $(PKG_BUILD_DIR)/*.h $(1)/usr/include/courier
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/courierauthconfig $(1)/usr/bin

endef


define Package/courier-authlib/install
	$(INSTALL_DIR) $(1)/usr/{lib,bin}
	$(INSTALL_DIR) $(1)/etc/init.d

	$(CP) $(PKG_BUILD_DIR)/.libs/*so*  $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/{authdaemondprog,authenumerate,authpasswd,authtest} $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/userdb/userdbpw $(1)/usr/bin

#	$(INSTALL_DATA) files/$(PKG_NAME).auth $(1)/etc/
	$(INSTALL_BIN) files/authdaemondprog.init $(1)/etc/init.d/authdaemondprog
endef


$(eval $(call BuildPackage,courier-authlib))
