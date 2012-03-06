# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contri-butor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgname=networkmanager
pkgver=0.9.3.995
pkgrel=1
pkgdesc="Network Management daemon"
arch=('i686' 'x86_64')
license=('GPL')
url="http://www.gnome.org/projects/NetworkManager/"
depends=('dbus-glib' 'iproute2' 'libnl' 'nss' 'polkit' 'udev' 'wpa_supplicant' 'ppp' 'dhcpcd'
         'libsystemd' 'libsoup')
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
source=(http://ftp.gnome.org/pub/gnome/sources/NetworkManager/0.9/NetworkManager-${pkgver}.tar.xz
        NetworkManager.conf disable_set_hostname.patch dnsmasq-path.patch systemd-fallback.patch)
sha256sums=('67b9005190f01e1cd7d91b9cc87f3068255f2a7eae80ccba78c6e5c99df2ae15'
            '44b048804c7c0b8b3b0c29b8632b6ad613c397d0a1635ec918e10c0fbcdadf21'
            '25056837ea92e559f09563ed817e3e0cd9333be861b8914e45f62ceaae2e0460'
            '65124505048cc8396daf0242c9f5d532fa669b4bbca305998c248ab2329490cb'
            '5e9bbd8a84883037d27a71ea9969d0cb03f09ca238fa733381bcf136bbc340a5')

build() {
  cd NetworkManager-${pkgver}

  patch -Np1 -i ../disable_set_hostname.patch
  patch -Np1 -i ../dnsmasq-path.patch
  patch -Np1 -i ../systemd-fallback.patch
  AUTOPOINT='intltoolize --automake --copy' autoreconf -f -i

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
    --with-resolvconf=/usr/sbin/resolvconf \
    --with-session-tracking=systemd \
    --with-wext=no \
    --disable-static \
    --enable-more-warnings=no \
    --disable-wimax

  make
}

package() {
  cd NetworkManager-${pkgver}
  make DESTDIR="${pkgdir}" install

  install -m644 ../NetworkManager.conf "${pkgdir}/etc/NetworkManager/"

  rm -rf "${pkgdir}/var/run/"
}
