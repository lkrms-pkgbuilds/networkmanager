# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contri-butor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgname=networkmanager
pkgver=0.7.2
pkgrel=1
pkgdesc="Network Management daemon"
arch=('i686' 'x86_64')
license=('GPL')
url="http://www.gnome.org/projects/NetworkManager/"
depends=('wireless_tools' 'iproute2' 'hal>=0.5.13-3' 'libnl>=1.1' 'ppp' 'dhcpcd>=4.0' 'wpa_supplicant>=0.6.9' 'iptables' 'nss>=3.12.4' 'policykit')
makedepends=('pkgconfig' 'intltool')
options=('!libtool' '!makeflags')
backup=('etc/NetworkManager/nm-system-settings.conf')
replaces=('libnetworkmanager')
provides=("libnetworkmanager=${pkgver}")
conflicts=('libnetworkmanager')
source=(http://ftp.gnome.org/pub/gnome/sources/NetworkManager/0.7/NetworkManager-${pkgver}.tar.bz2
	nm-system-settings.conf)
sha256sums=('8d6f47432ae372aaffdc78f056cec9e9fc6bb1547a454e0c1ad7d130a9198470'
            '44b048804c7c0b8b3b0c29b8632b6ad613c397d0a1635ec918e10c0fbcdadf21')

build() {
  cd "${srcdir}/NetworkManager-${pkgver}"
  sed -e 's/"\/sbin\/iptables/"\/usr\/sbin\/iptables/g' -i src/nm-activation-request.c || return 1
  ./configure --prefix=/usr --sysconfdir=/etc \
      --with-distro=arch --localstatedir=/var \
      --libexecdir=/usr/lib/networkmanager \
      --disable-static --with-dhcp-client=dhcpcd \
      --with-crypto=nss || return 1

  make || return 1
  make DESTDIR="${pkgdir}" install || return 1
  install -m644 "${srcdir}/nm-system-settings.conf" "${pkgdir}/etc/NetworkManager/" || return 1
}
