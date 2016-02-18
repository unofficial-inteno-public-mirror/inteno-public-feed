#
# Copyright (C) 2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=owsd
PKG_VERSION:=0.3
PKG_RELEASE:=1

PKG_SOURCE_PROTO=git
PKG_SOURCE_URL:=git@public.inteno.se:owsd.git
PKG_SOURCE_VERSION:=dd922a1655192c7055a35f94de5ad3c80b7bd24e
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_SUBDIR:=${PKG_NAME}-${PKG_VERSION}
PKG_MAINTAINER:=Sartura Support for Inteno <support-inteno@sartura.hr>
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/owsd/default
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+ubus +ubox +libjson-c +libblobmsg-json +rpcd
  TITLE:=owsd daemon
endef

define Package/owsd
	$(call Package/$(PKG_NAME)/default)
	TITLE += (default)
	DEPENDS += +libwebsockets-openssl
#	DEPENDS += +libwebsockets-nossl
	VARIANT:=owsd
endef

#ifeq ($(BUILD_VARIANT),owsd)
#	CMAKE_OPTIONS += -OWSD_OPENSSL_SUPPORT=OFF
#	CMAKE_OPTIONS += -OWSD_WITH_SSL=OFF
#endif

define Package/owsd-openssl
	$(call Package/$(PKG_NAME)/default)
	TITLE += (With SSL support)
	DEPENDS += +libwebsockets-openssl
	VARIANT:=openssl
endef

#ifeq ($(BUILD_VARIANT),openssl)
#	CMAKE_OPTIONS += -OWSD_OPENSSL_CLIENT_CERTS=/etc/ssl/certs
#	CMAKE_OPTIONS += -OWSD_OPENSSL_SUPPORT=ON
#	CMAKE_OPTIONS += -OWSD_WITH_SSL=ON
#endif


define Package/owsd/description
 Web socket ubus daemon
endef

define Package/owsd/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/owsd $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/owsd.init $(1)/etc/init.d/owsd
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_BIN) ./files/owsd $(1)/etc/config/owsd
	$(INSTALL_DIR) $(1)//usr/share/rpcd/acl.d
	$(INSTALL_BIN) ./files/superuser.json $(1)/usr/share/rpcd/acl.d/superuser.json
endef

$(eval $(call BuildPackage,owsd))
$(eval $(call BuildPackage,owsd-openssl))