# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contributor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgname=networkmanager
_pkgname=NetworkManager
pkgver=0.9.6.0
pkgrel=1
pkgdesc="Network Management daemon"
arch=('i686' 'x86_64')
license=('GPL')
url="http://www.gnome.org/projects/$_pkgname/"
depends=('dbus-glib' 'iproute2' 'libnl' 'nss' 'polkit' 'udev' 'wpa_supplicant' 'ppp' 'dhcpcd'
         'libsoup')
makedepends=('intltool' 'dhclient' 'iptables' 'gobject-introspection')
optdepends=('modemmanager: for modem management service'
            'dhclient: alternative DHCP/DHCPv6 client'
            'iptables: Connection sharing'
            'dnsmasq: Connection sharing'
            'bluez: Bluetooth support'
            'openresolv: openresolv support')
options=('!libtool')
backup=('etc/NetworkManager/NetworkManager.conf')
install=networkmanager.install
source=(http://ftp.gnome.org/pub/gnome/sources/$_pkgname/${pkgver:0:3}/$_pkgname-$pkgver.tar.xz
        NetworkManager.conf disable_set_hostname.patch dnsmasq-path.patch
        nm-polkit-permissive.patch)
sha256sums=('3982b623b7b199ac99e2ddd0840fe7d088245a49e3f680237e8baebf0cf86d07'
            '44b048804c7c0b8b3b0c29b8632b6ad613c397d0a1635ec918e10c0fbcdadf21'
            '25056837ea92e559f09563ed817e3e0cd9333be861b8914e45f62ceaae2e0460'
            '65124505048cc8396daf0242c9f5d532fa669b4bbca305998c248ab2329490cb'
            'e23b651a90ef62d515921953b4c6779538d35a737fb74c266b7fce94d62fdb06')

build() {
  cd $_pkgname-$pkgver

  patch -Np1 -i ../disable_set_hostname.patch
  patch -Np1 -i ../dnsmasq-path.patch
  patch -Np1 -i ../nm-polkit-permissive.patch

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
    --with-systemdsystemunitdir=/usr/lib/systemd/system \
    --with-udev-dir=/usr/lib/udev \
    --with-resolvconf=/usr/sbin/resolvconf \
    --with-session-tracking=ck \
    --disable-static \
    --enable-more-warnings=no \
    --disable-wimax

  make
}

package() {
  cd $_pkgname-$pkgver
  make DESTDIR="$pkgdir" install

  install -m644 ../NetworkManager.conf "$pkgdir/etc/NetworkManager/"

  rm -r "$pkgdir/var/run"

  # Provide native service for arch-daemons generator
  ln -s NetworkManager.service "$pkgdir/usr/lib/systemd/system/networkmanager.service"
}
