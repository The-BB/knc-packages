include $(TOPDIR)/rules.mk

PKG_NAME:=libucontext
PKG_VERSION:=1.5.1
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/kaniini/libucontext.git
PKG_SOURCE_VERSION:=$(PKG_NAME)-$(PKG_VERSION)
PKG_MIRROR_HASH:=6c5f756f32418d8db52b715e1837e46ea2d94926ba93e74d202e2cf891eda1f0

PKG_MAINTAINER:=Volker Christian <me@vchrist.at>
PKG_LICENSE:=ISC
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_FLAGS:=no-mips16

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/meson.mk

define Package/libucontext
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=libucontext is a library which provides the ucontext.h C API
  URL:=https://github.com/kaniini/libucontext
endef

define Package/libucontext-tests
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Test applications for libucontext
  URL:=https://github.com/kaniini/libucontext
  DEPENDS:=libucontext
endef

define Package/libucontext/description
  This package is a development package aimed to be linked to
  libraries/applications which need the SYS-V ucontext API.
endef

MESON_ARGS += \
	-Ddocs=false

ifneq ($(CONFIG_USE_MUSL),)
MESON_ARGS += \
	-Dfreestanding=false
else ifneq ($(CONFIG_USE_GLIBC),)
MESON_ARGS += \
	-Dfreestanding=true \
	-Dexport_unprefixed=false
endif

define Build/InstallDev
	$(INSTALL_DIR) $(1)/opt/include $(1)/opt/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/opt/include/libucontext $(1)/opt/include/
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libucontext* $(1)/opt/lib/
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/pkgconfig/libucontext.pc $(1)/opt/lib/pkgconfig/
endef

define Package/libucontext/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libucontext*.so.* $(1)/opt/lib/
endef

define Package/libucontext-tests/install
	$(INSTALL_DIR) $(1)/opt/bin
ifneq ($(CONFIG_USE_MUSL),)
	$(INSTALL_BIN) $(MESON_BUILD_DIR)/test_libucontext_posix $(1)/opt/bin/
endif
	$(INSTALL_BIN) $(MESON_BUILD_DIR)/test_libucontext $(1)/opt/bin/
endef

$(eval $(call BuildPackage,libucontext))
$(eval $(call BuildPackage,libucontext-tests))
