#
# Copyright (C) 2006-2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=sudo
PKG_REALVERSION:=1.9.17p2
PKG_VERSION:=$(subst p,_p,$(PKG_REALVERSION))
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_REALVERSION).tar.gz
PKG_SOURCE_URL:=https://www.sudo.ws/dist
PKG_HASH:=4a38a1ab3adb1199257edc2a7c4a2bd714665eb605b04368843b06dada2cfcfb
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_REALVERSION)

PKG_MAINTAINER:=Alexandru Ardelean <ardeleanalex@gmail.com>

PKG_LICENSE:=ISC
PKG_LICENSE_FILES:=LICENSE.md
PKG_CPE_ID:=cpe:/a:sudo_project:sudo

PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

include $(INCLUDE_DIR)/package.mk

define Package/sudo
  SECTION:=admin
  CATEGORY:=Administration
  TITLE:=Delegate authority to run commands
  URL:=https://www.sudo.ws/
#  DEPENDS:= +USE_GLIBC:libcrypt-compat
endef

define Package/sudo/description
 Sudo (su "do") allows a system administrator to delegate authority to
 give certain users (or groups of users) the ability to run some (or
 all) commands as root or another user while providing an audit trail of
 the commands and their arguments.
endef

define Package/sudo/conffiles
/opt/etc/sudoers
/opt/etc/sudoers.d/
endef

TARGET_LDFLAGS += $(if $(CONFIG_GCC_LIBSSP),-lssp)

CONFIGURE_ARGS += \
	--without-pam \
	--disable-pam-session \
	--with-editor=/opt/bin/vi \
	--without-lecture \
	--disable-zlib \
	--disable-openssl \
	--with-rundir=/opt/var/run/sudo \
	--with-vardir=/opt/var/lib/sudo \
	--with-iologdir=/opt/var/log/sudo-io \
	--with-ldap-conf-file=/opt/etc/ldap.conf \
	--with-ldap-secret-file=/opt/etc/ldap.secret \
	--with-logpath=/opt/var/log/sudo.log \
	--with-netsvc=/opt/etc/netsvc.conf \
	--with-nsswitch=/opt/etc/nsswitch.conf \
	--with-relaydir=/opt/var/log/sudo_logsrvd \
	--with-sssd-conf=/opt/etc/sssd/sssd.conf \
	--with-tzdir=/opt/share/zoneinfo

CONFIGURE_VARS += \
	sudo_cv_uid_t_len=10 \
	sudo_cv_func_unsetenv_void=no \
	sudo_cv_log_dir=/opt/var/log \
	ac_cv_path_BSHELLPROG=/opt/bin/sh \
	ac_cv_path_MVPROG=/opt/bin/mv \
	ac_cv_path_VIPROG=/opt/bin/vi

define Package/sudo/install
	$(INSTALL_DIR) \
		$(1)/opt/etc/sudoers.d \
		$(1)/opt/{bin,sbin} \
		$(1)/opt/lib/sudo

	$(CP) $(PKG_INSTALL_DIR)/opt/bin/sudo $(1)/opt/bin/
	chmod 4755 $(1)/opt/bin/sudo
	$(CP) $(PKG_INSTALL_DIR)/opt/sbin/visudo $(1)/opt/sbin/
	$(CP) $(PKG_INSTALL_DIR)/opt/etc/sudoers $(1)/opt/etc/
	chmod 0440 $(1)/opt/etc/sudoers
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/sudo/*.so* $(1)/opt/lib/sudo/
#	$(INSTALL_BIN) ./files/sudo.init $(1)/etc/init.d/sudo
endef

#define Package/sudo/postinst
##!/bin/sh
#
#[ -n "$$IPKG_INSTROOT" ] || {
#	/etc/init.d/sudo enable
#	/etc/init.d/sudo start
#}
#endef

$(eval $(call BuildPackage,sudo))
