SPK_NAME = debian-chroot
SPK_VERS = 7.3
SPK_REV = 4
SPK_ICON = src/debian.png
DSM_UI_DIR = app

DEPENDS  =
SPK_DEPENDS = "python>2.7.3-3"

MAINTAINER = SynoCommunity
DESCRIPTION = Debian is a free operating system \(OS\) that comes with over 29000 packages, precompiled software bundled up in a nice format for easy installation on your DiskStation. Debian Chroot allows you to benefit from the Debian OS inside your DiskStation, alongside DSM. This package is intended for advanced users only.
DESCRIPTION_FRE = Debian est un système d\\\'exploitation \\\(SE\\\) qui rend disponible plus de 29000 paquets, logiciels précompilés et empaquetés dans un joli format pour rendre son installation facile sur votre DiskStation. Debian Chroot vous permet de bénéficier du SE Debian au sein de votre DiskStation, aux cotés de DSM. Ce paquet est destiné aux utilisateurs avancés uniquement.
RELOAD_UI = yes
DISPLAY_NAME = Debian Chroot
CHANGELOG = "1. DSM 5.0 compatibility"

HOMEPAGE   = http://www.debian.org/
LICENSE    =
HELPURL    = http://www.synocommunity.com/help/debian-chroot

INSTALLER_SCRIPT = src/installer.sh
SSS_SCRIPT       = src/dsm-control.sh

INSTALL_PREFIX = /usr/local/$(SPK_NAME)

COPY_TARGET = nop
POST_STRIP_TARGET = debian-chroot_extra_install

DEBIAN_ARCH =
ifeq ($(findstring $(ARCH),88f6281),$(ARCH))
DEBIAN_ARCH = armel
endif
ifeq ($(findstring $(ARCH),armadaxp),$(ARCH))
DEBIAN_ARCH = armhf
endif
ifeq ($(findstring $(ARCH),bromolow cedarview x86),$(ARCH))
DEBIAN_ARCH = i386
endif
ifeq ($(findstring $(ARCH),qoriq),$(ARCH))
DEBIAN_ARCH = powerpc
endif
ifeq ($(strip $(DEBIAN_ARCH)),)
$(error Arch $(ARCH) not supported)
endif


include ../../mk/spksrc.spk.mk

.PHONY: debian-chroot_extra_install
debian-chroot_extra_install: $(STAGING_DIR)/share/requirements.pybundle
	install -m 755 -d $(STAGING_DIR)/var
	debootstrap --foreign --arch $(DEBIAN_ARCH) wheezy $(STAGING_DIR)/var/chroottarget "http://ftp.debian.org/debian"
	install -m 644 src/sources.list $(STAGING_DIR)/var/chroottarget/etc/apt/sources.list.default
	install -m 644 src/preferences $(STAGING_DIR)/var/chroottarget/etc/apt/preferences.default
	install -m 644 src/policy-rc.d $(STAGING_DIR)/var/chroottarget/usr/sbin/policy-rc.d
	install -m 755 -d $(STAGING_DIR)/app
	install -m 644 src/app/config $(STAGING_DIR)/app/config
	install -m 644 src/app/style.css $(STAGING_DIR)/app/style.css
	install -m 644 src/app/debian-chroot.js $(STAGING_DIR)/app/debian-chroot.js
	install -m 755 src/app/setup.py $(STAGING_DIR)/app/setup.py
	install -m 755 src/app/start.py $(STAGING_DIR)/app/start.py
	install -m 755 src/app/stop.py $(STAGING_DIR)/app/stop.py
	install -m 755 src/app/debian-chroot.cgi.py $(STAGING_DIR)/app/debian-chroot.cgi
	install -m 755 -d $(STAGING_DIR)/app/application
	install -m 644 src/app/application/* $(STAGING_DIR)/app/application/
	install -m 755 -d $(STAGING_DIR)/app/texts
	for language in enu fre; do \
		install -m 755 -d $(STAGING_DIR)/app/texts/$${language}; \
		install -m 644 src/app/texts/$${language}/strings $(STAGING_DIR)/app/texts/$${language}/strings; \
	done
	install -m 755 -d $(STAGING_DIR)/app/images
	install -m 644 src/app/images/*.png $(STAGING_DIR)/app/images/
	for size in 16 24 32 48; do \
		convert $(SPK_ICON) -thumbnail $${size}x$${size} \
		        $(STAGING_DIR)/app/images/$(SPK_NAME)-$${size}.png ; \
	done

$(STAGING_DIR)/share/requirements.pybundle:
	@$(MSG) "Bundling requirements"
	install -m 755 -d $(STAGING_DIR)/share
	@cd $(WORK_DIR) && $(PIP) bundle $@ pyextdirect==0.3.1 flask
