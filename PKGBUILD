# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contri-butor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgname=networkmanager
pkgver=0.7.0rc2
pkgrel=1
pkgdesc="Network Management daemon"
arch=('i686' 'x86_64')
license=('GPL')
url="http://www.gnome.org/projects/NetworkManager/"
depends=('wireless_tools' 'iproute' 'hal>=0.5.11-5' 'libnl>=1.1' 'ppp' 'dhcpcd'
	 'wpa_supplicant>=0.5.10-2' "libnetworkmanager>=${pkgver}")
makedepends=('pkgconfig' 'perlxml')
options=('!libtool' '!makeflags')
source=(http://ftp.gnome.org/pub/gnome/sources/NetworkManager/0.7/NetworkManager-0.7.0-rc2.tar.bz2)
md5sums=('cd9feed3004db64fd4d44f5b4cd5c073')

build() {
  cd "${srcdir}/NetworkManager-0.7.0"
  ./configure --prefix=/usr --sysconfdir=/etc \
      --with-distro=arch --localstatedir=/var \
      --libexecdir=/usr/lib/networkmanager \
      --disable-static --with-dhcp-client=dhcpcd \
      --with-crypto=nss || return 1
  
  make || return 1
  sed -i -e 's/\slibnm-util\s/ /' -e 's/\slibnm-glib\s/ /' -e 's/\sinclude/ /' Makefile || return 1
  make DESTDIR="${pkgdir}" install || return 1
  install -d -m755 "${pkgdir}/usr/bin"
  install -m755 test/.libs/nm-tool "${pkgdir}/usr/bin/" || return 1
}
