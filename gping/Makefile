# SPDX-License-Identifier: WTFPL

include $(TOPDIR)/rules.mk

PKG_NAME:=gping
PKG_VERSION:=1.6.3
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/orf/gping/tar.gz/refs/tags/$(PKG_NAME)-v$(PKG_VERSION)?
PKG_HASH:=ed55d87d04482a137e1d56355095f56fb4977724032245e3547206274966c1c5

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk
include ../cargo.mk

PKG_UNPACK:=$(HOST_TAR) -C "$(PKG_BUILD_DIR)" --strip-components=1 -xzf "$(DL_DIR)/$(PKG_SOURCE)"

define Package/gping
  SECTION:=net
  CATEGORY:=Network
  TITLE:=ping written in Rust
  URL:=https://github.com/orf/gping
  EXTRA_DEPENDS:=iputils-ping
endef

define Package/gping/description
  Ping, but with a graph.
endef

CARGO_ARGS += --workspace

define Package/gping/install
	$(INSTALL_DIR) $(1)/opt/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/gping $(1)/opt/usr/bin
endef

$(eval $(call BuildPackage,gping))
