# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contributor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgbase=networkmanager
pkgname=(networkmanager libnm-glib)
pkgver=1.2.2
pkgrel=2
pkgdesc="Network Management daemon"
arch=(i686 x86_64)
license=(GPL2 LGPL2.1)
url="http://www.gnome.org/projects/NetworkManager/"
_pppver=2.4.7
makedepends=(intltool dhclient iptables gobject-introspection gtk-doc
             "ppp=$_pppver" modemmanager dbus-glib iproute2 nss polkit
             wpa_supplicant libsoup systemd libgudev libmm-glib rp-pppoe
             libnewt libndp libteam vala perl-yaml python-gobject)
checkdepends=(libx11 python-dbus)
source=(https://download.gnome.org/sources/NetworkManager/${pkgver:0:3}/NetworkManager-$pkgver.tar.xz
        NetworkManager.conf)
sha256sums=('41d8082e027f58bb5fa4181f93742606ab99c659794a18e2823eff22df0eecd9'
            '67f112c1ac8ee3726eb229f5cd783de19f09cc252af49e157343d82b324b923f')

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

  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0 /g' -e 's/    if test "$export_dynamic" = yes && test -n "$export_dynamic_flag_spec"; then/      func_append compile_command " -Wl,-O1,--as-needed"\n      func_append finalize_command " -Wl,-O1,--as-needed"\n\0/' libtool

  make
}

check() {
  cd NetworkManager-$pkgver
  make -k check
}

package_networkmanager() {
  depends=(libnm-glib iproute2 polkit wpa_supplicant libsoup libmm-glib
           libnewt libndp libteam)
  optdepends=('dnsmasq: connection sharing'
              'bluez: Bluetooth support'
              'openresolv: resolvconf support'
              'ppp: dialup connection support'
              'rp-pppoe: ADSL support'
              'dhclient: External DHCP client'
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
  rmdir -p --ignore-fail-on-non-empty \
    "$pkgdir"/usr/{share/{vala/vapi,gir-1.0},lib/girepository-1.0}
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
