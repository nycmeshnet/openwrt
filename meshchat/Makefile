include $(TOPDIR)/rules.mk

PKG_NAME:=meshchat
PKG_VERSION:=0.1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=git://github.com/RocHack/meshchat.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=master
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/meshchat
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Instant Messaging
  TITLE:=Experimental decentralized chat system for cjdns, with IRC front-end.
  URL:=https://github.com/RocHack/meshchat
  DEPENDS:=+cjdns
  MAINTAINER:=snikus
endef

define Build/Configure
	git clone https://github.com/joyent/libuv.git $(PKG_BUILD_DIR)/deps/libuv
	git clone https://git.chromium.org/external/gyp.git \
	    $(PKG_BUILD_DIR)/deps/libuv/build/gyp
endef

define Build/Compile
	cd $(PKG_BUILD_DIR)/deps/libuv; python2 ./gyp_uv.py \
	    -Dtarget_arch=$(CONFIG_ARCH)

	CXX="$(TARGET_CXX)" \
	make -C $(PKG_BUILD_DIR)/deps/libuv/out CC="$(TARGET_CC)"

	CFLAGS="$(TARGET_CFLAGS)" \
	CC="$(TARGET_CC)" \
	LDFLAGS="$(TARGET_LDFLAGS)" \
	TARGET_ARCH="$(CONFIG_ARCH)" \
	$(MAKE) -C $(PKG_BUILD_DIR) -f main.mk INTERNAL_LIBUV=1
endef

define Package/meshchat/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/meshchat $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/meshchat.init $(1)/etc/init.d/meshchat
endef

$(eval $(call BuildPackage,meshchat))
