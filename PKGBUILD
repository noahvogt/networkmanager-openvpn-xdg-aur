# Maintainer: Noah Vogt (noahvogt) <noah@noahvogt.com>
# Contributor: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>

pkgname=networkmanager-openvpn-xdg
pkgver=1.10.4
pkgrel=1
pkgdesc="NetworkManager VPN plugin for OpenVPN - with Support for the XDG Base Directory Specification"
url="https://networkmanager.dev/docs/vpn/"
arch=(x86_64)
license=(GPL-2.0-or-later)
depends=(
  gcc-libs
  glib2
  glibc
  libnm
  libsecret
  openvpn
)
makedepends=(
  git
  libnma
  libnma-gtk4
  python
)
optdepends=(
  'libnma-gtk4: GUI support (GTK 4)'
  'libnma: GUI support (GTK 3)'
)
# Can't locate the public key (ihuguet@redhat.com, 07F9AEC86144386D9576210B66A44781B4EBC2D0)
source=(
  "git+https://gitlab.gnome.org/GNOME/NetworkManager-openvpn.git#tag=$pkgver"
        "xdg-basedir.patch")
b2sums=('5498e9d65e2bdc2e2b473e80e1379b44aa8041bfef88d345c74e24760c28c32a164fe48f353039b5415aa94b2ecee4a44e72741510de8994a9eb2aff33cbd1c1'
        '12e4e6fbe9c3212636825e3686dfcdc618d7fb410c6e9e7a8d286918a50356e5ea6e51d387f25fa78655a1a155aacc44cf8714063342e464034a17b4d0b4b498')
provides=('networkmanager-openvpn')
conflicts=('networkmanager-openvpn')

pkgver() {
  cd NetworkManager-openvpn
  git describe --tags | sed 's/-dev/dev/;s/[^-]*-g/r&/;s/-/+/g'
}

prepare() {
  cd NetworkManager-openvpn
  patch -Np1 -i ../xdg-basedir.patch
  autoreconf -fvi
}

build() {
  local configure_options=(
    --prefix=/usr
    --sysconfdir=/etc
    --localstatedir=/var
    --libexecdir=/usr/lib
    --disable-static
    --with-gtk4
  )


  cd NetworkManager-openvpn
  ./configure "${configure_options[@]}"
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool
  make
}

package() {
  cd NetworkManager-openvpn
  make DESTDIR="$pkgdir" install dbusservicedir=/usr/share/dbus-1/system.d
  echo 'u nm-openvpn - "NetworkManager OpenVPN"' |
    install -Dm644 /dev/stdin "$pkgdir/usr/lib/sysusers.d/$pkgname.conf"
}

# vim:set sw=2 et:
