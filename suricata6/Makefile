# SPDX-License-Identifier: WTFPL

# FIXME LINUX_2_6; see readme.txt

include $(TOPDIR)/rules.mk

PKG_NAME:=suricata
PKG_VERSION:=6.0.5
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://www.openinfosecfoundation.org/download
PKG_HASH:=0d4197047c84ba070dfc6b1d9f9ee92f52a71403bfac0e29b2554bb21fe00754

PKG_INSTALL:=1
PYTHON3_PKG_BUILD:=0

HOST_PYTHON3_PACKAGE_BUILD_DEPENDS:=PyYAML

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk
include $(TOPDIR)/feeds/packages/lang/python/python3-package.mk

define Package/suricata6/Default
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+jansson +libatomic +libmagic +libpcap +libpcre +libyaml +zlib $(ICONV_DEPENDS) 
  TITLE:=OISF Suricata IDS
  URL:=https://www.openinfosecfoundation.org/
endef

define Package/suricata6
  $(call Package/suricata6/Default)
  TITLE+= (reccommended)
  VARIANT:=reccommended
  DEPENDS+=+libcap-ng +libevent2 +libevent2-pthreads +libhiredis +liblua +liblz4 +libnet-1.2.x
endef

define Package/suricata6-extra
  $(call Package/suricata6/Default)
  TITLE+= (extra)
  VARIANT:=extra
  DEPENDS+=+libcap-ng +libevent2 +libevent2-pthreads +libhiredis +liblz4 +libnet-1.2.x +luajit
  DEPENDS+=+libnetfilter-log +libnetfilter-queue +libnss +nspr
endef

define Package/suricata6-mini
  $(call Package/suricata6/Default)
  TITLE+= (mimimal)
  VARIANT:=mini
endef

define Package/suricata6/description
  Suricata is the leading independent open source threat detection engine.
  By combining intrusion detection (IDS), intrusion prevention (IPS), network 
  security monitoring (NSM) and PCAP processing, Suricata can quickly identify,
  stop, and assess the most sophisticated attacks.
endef

Package/suricata6-extra/description=$(call Package/suricata6/description)
Package/suricata6-mini/description=$(call Package/suricata6/description)

define Package/suricata6/conffiles
/opt/etc/suricata/classification.config
/opt/etc/suricata/reference.config
/opt/etc/suricata/suricata.yaml
/opt/etc/suricata/threshold.config
endef

Package/suricata6-extra/conffiles=$(call Package/suricata6/conffiles)
Package/suricata6-mini/conffiles=$(call Package/suricata6/conffiles)

define Package/suricata6-python
  $(call Package/suricata6/Default)
  TITLE:=Suricata control tools (python)
  VARIANT:=python
  DEPENDS:=+python3 +python3-simplejson #+python3-argparse
endef

define Package/suricata6-update
  $(call Package/suricata6/Default)
  TITLE:=Suricata Update Tool (python)
  VARIANT:=update
  DEPENDS:=+python3 +python3-yaml
endef

Package/suricata6-python/description=$(call Package/suricata6/description)
Package/suricata6-update/description=$(call Package/suricata6/description)

TARGET_LDFLAGS += -latomic

CONFIGURE_ARGS += \
	--host=$(REAL_GNU_TARGET_NAME) \
	--enable-http2-decompression \
	CARGO_HOME="$(STAGING_DIR)/host/share/cargo"

ifeq ($(BUILD_VARIANT),extra)
CONFIGURE_ARGS += \
	--disable-lua \
	--disable-suricata-update \
	--enable-hiredis \
	--enable-nspr \
	--enable-nss \
	--enable-luajit \
	--enable-nflog \
	--enable-nfqueue \
	--disable-python \
	--with-libnet-includes=$(STAGING_DIR)/opt/include \
	--with-libnet-libraries=$(STAGING_DIR)/opt/lib

else ifeq ($(BUILD_VARIANT),mini)
CONFIGURE_ARGS += \
	--disable-hiredis \
	--disable-nspr \
	--disable-nss \
	--disable-lua \
	--disable-luajit \
	--disable-nflog \
	--disable-nfqueue \
	--disable-python \
	--disable-suricata-update

CONFIGURE_VARS += \
	ac_cv_lib_cap_ng_capng_clear=no \
	ac_cv_lib_lz4_LZ4F_createCompressionContext=no

