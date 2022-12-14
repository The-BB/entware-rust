# SPDX-License-Identifier: WTFPL

include $(TOPDIR)/rules.mk

PKG_NAME:=static-web-server
PKG_VERSION ?=
PKG_RELEASE ?=

# XXX ^2.x depends on "ring"
ifeq ($(or $(CONFIG_mips),$(CONFIG_mipsel)),y)
  PKG_VERSION:=1.19.3
  PKG_RELEASE:=2
  PKG_HASH:=483a3abda885248dd72203dfc2725cffd5257533becb0c410105223952d9ccf3
else
  PKG_VERSION:=2.14.1
  PKG_RELEASE:=1
  PKG_HASH:=494a2c7c233df187776d26c7874064340474068846ddee93de6c6bfd8e666348
endif

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/joseluisq/static-web-server/tar.gz/refs/tags/v$(PKG_VERSION)?

PKG_LICENSE:=Apache-2.0 MIT
PKG_LICENSE_FILES:=LICENSE-APACHE LICENSE-MIT

include $(INCLUDE_DIR)/package.mk
include ../cargo.mk

define Package/static-web-server
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  TITLE:=Static Web Server
  URL:=https://sws.joseluisq.net/
  DEPENDS:=+libopenssl
endef

define Package/static-web-server/description
  A blazing fast and asynchronous web server for static files-serving.
endef

RUSTC_VARS += \
	OPENSSL_NO_VENDOR=1

define Package/static-web-server/install
	$(INSTALL_DIR) $(1)/opt/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/static-web-server $(1)/opt/usr/bin
endef

$(eval $(call BuildPackage,static-web-server))
