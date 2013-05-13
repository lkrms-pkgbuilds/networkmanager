# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contributor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgname=networkmanager
_pkgname=NetworkManager
pkgver=0.9.8.0
pkgrel=3
pkgdesc="Network Management daemon"
arch=('i686' 'x86_64')
license=('GPL')
url="http://www.gnome.org/projects/$_pkgname/"
depends=(dbus-glib iproute2 libnl nss polkit udev wpa_supplicant dhcp-client
         libsoup systemd modemmanager)
makedepends=(intltool dhcpcd dhclient iptables gobject-introspection gtk-doc)
optdepends=('dhclient: DHCPv6 support'
            'iptables: Connection sharing'
            'dnsmasq: Connection sharing'
            'bluez: Bluetooth support'
            'openresolv: resolvconf support'
            'ppp: Dialup connection support')
options=('!libtool')
backup=('etc/NetworkManager/NetworkManager.conf')
install=networkmanager.install
source=(http://ftp.gnome.org/pub/gnome/sources/$_pkgname/${pkgver:0:3}/$_pkgname-$pkgver.tar.xz
        NetworkManager.conf disable_set_hostname.patch dnsmasq-path.patch)
sha256sums=('c366bcded6354d8186ad93c05d26d6a20bc550aa0391f974704e7a60e9f6096b'
            '44b048804c7c0b8b3b0c29b8632b6ad613c397d0a1635ec918e10c0fbcdadf21'
            '25056837ea92e559f09563ed817e3e0cd9333be861b8914e45f62ceaae2e0460'
            'bc0291f09bbb3aa646af6927a7de64c964f0179cf02b013019b1ac7d49ea6caf')

prepare() {
  cd $_pkgname-$pkgver
  patch -Np1 -i ../disable_set_hostname.patch
  patch -Np1 -i ../dnsmasq-path.patch
  AUTOPOINT="intltoolize -f -c --automake" autoreconf -fi
}

build() {
  cd $_pkgname-$pkgver
  ./configure \
    --prefix=/usr \
    --sysconfdir=/etc \
    --localstatedir=/var \
    --libexecdir=/usr/lib/networkmanager \
    --with-crypto=nss \
    --with-dhclient=/usr/sbin/dhclient \
    --with-dhcpcd=/usr/sbin/dhcpcd \
    --with-iptables=/usr/sbin/iptables \
    --with-dnsmasq=/usr/bin/dnsmasq \
    --with-systemdsystemunitdir=/usr/lib/systemd/system \
    --with-udev-dir=/usr/lib/udev \
    --with-resolvconf=/usr/sbin/resolvconf \
    --with-session-tracking=systemd \
    --with-modem-manager-1 \
    --disable-static \
    --enable-more-warnings=no \
    --disable-wimax \
    --enable-modify-system \
    --enable-doc

  make
}

package() {
  cd $_pkgname-$pkgver
  make DESTDIR="$pkgdir" install

  install -m644 ../NetworkManager.conf "$pkgdir/etc/NetworkManager/"
  install -m755 -d "$pkgdir/etc/NetworkManager/dnsmasq.d"

  rm -r "$pkgdir/var/run"
}
