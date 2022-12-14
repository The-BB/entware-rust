# SPDX-License-Identifier: WTFPL
#
# XXX depends on "ring" since 0.10.0

include $(TOPDIR)/rules.mk

PKG_NAME:=termscp
PKG_VERSION:=0.9.0
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/veeso/termscp/tar.gz/refs/tags/v$(PKG_VERSION)?
PKG_HASH:=07ad690097d5a846ef0c6b1c8a681641ddc19a329400ddd8502018fbd5143f59

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

CARGO_COMPILE:=0

include $(INCLUDE_DIR)/package.mk
include ../cargo.mk

define Package/termscp-legacy
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=File Transfer
  TITLE:=a feature rich terminal file transfer and explorer
  URL:=https://veeso.github.io/termscp/
  DEPENDS:=@(mips||mipsel) +libdbus +libopenssl +zlib
endef

define Package/termscp-legacy/description
  termscp is a feature rich terminal file transfer
  and explorer with support for SCP/SFTP/FTP/S3.
endef

RUSTC_VARS += \
	OPENSSL_NO_VENDOR=1

ifeq ($(or $(CONFIG_aarch64),$(CONFIG_i386),$(CONFIG_x86_64)),)
RUSTFLAGS += --cfg crossbeam_no_atomic_64
endif

define Package/termscp-legacy/install
	$(INSTALL_DIR) $(1)/opt/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/termscp $(1)/opt/usr/bin
endef

$(eval $(call BuildPackage,termscp-legacy))
