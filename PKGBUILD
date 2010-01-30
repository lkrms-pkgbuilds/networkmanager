# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contri-butor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgname=networkmanager
pkgver=0.7.999
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
        nm-system-settings.conf
        disable_set_hostname.patch)
sha256sums=('6fa351b3efc78cff4daa1f42386ba8435474bc121b4358a7d08bd7c9fb63aaef'
            '44b048804c7c0b8b3b0c29b8632b6ad613c397d0a1635ec918e10c0fbcdadf21'
            '00a16b694cd8d249d299349f02be1eefe646199ee7b5d1f910f8f7f353f463bf')

build() {
  cd "${srcdir}/NetworkManager-${pkgver}"
  patch -Np1 -i "${srcdir}/disable_set_hostname.patch" || return 1

  ./configure --prefix=/usr --sysconfdir=/etc \
      --with-distro=arch --localstatedir=/var \
      --libexecdir=/usr/lib/networkmanager \
      --disable-static --with-dhcp-client=dhcpcd \
      --with-crypto=nss --with-iptables=/usr/sbin/iptables \
      --enable-more-warnings=no || return 1

  make || return 1
  make DESTDIR="${pkgdir}" install || return 1
  install -m644 "${srcdir}/nm-system-settings.conf" "${pkgdir}/etc/NetworkManager/" || return 1
}
