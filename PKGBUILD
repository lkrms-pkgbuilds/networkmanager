# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contri-butor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgname=networkmanager
pkgver=0.9.1.90
pkgrel=1
pkgdesc="Network Management daemon"
arch=('i686' 'x86_64')
license=('GPL')
url="http://www.gnome.org/projects/NetworkManager/"
depends=('dbus-glib' 'iproute2' 'libnl' 'nss' 'polkit' 'udev' 'wireless_tools' 'wpa_supplicant' 'ppp' 'dhcpcd')
makedepends=('intltool' 'dhclient' 'iptables' 'gobject-introspection')
optdepends=('modemmanager: for modem management service'
            'dhclient: alternative DHCP/DHCPv6 client'
            'iptables: Connection sharing'
            'dnsmasq: Connection sharing'
            'bluez: Bluetooth support')
options=('!libtool')
backup=('etc/NetworkManager/NetworkManager.conf')
install=networkmanager.install
source=(http://ftp.gnome.org/pub/gnome/sources/NetworkManager/0.9/NetworkManager-${pkgver}.tar.xz
        NetworkManager.conf disable_set_hostname.patch)
sha256sums=('de7e5d6de80bf14ada468d01f15dd4b118a6bef5d06cadf04954fd7de7ce5910'
            '44b048804c7c0b8b3b0c29b8632b6ad613c397d0a1635ec918e10c0fbcdadf21'
            '25056837ea92e559f09563ed817e3e0cd9333be861b8914e45f62ceaae2e0460')

build() {
  cd "${srcdir}/NetworkManager-${pkgver}"

  patch -Np1 -i "${srcdir}/disable_set_hostname.patch"

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
    --with-systemdsystemunitdir=/lib/systemd/system \
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
