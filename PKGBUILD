# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contri-butor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgname=networkmanager
pkgver=0.8.1
pkgrel=1
pkgdesc="Network Management daemon"
arch=('i686' 'x86_64')
license=('GPL')
url="http://www.gnome.org/projects/NetworkManager/"
depends=('wireless_tools' 'iproute2' 'libnl>=1.1' 'ppp' 'dhcpcd>=5.2.5' 'wpa_supplicant>=0.6.10' 'iptables' 'nss>=3.12.6' 'polkit>=0.96' 'udev>=157' 'dbus-glib')
makedepends=('pkg-config' 'intltool')
optdepends=('modemmanager: for modem management service')
options=('!libtool' '!makeflags')
backup=('etc/NetworkManager/nm-system-settings.conf')
source=(http://ftp.gnome.org/pub/gnome/sources/NetworkManager/0.8/NetworkManager-${pkgver}.tar.bz2
        nm-system-settings.conf
        disable_set_hostname.patch)
sha256sums=('dc126fbe3199d47899c4781e4fff32cee404dc7c728c6ade9eaa899bd80f19fa'
            '44b048804c7c0b8b3b0c29b8632b6ad613c397d0a1635ec918e10c0fbcdadf21'
            '941cb6cfbfc6b0e6e3d5ace140e17956d29b909d6a72c8f12f00734f3d6b6066')

build() {
  cd "${srcdir}/NetworkManager-${pkgver}"
  patch -Np1 -i "${srcdir}/disable_set_hostname.patch"

  ./configure --prefix=/usr --sysconfdir=/etc \
      --with-distro=arch --localstatedir=/var \
      --libexecdir=/usr/lib/networkmanager \
      --disable-static --with-dhcpcd=/sbin/dhcpcd \
      --with-crypto=nss --with-iptables=/usr/sbin/iptables \
      --enable-more-warnings=no

  make || return 1
  make DESTDIR="${pkgdir}" install
  install -m644 "${srcdir}/nm-system-settings.conf" "${pkgdir}/etc/NetworkManager/"
}
