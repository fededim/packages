#
# Copyright (C) 2007-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=cyrus-imapd
PKG_VERSION:=2.5.10
PKG_RELEASE:=1

PKG_LICENSE:=GPL-2.0+
PKG_LICENSE_FILES:=COPYING

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://www.cyrusimap.org/releases/
PKG_MD5SUM:=b38f4fd72825a298ac47426dcd2a50c8437c2947864ba50d79a9a53fe9845c5f

include $(INCLUDE_DIR)/package.mk

TARGET_CFLAGS += $(TARGET_CPPFLAGS)

define Package/$(PKG_NAME)
  SECTION:=mail
  CATEGORY:=Mail
  DEPENDS:=+libuuid +libopenssl +cyrus-sasl +jansson +libsqlite3 +zlib +libical +libxml2
  TITLE:=Cyrus IMAP is an email, contacts and calendar server.
  URL:=http://www.cyrusimap.org/
  MAINTAINER:=Federico Di Marco <fededim@gmail.com>
endef

define Package/$(PKG_NAME)/description
This is the highlight reel of Cyrus imap’s full list of features.

Security: Cyrus runs on sealed servers, where normal users can’t log in. Users access mail through IMAP/POP or KPOP.
Performance and scalability: The mail spool uses a filesystem layout.
Filtering: Server-side mail filtering via Sieve.
Efficiency, ease of administration: The private mailbox database design gives Cyrus considerable advantages. Multiple concurrent read/write connections to the same mailbox are permitted. The server supports access control lists on mailboxes and storage quotas on mailbox hierarchies.
Beyond Email: Support for CalDAV and CardDAV provides an integrated calendaring and email solution.
Scalability: Cyrus Murder provides horizontal scalability: distributing the load across a pool of servers, without limiting to a particular subset of the IMAP namespace.
Authentication: Supports X.509 PKI auth via STARTTLS and EXTERNAL. Plus all the Cyrus SASL options.
endef

define Package/$(PKG_NAME)/conffiles
/etc/imapd.conf
/etc/cyrus.conf
endef

#CONFIGURE_VARS += \
#	LIBS="$(TARGET_LDFLAGS) -lcrypto -lssl"


#TARGET_LDFLAGS = \
#	-L./com_err/et/.libs/

CONFIGURE_ARGS += \
	--with-sqlite="$(STAGING_DIR)/usr" --with-bdb=no --enable-idled --enable-http --disable-pcre --with-cyrus-prefix=/usr/libexec/cyrus


define Build/Install
	$(MAKE) $(MAKE_FLAGS) -C $(PKG_BUILD_DIR) install DESTDIR=$(PKG_INSTALL_DIR)
	chmod -R u+w $(PKG_INSTALL_DIR)
	rm -fr $(PKG_INSTALL_DIR)/usr/{include,share}
	rm -fr $(PKG_INSTALL_DIR)/usr/lib/*.la $(PKG_INSTALL_DIR)/usr/lib/{pkgconfig,x86_*}
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/{lib,bin}
	$(INSTALL_DIR) $(1)/etc/init.d

	$(CP) $(PKG_BUILD_DIR)/com_err/et/.libs/*.so*  $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/imap/.libs/*.so*  $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/lib/.libs/*.so*  $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/sieve/.libs/*.so*  $(1)/usr/lib

	find  $(PKG_BUILD_DIR)/imap/.libs -perm /a+x -type f -exec $(INSTALL_BIN) {} $(1)/usr/bin \;
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/imtest/.libs/* $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/master/.libs/* $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/notifyd/.libs/* $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/sieve/.libs/{sievec,sieved} $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/timsieved/.libs/timsieved $(1)/usr/bin
	$(RM) $(1)/usr/bin/*.so*

	$(INSTALL_DATA) files/*.conf $(1)/etc/
	$(INSTALL_BIN) files/cyrusimapd.init $(1)/etc/init.d/cyrusimapd
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
