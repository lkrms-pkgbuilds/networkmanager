# $Id: PKGBUILD,v 1.12 2007/08/19 12:38:16 jgc Exp $
# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contri-butor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgname=networkmanager
pkgver=0.6.5
pkgrel=3
pkgdesc="Network Management daemon"
arch=('i686' 'x86_64')
license=('GPL')
url="http://www.gnome.org/projects/NetworkManager/"
depends=('wireless_tools' 'iproute' 'dhcdbd' 'hal>=0.5.9.1' 'libnl'
	 'wpa_supplicant>=0.5.8' "libnetworkmanager>=${pkgver}")
makedepends=('pkgconfig' 'perlxml')
options=('!libtool')
source=(http://ftp.gnome.org/pub/GNOME/sources/NetworkManager/0.6/NetworkManager-${pkgver}.tar.bz2 
	networkmanager-initscript.patch
	dbus-hal-policy.patch
	dont-tear-down-upped-interfaces.patch
	dont-up-notwired-interfaces.patch
	fix-ethernet-link-detection-races.patch
	NetworkManager.conf
	ntpdate
	netfs)
md5sums=('b827d300eb28458f6588eb843cba418d'
         '730a3ebd470986a9f61deaf52cac0cb0'
         '2f208e80adcd558ccbcb57344f842691'
         'a6af8522d2fdd59ba7a0e4b04c19a405'
         '4e88282faf2b41d757f9aca72c1a249f'
         'de72f700fdfec464d05d6296822d0e6c'
         'fbdcffdc2cc7740f99c6ef5047f35160'
         '9a3b8da1efac158d91bb9fc699736b03'
         'ee592ee567faf683e7aecf651bd15937')

build() {
  cd ${startdir}/src/NetworkManager-${pkgver}
  patch -Np0 -i ${startdir}/src/networkmanager-initscript.patch || return 1
  patch -Np1 -i ${startdir}/src/dbus-hal-policy.patch || return 1
  patch -Np1 -i ${startdir}/src/dont-up-notwired-interfaces.patch || return 1
  patch -Np1 -i ${startdir}/src/dont-tear-down-upped-interfaces.patch || return 1
  patch -Np1 -i ${startdir}/src/fix-ethernet-link-detection-races.patch || return 1

  ./configure --prefix=/usr --sysconfdir=/etc \
      --with-distro=arch --localstatedir=/var \
      --without-gnome --libexecdir=/usr/lib/networkmanager
  
  make || return 1
  sed -i -e '/\slibnm-util\s/d' -e '/\sinclude/d' -e '/\sgnome\s/d' Makefile
  make DESTDIR=${startdir}/pkg install
  install -d -m755 ${startdir}/pkg/usr/bin
  install -m755 test/nm-tool ${startdir}/pkg/usr/bin/nm-tool
  install -m644 ${startdir}/src/NetworkManager.conf ${startdir}/pkg/etc/dbus-1/system.d/NetworkManager.conf
  install -m755 ${startdir}/src/ntpdate ${startdir}/pkg/etc/NetworkManager/dispatcher.d/ntpdate
  install -m755 ${startdir}/src/netfs ${startdir}/pkg/etc/NetworkManager/dispatcher.d/netfs
}
