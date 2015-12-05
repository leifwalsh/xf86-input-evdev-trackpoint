# Maintainer: Taegil Bae <esrevinu@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: Alexander Baldeck <Alexander@archlinux.org

_pkgname=xf86-input-evdev
pkgname=${_pkgname}-trackpoint
pkgver=2.10.0
pkgrel=1
_pkgname2=xf86-input-synaptics
_pkgver2=1.8.3
pkgdesc="X.org evdev input driver for trackpoint with clickpad"
arch=(i686 x86_64)
url="http://xorg.freedesktop.org/"
license=('custom')
depends=('glibc' 'systemd' 'mtdev' 'libxtst' 'libevdev')
makedepends=('xorg-server-devel' 'resourceproto' 'scrnsaverproto' 'libxi' 'libx11' 'X-ABI-XINPUT_VERSION>=19' 'autoconf')
conflicts=('xorg-server<1.18' 'X-ABI-XINPUT_VERSION<22' 'X-ABI-XINPUT_VERSION>=23' 'xf86-input-evdev')
replaces=("${_pkgname}")
provides=("${_pkgname}" 'xf86-input-driver')
options=('!makeflags')
backup=('etc/X11/xorg.conf.d/90-evdev-trackpoint.conf')
source=(${url}/releases/individual/driver/${_pkgname}-${pkgver}.tar.bz2
	${url}/releases/individual/driver/${_pkgname2}-${_pkgver2}.tar.bz2
	0001-implement-trackpoint-wheel-emulation.patch
	0004-disable-clickpad_guess_clickfingers.patch
	0006-add-synatics-files-into-Makefile.am.patch
	90-evdev-trackpoint.conf)
sha256sums=('d097298eb07b7a9edf4493b5c3c058041458ca52c8c62dbd4f40b84c5086d117'
            'd39f100c74f3673778b53f17bab7690161925e25dd998a15dd8cc69b52e83f01'
            '488f56a9c74182d0b0bf802ba35881d3ebc16efcf9dfceea01c02dbceb332e30'
            '960c5fca31986aadb71b89ba9c9b7b60046334f1b18c574617e70a0c15ebb582'
            '1df4e443efe4cc7ef2f142e206b97966d9e588d7fb4f507f6166aa9a848f1547'
            '2278cb9b452524cc3fe17071efa2d5b5c8064628b440997fdfa7f999dbf29b62')
install=xf86-input-evdev-trackpoint.install

prepare() {
  synaptics_srcdir=${srcdir}/${_pkgname2}-${_pkgver2}

  cd "${srcdir}/${_pkgname}-${pkgver}"

  cp ${synaptics_srcdir}/src/{eventcomm.c,eventcomm.h,properties.c,\
synaptics.c,synapticsstr.h,synproto.c,synproto.h} src/
  cp ${synaptics_srcdir}/include/synaptics-properties.h include/

  patch -p1 -i ../0001-implement-trackpoint-wheel-emulation.patch
  patch -p1 -i ../0004-disable-clickpad_guess_clickfingers.patch
  patch -p1 -i ../0006-add-synatics-files-into-Makefile.am.patch
}

build() {
  cd "${srcdir}/${_pkgname}-${pkgver}"

  autoreconf -i
  ./configure --prefix=/usr
  make
}

package() {
  cd "${srcdir}/${_pkgname}-${pkgver}"
  make DESTDIR="${pkgdir}" install
  install -m755 -d "${pkgdir}/usr/share/licenses/${pkgname}"
  install -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/"
  install -m755 -d "${pkgdir}/etc/X11/xorg.conf.d"
  install -m644 ${srcdir}/90-evdev-trackpoint.conf "${pkgdir}/etc/X11/xorg.conf.d/"
}
