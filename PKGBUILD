# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contributor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgbase=networkmanager
pkgname=(networkmanager libnm-glib)
pkgver=1.1.94
pkgrel=1
pkgdesc="Network Management daemon"
arch=(i686 x86_64)
license=(GPL2 LGPL2.1)
url="http://www.gnome.org/projects/NetworkManager/"
_pppver=2.4.7
makedepends=(intltool dhclient iptables gobject-introspection gtk-doc "ppp=$_pppver"
             modemmanager dbus-glib iproute2 libnl nss polkit wpa_supplicant libsoup
             systemd libgudev libmm-glib rp-pppoe libnewt libndp libteam vala perl-yaml
             python-gobject)
checkdepends=(libx11 python-dbus)
source=(https://download.gnome.org/sources/NetworkManager/${pkgver:0:3}/NetworkManager-$pkgver.tar.xz
        NetworkManager.conf)
sha256sums=('93d4e9359f8f4dd19f536fafff77820c9e6a318ef65e2ec6bf9ee0e3365498c6'
            '2c6a647b5aec9f3c356d5d95251976a21297c6e64bd8d2a59339f8450a86cb3b')

prepare() {
  cd NetworkManager-$pkgver
  2to3 -w libnm src tools
  NOCONFIGURE=1 ./autogen.sh
}

build() {
  cd NetworkManager-$pkgver
  ./configure --prefix=/usr \
    --sysconfdir=/etc \
    --localstatedir=/var \
    --sbindir=/usr/bin \
    --libexecdir=/usr/lib/networkmanager \
    --with-crypto=nss \
    --with-dhclient=/usr/bin/dhclient \
    --without-dhcpcd \
    --with-dnsmasq=/usr/bin/dnsmasq \
    --with-iptables=/usr/bin/iptables \
    --with-systemdsystemunitdir=/usr/lib/systemd/system \
    --with-udev-dir=/usr/lib/udev \
    --with-resolvconf=/usr/bin/resolvconf \
    --with-pppd=/usr/bin/pppd \
    --with-pppd-plugin-dir=/usr/lib/pppd/$_pppver \
    --with-pppoe=/usr/bin/pppoe \
    --with-kernel-firmware-dir=/usr/lib/firmware \
    --with-session-tracking=systemd \
    --with-modem-manager-1 \
    --disable-static \
    --enable-more-warnings=no \
    --disable-wimax \
    --enable-modify-system \
    --enable-doc \
    --enable-gtk-doc

  make
}

check() {
  cd NetworkManager-$pkgver
  make -k check
}

package_networkmanager() {
  depends=(libnm-glib iproute2 libnl polkit wpa_supplicant dhclient libsoup
           libmm-glib libnewt libndp libteam libgudev)
  optdepends=('dnsmasq: connection sharing'
              'bluez: Bluetooth support'
              'openresolv: resolvconf support'
              'ppp: dialup connection support'
              'rp-pppoe: ADSL support'
              'modemmanager: cellular network support')
  backup=('etc/NetworkManager/NetworkManager.conf')

  cd NetworkManager-$pkgver
  make DESTDIR="$pkgdir" install
  make DESTDIR="$pkgdir" -C libnm uninstall
  make DESTDIR="$pkgdir" -C libnm-glib uninstall
  make DESTDIR="$pkgdir" -C libnm-util uninstall
  make DESTDIR="$pkgdir" -C vapi uninstall

  # Some stuff to move is left over
  mv "$pkgdir/usr/include" ..
  mv "$pkgdir/usr/lib/pkgconfig" ..

  install -m644 ../NetworkManager.conf "$pkgdir/etc/NetworkManager/"
  install -m755 -d "$pkgdir/etc/NetworkManager/dnsmasq.d"

  rm -r "$pkgdir/var/run"
}

package_libnm-glib() {
  pkgdesc="NetworkManager library"
  depends=(libgudev nss dbus-glib libutil-linux)

  install -d "$pkgdir/usr/lib"
  mv include "$pkgdir/usr"
  mv pkgconfig "$pkgdir/usr/lib"

  cd NetworkManager-$pkgver
  make DESTDIR="$pkgdir" -C libnm install
  make DESTDIR="$pkgdir" -C libnm-util install
  make DESTDIR="$pkgdir" -C libnm-glib install
  make DESTDIR="$pkgdir" -C vapi install
}
