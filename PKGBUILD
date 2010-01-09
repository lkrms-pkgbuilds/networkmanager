# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contri-butor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgname=networkmanager
pkgver=0.7.998
pkgrel=2
pkgdesc="Network Management daemon"
arch=('i686' 'x86_64')
license=('GPL')
url="http://www.gnome.org/projects/NetworkManager/"
depends=('wireless_tools' 'iproute2' 'libnl>=1.1' 'ppp' 'dhcpcd>=4.0' 'wpa_supplicant>=0.6.9' 'iptables' 'nss>=3.12.4' 'polkit>=0.95' 'udev>=146')
makedepends=('pkgconfig' 'intltool')
optdepends=('modemmanager: for modem management service')
options=('!libtool' '!makeflags')
backup=('etc/NetworkManager/nm-system-settings.conf')
replaces=('libnetworkmanager')
provides=("libnetworkmanager=${pkgver}")
conflicts=('libnetworkmanager')
source=(http://ftp.gnome.org/pub/gnome/sources/NetworkManager/0.7/NetworkManager-${pkgver}.tar.bz2
	nm-system-settings.conf disable_set_hostname.patch)
sha256sums=(' 5ad5a348b0d3fb3274c11451bc40b1e2cc85e31eab0c56b52eab060103c0b903'
            '44b048804c7c0b8b3b0c29b8632b6ad613c397d0a1635ec918e10c0fbcdadf21'
	    'fc99590a451e845c428ed8e203612e48a0899ebeaef00e4928936328f71e35b2')

build() {
  cd "${srcdir}/NetworkManager-${pkgver}"
  patch -Np1 -i $srcdir/disable_set_hostname.patch || return 1

  ./configure --prefix=/usr --sysconfdir=/etc \
      --with-distro=arch --localstatedir=/var \
      --libexecdir=/usr/lib/networkmanager \
      --disable-static --with-dhcp-client=dhcpcd \
      --with-crypto=nss --with-iptables=/usr/sbin/iptables || return 1

  make || return 1
  make DESTDIR="${pkgdir}" install || return 1
  install -m644 "${srcdir}/nm-system-settings.conf" "${pkgdir}/etc/NetworkManager/" || return 1
}
