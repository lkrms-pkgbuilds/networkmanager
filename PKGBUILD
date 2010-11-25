# Maintainer: Jan de Groot <jgc@archlinxu.org>
# Contri-butor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>
# Contributor: Valentine Sinitsyn <e_val@inbox.ru>

pkgname=networkmanager
pkgver=0.8.2
pkgrel=6
pkgdesc="Network Management daemon"
arch=('i686' 'x86_64')
license=('GPL')
url="http://www.gnome.org/projects/NetworkManager/"
depends=('dbus-glib' 'iproute2' 'libnl' 'nss' 'polkit' 'udev' 'wireless_tools' 'wpa_supplicant' 'ppp' 'dhcpcd')
makedepends=('intltool' 'autoconf' 'dhclient' 'iptables')
optdepends=('modemmanager: for modem management service'
            'dhclient: alternative DHCP/DHCPv6 client'
            'iptables: Connection sharing'
            'dnsmasq: Connection sharing'
            'bluez: Bluetooth support')
options=('!libtool')
backup=('etc/NetworkManager/nm-system-settings.conf')
source=(http://ftp.gnome.org/pub/gnome/sources/NetworkManager/0.8/NetworkManager-${pkgver}.tar.bz2
        nm-system-settings.conf create_dirs.patch dhcpcd.patch disable_set_hostname.patch)
sha256sums=('58e49dcd83cb641a9dcaad4fd566a08196c862479ad3086c00f28f03768eb4f1'
            '44b048804c7c0b8b3b0c29b8632b6ad613c397d0a1635ec918e10c0fbcdadf21'
            '2961ce4cf3079edbf27927a67573ce85dad2268f610fd123dd781eb5ba764c12'
            '528b0bd28565da4711b1f57632352db29401e57e1d88bcefe1624709f9fbf044'
            '1e4586991bc96ef004dc520c794047a336c54433e0bd4edc3879fb6e7ab0e553')

build() {
  cd "${srcdir}/NetworkManager-${pkgver}"

  patch -Np1 -i "${srcdir}/create_dirs.patch"
  patch -Np1 -i "${srcdir}/dhcpcd.patch"
  patch -Np1 -i "${srcdir}/disable_set_hostname.patch"

  autoreconf -fi

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

  install -m644 "${srcdir}/nm-system-settings.conf" "${pkgdir}/etc/NetworkManager/"
  install -m 0755 test/.libs/nm-online "${pkgdir}/usr/bin/"
}
