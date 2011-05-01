# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contri-butor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgname=networkmanager
pkgver=0.8.998
pkgrel=3
pkgdesc="Network Management daemon"
arch=('i686' 'x86_64')
license=('GPL')
url="http://www.gnome.org/projects/NetworkManager/"
depends=('dbus-glib' 'iproute2' 'libnl' 'nss' 'polkit' 'udev' 'wireless_tools' 'wpa_supplicant' 'ppp' 'dhcpcd')
makedepends=('intltool' 'dhclient' 'iptables' 'gobject-introspection' 'gtk-doc')
optdepends=('modemmanager: for modem management service'
            'dhclient: alternative DHCP/DHCPv6 client'
            'iptables: Connection sharing'
            'dnsmasq: Connection sharing'
            'bluez: Bluetooth support')
options=('!libtool')
backup=('etc/NetworkManager/NetworkManager.conf')
install=networkmanager.install
#http://ftp.gnome.org/pub/gnome/sources/NetworkManager/0.8/NetworkManager-${pkgver}.tar.bz2
source=(ftp://ftp.archlinux.org/other/networkmanager/NetworkManager-${pkgver}-20110501.tar.xz
        NetworkManager.conf disable_set_hostname.patch)
sha256sums=('f9362e5db1f700df927c5ce5a4089e67270a0ce8ca9ab1c5384ae93d2d8ec0e2'
            '44b048804c7c0b8b3b0c29b8632b6ad613c397d0a1635ec918e10c0fbcdadf21'
            '1e4586991bc96ef004dc520c794047a336c54433e0bd4edc3879fb6e7ab0e553')

build() {
  cd "${srcdir}/NetworkManager-${pkgver}"

  patch -Np1 -i "${srcdir}/disable_set_hostname.patch"

  ./autogen.sh
  ./configure \
    --prefix=/usr \
    --sysconfdir=/etc \
    --localstatedir=/var \
    --libexecdir=/usr/lib/networkmanager \
    --with-crypto=nss \
    --with-distro=arch \
    --with-dhclient=/usr/sbin/dhclient \
    --with-dhcpcd=/sbin/dhcpcd \
    --with-iptables=/usr/sbin/iptables \
    --disable-static \
    --enable-more-warnings=no \
    --disable-wimax

  make
}

package() {
  cd "${srcdir}/NetworkManager-${pkgver}"
  make DESTDIR="${pkgdir}" install

  install -m644 "${srcdir}/NetworkManager.conf" "${pkgdir}/etc/NetworkManager/"

  rm -rf "${pkgdir}/var/run/"
}
