.ONESHELL:
SHELL = /bin/bash
.SHELLFLAGS += -e

MAIN_TARGET = $(ETHTOOL)
DERIVED_TARGET = $(ETHTOOL_DBG)

$(addprefix $(DEST)/, $(MAIN_TARGET)): $(DEST)/% :
	# Obtaining the ethtool debian package
	rm -rf ethtool*
	git clone https://salsa.debian.org/kernel-team/ethtool/
	pushd ethtool
	git checkout tags/debian/1%$(ETHTOOL_VERSION_BASE)-1
	# Build package
ifneq (,(filter $(CONFIGURED_ARCH),arm64 armhf))
	DEB_BUILD_OPTIONS=nocheck dpkg-buildpackage -rfakeroot -b -us -uc -j$(SONIC_CONFIG_MAKE_JOBS) --admindir $(SONIC_DPKG_ADMINDIR)
else
	dpkg-buildpackage -rfakeroot -b -us -uc -j$(SONIC_CONFIG_MAKE_JOBS) --admindir $(SONIC_DPKG_ADMINDIR)
endif
	
	popd
	mv $(DERIVED_TARGET) $* $(DEST)/

$(addprefix $(DEST)/, $(DERIVED_TARGET)): $(DEST)/% : $(DEST)/$(MAIN_TARGET)
