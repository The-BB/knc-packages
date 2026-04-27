include $(TOPDIR)/rules.mk

PKG_NAME:=perl-mail-spamassassin
PKG_RELEASE:=1
PKG_VERSION:=4.0.1
PKG_HASH:=5c6bb222e18405f1a276816d04e1ffc5cc90785e1265714b4506c2b541d6d5e5

PKG_SOURCE_NAME:=Mail-SpamAssassin
PKG_SOURCE_URL:=@APACHE/spamassassin/source
PKG_SOURCE:=$(PKG_SOURCE_NAME)-$(PKG_VERSION).tar.gz
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_SOURCE_NAME)-$(PKG_VERSION)
PKG_MAINTAINER:=Daniel Golle <daniel@makrotopia.org>
PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE
PKG_CPE_ID:=cpe:/a:apache:spamassassin

PKG_BUILD_DEPENDS:=perl-dbi/host perl-html-parser/host perl-net-dns/host perl-netaddr-ip/host
PKG_INSTALL:=1
PKG_SOURCE_VERSION:=$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include ../perlmod.mk

CONFIGURE_PATH:=spamc

define Package/spamassassin
  SECTION:=mail
  CATEGORY:=Mail
  TITLE:=SpamAssassin
  URL:=https://spamassassin.apache.org/
  DEPENDS:=perl +perlbase-autoloader +perlbase-config +perlbase-data +perlbase-digest \
          +perlbase-encode +perlbase-essential +perlbase-file +perlbase-getopt \
          +perlbase-hash +perlbase-mime +perlbase-net +perlbase-socket \
          +perl-dbi +perl-html-parser +perl-net-dns +perl-netaddr-ip
  VARIANT:=ssl
endef

define Package/spamc/Default
  SECTION:=mail
  CATEGORY:=Mail
  TITLE:=SpamAssassin client binary
  URL:=https://spamassassin.apache.org/
  DEPENDS:=+zlib
endef

define Package/spamc
  $(call Package/spamc/Default)
  VARIANT:=nossl
endef

define Package/spamc-ssl
  $(call Package/spamc/Default)
  TITLE+= (with SSL)
  DEPENDS+=+libopenssl
  VARIANT:=ssl
endef

ifeq ($(BUILD_VARIANT),ssl)
TARGET_CFLAGS += -DSPAMC_SSL
CONFIGURE_ARGS += --enable-ssl
endif

define Package/spamassassin/conffiles
/opt/etc/mail/spamassassin
endef

define Build/Configure
	$(call perlmod/Configure,,)
	$(call Build/Configure/Default)
	( cd "$(PKG_BUILD_DIR)/$(CONFIGURE_PATH)" && ./version.h.pl --with-version=$(PKG_SOURCE_VERSION) )
	$(SED) 's,/etc/opt,/opt/etc,;s,/var/opt,/opt/var,' $(PKG_BUILD_DIR)/Makefile
endef

define Build/Compile
	$(call perlmod/Compile,,)
	$(call Build/Compile/Default,,,spamc)
endef

define Package/spamassassin/install
	$(call perlmod/Install,$(1),Mail/SpamAssassin auto/Mail/SpamAssassin)
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/sa-awl $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/sa-learn $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/sa-compile $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/spamassassin $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/sa-update $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/spamd $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/sa-check_spamd $(1)/opt/bin
	$(SED) 's,^#!.*perl,#!/opt/bin/perl,' $(1)/opt/bin/s*
	$(INSTALL_DIR) $(1)/opt/etc/mail/spamassassin
	$(INSTALL_CONF) $(PKG_INSTALL_DIR)/opt/etc/mail/spamassassin/* $(1)/opt/etc/mail/spamassassin
endef

define Package/spamc/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/spamc $(1)/opt/bin
endef

Package/spamc-ssl/install = $(Package/spamc/install)

$(eval $(call BuildPackage,spamassassin))
$(eval $(call BuildPackage,spamc))
$(eval $(call BuildPackage,spamc-ssl))
