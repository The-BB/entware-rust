# SPDX-License-Identifier: WTFPL

include $(TOPDIR)/rules.mk

PKG_NAME:=maturin
PKG_VERSION:=0.14.6
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/PyO3/maturin/tar.gz/refs/tags/v$(PKG_VERSION)?
PKG_HASH:=28ac0c34b55f938ba03aa123fec6b01ebec925e12ee37dd6967fa20d986d082c

PKG_LICENSE:=Apache-2.0 MIT
PKG_LICENSE_FILES:=license-apache license-mit

PKG_HOST_ONLY:=1
HOST_BUILD_DEPENDS:=python3/host rustc-dev/host

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk
include $(TOPDIR)/feeds/packages/lang/python/python3-host.mk

define Package/maturin
  SECTION:=devel
  CATEGORY:=Development
  TITLE:=Build and publish crates
  URL:=https://github.com/pyo3/maturin
  BUILDONLY:=1
endef

define Package/maturin/description
  Build and publish crates with pyo3, rust-cpython and cffi bindings as well
  as rust binaries as python packages.
endef

define Host/Compile
	( cd $(HOST_BUILD_DIR); \
		CARGO_HOME=$(STAGING_DIR)/host/share/cargo \
		cargo build --release; \
	)
endef

define Host/Install
	$(INSTALL_DIR) $(STAGING_DIR_HOSTPKG)/bin
	$(CP) $(HOST_BUILD_DIR)/target/release/maturin $(STAGING_DIR_HOSTPKG)/bin

	$(INSTALL_DIR) $(HOST_PYTHON3_PKG_DIR)
	$(CP) $(HOST_BUILD_DIR)/maturin $(HOST_PYTHON3_PKG_DIR)
endef

define Host/Clean
	$(RM) $(STAGING_DIR_HOSTPKG)/bin/maturin
	$(RM) -r $(HOST_PYTHON3_PKG_DIR)/maturin
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,maturin))
