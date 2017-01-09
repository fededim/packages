#
# Copyright (C) 2007-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=cyrus-sasl
PKG_VERSION:=2.1.26
PKG_RELEASE:=1

PKG_LICENSE:=GPL-2.0+
PKG_LICENSE_FILES:=COPYING

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://www.cyrusimap.org/releases/
PKG_MD5SUM:=8fbc5136512b59bb793657f36fadda6359cae3b08f01fd16b3d406f1345b7bc3

PKG_BUILD_DEPENDS:=cyrus-sasl/host

MAKEMD5:=$(STAGING_DIR_HOST)/share/cyrus-sasl/makemd5


include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk

TARGET_CFLAGS += $(TARGET_CPPFLAGS)

# Prevent calling upstream configure
define Host/Configure
endef

define Host/Compile
	# Build include/makemd5 using host compiler and let it generate makemd5 executable
	CC="$(HOSTCC)" $(MAKE) -C $(HOST_BUILD_DIR)/include makemd5
endef

define Host/Install
	$(INSTALL_DIR) $(STAGING_DIR_HOST)/share/cyrus-sasl
	$(INSTALL_BIN) $(HOST_BUILD_DIR)/include/makemd5 $(MAKEMD5)
endef


define Package/$(PKG_NAME)
  SECTION:=mail
  CATEGORY:=Mail
  DEPENDS:=+libopenssl
  TITLE:=Cyrus SASL is a library of authentication mechanisms.
  URL:=http://www.cyrusimap.org/
  MAINTAINER:=Federico Di Marco <fededim@gmail.com>
endef

define Package/$(PKG_NAME)/description
Simple Authentication and Security Layer (SASL) is a specification that describes how authentication mechanisms can be plugged into an application protocol on the wire. Cyrus SASL is an implementation of SASL that makes it easy for application developers to integrate authentication mechanisms into their application in a generic way. 

Cyrus SASL provides a number of authentication plugins out of the box.

Berkeley DB, GDBM, or NDBM (sasldb), PAM, MySQL, PostgreSQL, SQLite, LDAP, Active Directory(LDAP), DCE, Kerberos 4 and 5, proxied IMAP auth, getpwent, shadow, SIA, Courier Authdaemon, httpform, APOP and SASL mechanisms: ANONYMOUS, CRAM-MD5, DIGEST-MD5, EXTERNAL, GSSAPI, LOGIN, NTLM, OTP, PASSDSS, PLAIN, SR
endef

define Package/$(PKG_NAME)/conffiles
/etc/saslauthd.conf
endef

#CONFIGURE_VARS += \
#	LIBS="$(TARGET_LDFLAGS) -lcrypto -lssl"

CONFIGURE_ARGS += \
	--with-dblib=no --enable-login --enable-ntlm --sysconfdir=/etc

define Build/Configure
	# Fetch prebuilt makemd5 from staging dir
	$(INSTALL_DIR) $(PKG_BUILD_DIR)/include
	$(CP) $(MAKEMD5) $(PKG_BUILD_DIR)/include

	# Portably set makemd5 modtime to one day in the future
	# to prevent rebuilding it
	perl -e 'utime(time() + 86400, time() + 86400, $$$$ARGV[0])' \
		$(PKG_BUILD_DIR)/include/makemd5

	$(call Build/Configure/Default)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/{lib,include}
	$(INSTALL_DIR) $(1)/usr/include/sasl

	$(CP) $(PKG_BUILD_DIR)/lib/.libs/*so*  $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/plugins/.libs/*so*  $(1)/usr/lib

	$(CP) -L $(PKG_BUILD_DIR)/lib/.libs/*.la  $(1)/usr/lib
	$(CP) -L $(PKG_BUILD_DIR)/plugins/.libs/*.la  $(1)/usr/lib
	$(CP) -L $(PKG_BUILD_DIR)/sasldb/.libs/*.la  $(1)/usr/lib

	$(INSTALL_DATA) $(PKG_BUILD_DIR)/include/*.h $(1)/usr/include/sasl
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/lib/*.h $(1)/usr/include/sasl
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/plugins/*.h $(1)/usr/include/sasl
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/saslauthd/*.h $(1)/usr/include/sasl
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/sasldb/*.h $(1)/usr/include/sasl
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/utils/*.h $(1)/usr/include/sasl
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/etc/init.d

	$(INSTALL_DIR) $(1)/usr/{lib,bin}
	$(CP) $(PKG_BUILD_DIR)/lib/.libs/*so*  $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/plugins/.libs/*so*  $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/saslauthd/*saslauthd $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/utils/.libs/* $(1)/usr/bin

	$(INSTALL_BIN) files/cyrussasl.init $(1)/etc/init.d/cyrussasl
endef




$(eval $(call BuildPackage,$(PKG_NAME)))
$(eval $(call HostBuild))
