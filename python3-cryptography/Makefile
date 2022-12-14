# SPDX-License-Identifier: WTFPL

include $(TOPDIR)/rules.mk

PKG_NAME:=python3-cryptography
PKG_VERSION:=38.0.3
PKG_RELEASE:=1

PYPI_NAME:=cryptography
PKG_HASH:=bfbe6ee19615b07a98b1d2287d6a6073f734735b49ee45b11324d85efc4d5cbd

PKG_LICENSE:=Apache-2.0 BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE LICENSE.APACHE LICENSE.BSD LICENSE.PSF

PKG_BUILD_DEPENDS:=libffi/host

HOST_PYTHON3_PACKAGE_BUILD_DEPENDS:=cffi ./setuptools-rust.txt # cffi>=1.12

include $(TOPDIR)/feeds/packages/lang/python/pypi.mk
include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/python/python3-package.mk
include ../rust.mk

define Package/python3-cryptography
  SECTION:=lang
  CATEGORY:=Languages
  SUBMENU:=Python
  TITLE:=Cryptographic recipes and primitives (rust)
  URL:=https://github.com/pyca/cryptography
  DEPENDS:= \
	+libopenssl \
	+python3-cffi \
	+python3-distutils \
	+python3-email \
	+python3-urllib
endef

define Package/python3-cryptography/description
  cryptography is a package which provides cryptographic recipes and primitives
  to Python developers. Our goal is for it to be your "cryptographic standard
  library". It supports Python 3.6+ and PyPy3 7.2+.
endef

RUSTFLAGS += -C relocation-model=pic

PYTHON3_PKG_SETUP_VARS += \
	CARGO_HOME="$(CARGO_HOME)" \
	RUSTFLAGS="$(RUSTFLAGS)"

$(eval $(call Py3Package,python3-cryptography))
$(eval $(call BuildPackage,python3-cryptography))
#$(eval $(call BuildPackage,python3-cryptography-src))
