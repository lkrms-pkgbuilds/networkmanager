# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contri-butor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgname=networkmanager
pkgver=0.8.3.996
pkgrel=1
pkgdesc="Network Management daemon"
arch=('i686' 'x86_64')
license=('GPL')
url="http://www.gnome.org/projects/NetworkManager/"
depends=('dbus-glib' 'iproute2' 'libnl' 'nss' 'polkit' 'udev' 'wireless_tools' 'wpa_supplicant' 'ppp' 'dhcpcd')
makedepends=('intltool' 'dhclient' 'iptables')
optdepends=('modemmanager: for modem management service'
            'dhclient: alternative DHCP/DHCPv6 client'
            'iptables: Connection sharing'
            'dnsmasq: Connection sharing'
            'bluez: Bluetooth support')
options=('!libtool')
backup=('etc/NetworkManager/NetworkManager.conf')
install=networkmanager.install
source=(http://ftp.gnome.org/pub/gnome/sources/NetworkManager/0.8/NetworkManager-${pkgver}.tar.bz2
#source=(http://mirrors.kernel.org/archlinux/other/networkmanager/NetworkManager-$pkgver-$pkgrel.tar.bz2
        NetworkManager.conf disable_set_hostname.patch)
md5sums=('022ca43fb1d8fe8ff36b82ba9e3a7bb0'
         '1653159d6634fb62d3a5c548b7a56151'
         'b7deda5bb47b3df0eb42782eabbaefe7')

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
    --disable-static \
    --enable-more-warnings=no

  make
}

package() {
  cd "${srcdir}/NetworkManager-${pkgver}"
  make DESTDIR="${pkgdir}" install

  install -m644 "${srcdir}/NetworkManager.conf" "${pkgdir}/etc/NetworkManager/"
}
