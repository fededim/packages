#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=courier
PKG_VERSION:=0.76.3
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=@SF/courier
PKG_MD5SUM:=caac8e3aff3a1edbc8c0aef0a2b08ea1

#PKG_BUILD_DEPENDS:=+perl

#include $(INCLUDE_DIR)/uclibc++.mk   # $(CXX_DEPENDS)
include $(INCLUDE_DIR)/package.mk

define Package/courier
  SECTION:=mail
  CATEGORY:=Mail
  DEPENDS:=+libopenssl +libpcre +courier-authlib +libidn +libgdbm
  TITLE:=Courier provides ESMTP, IMAP, POP3, webmail, and mailing list services.
  URL:=http://www.courier-mta.org/
  MAINTAINER:=Federico Di Marco <fededim@gmail.com>
endef



define Package/courier/description
The Courier mail transfer agent (MTA) is an integrated mail/groupware server based on open commodity protocols, such as ESMTP, IMAP, POP3, LDAP, SSL, and HTTP. Courier provides ESMTP, IMAP, POP3, webmail, and mailing list services within a single, consistent, framework. Individual components can be enabled or disabled at will. The Courier mail server now implements basic web-based calendaring and scheduling services integrated in the webmail module. Advanced groupware calendaring services will follow soon.

The Courier mail server evolved out of several related projects, that merged together (more on that later). The Courier mail server implements SMTP extensions for mailing list management and spam filtering. The Courier mail server can function as an intermediate mail relay, relaying mail between an internal LAN and the Internet, or perform final delivery to mailboxes. The Courier mail server uses maildirs as its native mail storage format, but it can also deliver mail to legacy mailbox files as well. The Courier mail server's configuration is set by plain text files and Perl scripts. Most of The Courier mail server's configuration can now be adjusted from a web browser, using The Courier mail server's web-based administration module.

The Courier mail server can provide mail services for regular operating system accounts. The Courier mail server can also provide mail services for virtual mail accounts, managed by an LDAP, MySQL, or PostgreSQL-based authentication database.

endef


define Package/courier/conffiles
endef


# configure generates an header file called dbobj.h in build directory which calls either gbdb or bdb functions according to what is installed and must be included
TARGET_CFLAGS += -I$(PKG_BUILD_DIR) -I$(STAGING_DIR)/usr/include/courier

CONFIGURE_ARGS += \
	--with-locking-method=fcntl --with-waitfunc=wait --with-mailuid=1 --with-mailgid=1 \
	--with-mailuser=daemon --with-mailgroup=daemon --sysconfdir=/etc/courier  --datadir=/usr/share/courier --libexecdir=/usr/libexec --datarootdir=/usr/share/courier

CONFIGURE_VARS += \
	COURIERAUTHCONFIG=$(STAGING_DIR)/usr/bin


define Build/Configure
        $(Build/Configure/Default)
#	unluckily there is a copy of bdbobj.h inside libs/bdbobj which is not updated by configure so we have to force to update it manually
#	$(CP) $(PKG_BUILD_DIR)/dbobj.h $(PKG_BUILD_DIR)/libs/bdbobj/bdbobj.h
endef

define Build/Install
	$(MAKE) $(MAKE_FLAGS) -C $(PKG_BUILD_DIR) install DESTDIR=$(PKG_INSTALL_DIR)
	mv $(PKG_INSTALL_DIR)/usr/libexec/filters $(PKG_INSTALL_DIR)/usr/libexec/courier/filters
	chmod -R u+w $(PKG_INSTALL_DIR)
	rm -fr $(PKG_INSTALL_DIR)/var
endef

define Package/courier/install
	$(INSTALL_DIR) $(1)/usr/{bin,libexec,share}
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/var

	$(CP) $(PKG_INSTALL_DIR)/* $(1)

#	$(INSTALL_DATA) files/$(PKG_NAME).auth $(1)/etc/
#	$(INSTALL_BIN) files/$(PKG_NAME).init $(1)/etc/init.d/$(PKG_NAME)

#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libs/imap/{imapd,imaplogin,pop3d,pop3login} $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libs/ldapaddressbook/ldapsearch $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libs/liblock/lockmail $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libs/maildir/{deliverquota,maildiracl,maildirkw,maildirmake} $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libs/maildrop/{mailbot,maildrop,reformail} $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libs/makedat/{makedatprog} $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libs/pcp/{pcp,pcpd} $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libs/rfc2045/{headercheck,makemime,reformmime,} $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libs/sqwebmail/{ldapsearch,showmsg2html,sqwebmail,sqwebmaild,sqwebpasswd} $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libs/tcpd/{couriertcpd,couriertls} $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/courier/{aliascombine,aliascreate,aliasexp,aliaslookup,cancelmsg,courier,courier-config,courierd} $(1)/usr/bin

endef


$(eval $(call BuildPackage,courier))