else ifeq ($(BUILD_VARIANT),python)
CONFIGURE_ARGS += \
	--disable-suricata-update

else ifeq ($(BUILD_VARIANT),update)
CONFIGURE_ARGS += \
	--enable-suricata-update

else  # reccommended
CONFIGURE_ARGS += \
	--disable-luajit \
	--disable-nflog \
	--disable-nfqueue \
	--disable-nspr \
	--disable-nss \
	--disable-python \
	--disable-suricata-update \
	--enable-hiredis \
	--enable-lua \
	--with-libnet-includes=$(STAGING_DIR)/opt/include \
	--with-libnet-libraries=$(STAGING_DIR)/opt/lib

endif

define Package/suricata6/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/suricata $(1)/opt/bin

	$(INSTALL_DIR) $(1)/opt/etc/suricata
	$(INSTALL_CONF) \
		$(PKG_BUILD_DIR)/etc/classification.config \
		$(PKG_BUILD_DIR)/etc/reference.config \
		$(PKG_BUILD_DIR)/suricata.yaml \
		$(PKG_BUILD_DIR)/threshold.config \
			$(1)/opt/etc/suricata

	$(INSTALL_DIR) $(1)/opt/etc/suricata/rules
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/share/suricata/rules/*.rules \
			$(1)/opt/etc/suricata/rules

	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libhtp.so* $(1)/opt/lib

	$(INSTALL_DIR) $(1)/opt/var/lib/suricata/rules
	$(INSTALL_DIR) $(1)/opt/var/log/suricata/{certs,files}
	$(INSTALL_DIR) $(1)/opt/var/run/suricata
endef

Package/suricata6-extra/install=$(Package/suricata6/install)
Package/suricata6-mini/install=$(Package/suricata6/install)

define Package/suricata6-python/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/suricata{ctl,sc} $(1)/opt/bin

	$(INSTALL_DIR) $(1)$(PYTHON3_PKG_DIR)
	$(CP) \
		$(PKG_INSTALL_DIR)$(PYTHON3_PKG_DIR)/* $(1)$(PYTHON3_PKG_DIR)
	# Entware specific: fix exec path in scripts
	$(call Python3/FixShebang, \
		$(1)/opt/bin/suricatactl \
		$(1)/opt/bin/suricatasc \
		$(1)$(PYTHON3_PKG_DIR)/suricata/sc/suricatasc.py \
	)
	# rm __pycache__
	$(FIND) $(1)$(PYTHON3_PKG_DIR)/ -path '*/__pycache__/*' -delete
	$(FIND) $(1)$(PYTHON3_PKG_DIR)/ -type d -name __pycache__ -delete
endef

define Package/suricata6-update/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/suricata-update $(1)/opt/bin

	$(INSTALL_DIR) $(1)$(PYTHON3_PKG_DIR)/suricata
	$(CP) \
		$(PKG_INSTALL_DIR)$(PYTHON3_PKG_DIR)/suricata/update \
			$(1)$(PYTHON3_PKG_DIR)/suricata
	$(CP) \
		$(PKG_INSTALL_DIR)$(PYTHON3_PKG_DIR)/suricata_update* \
			$(1)$(PYTHON3_PKG_DIR)
	# Entware specific: fix exec path in scripts
	$(call Python3/FixShebang, \
		$(1)/opt/bin/suricata-update
	)
	# rm __pycache__
	$(FIND) $(1)$(PYTHON3_PKG_DIR)/ -path '*/__pycache__/*' -delete
	$(FIND) $(1)$(PYTHON3_PKG_DIR)/ -type d -name __pycache__ -delete
endef


define Package/suricata6/postinst
#!/bin/sh

echo -e "\n\tCurrent Suricata ruleset can be found at:
\thttps://rules.emergingthreats.net/open/suricata-$(PKG_VERSION)/emerging.rules.tar.gz
\n\tor you can install suricata6-update package\n"
endef

Package/suricata6-extra/postinst=$(Package/suricata6/postinst)
Package/suricata6-mini/postinst=$(Package/suricata6/postinst)

$(eval $(call BuildPackage,suricata6))
$(eval $(call BuildPackage,suricata6-extra))
$(eval $(call BuildPackage,suricata6-mini))
$(eval $(call BuildPackage,suricata6-python))
$(eval $(call BuildPackage,suricata6-update))