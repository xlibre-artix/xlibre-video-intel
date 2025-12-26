# Maintainer: artist for Artix Linux

pkgname=xlibre-video-intel
pkgver=25.0.0
pkgrel=3
arch=(x86_64)
license=('MIT')
install=$pkgname.install
pkgdesc="Libre fork of X.Org Intel i810/i830/i915/945G/G965+ video drivers"
_pkgname="${pkgname//xlibre/xf86}"
url="https://github.com/X11Libre/${_pkgname}"
depends=("xlibre-xserver>=${pkgver%.*}" 'glibc')
makedepends=("xlibre-xserver-devel>=${pkgver%.*}" 'xorgproto')
conflicts=("${_pkgname}")
provides=("${_pkgname}")
source=("${url}/archive/refs/tags/xlibre-${_pkgname}-${pkgver}.tar.gz")
groups=('xlibre-drivers')
depends+=('mesa' 'libxvmc' 'pixman' 'xcb-util>=0.3.9' 'libudev'
         'libxcb' 'libxfixes' 'libxshmfence' 'libdrm' 'libxrender'
         'libx11' 'libxdamage' 'libxext' 'libpciaccess')
makedepends+=('libxv' 'meson'
             # additional deps for intel-virtual-output
             'libxrandr' 'libxinerama' 'libxcursor' 'libxtst' 'libxss')
optdepends=('libxrandr: for intel-virtual-output'
            'libxinerama: for intel-virtual-output'
            'libxcursor: for intel-virtual-output'
            'libxtst: for intel-virtual-output'
            'libxss: for intel-virtual-output')
provides+=('xf86-video-intel-uxa' 'xf86-video-intel-sna')
replaces=('xf86-video-intel-sna' 'xf86-video-intel-uxa'
          'xf86-video-i810' 'xf86-video-intel-legacy')
options=('!lto')

build() {
  export CFLAGS=${CFLAGS/-fno-plt}
  export CFLAGS+=" -Wno-incompatible-pointer-types"
  export CXXFLAGS=${CXXFLAGS/-fno-plt}
  export LDFLAGS=${LDFLAGS/-Wl,-z,now}
  # Prevent build errors with meson (need to fiel bug):
  export CFLAGS+=" -I${srcdir}/build"
  #export CXXFLAGS+=" -I${srcdir}/build"
  
  artix-meson ${_pkgname}-xlibre-${_pkgname}-${pkgver} build \
    -D dri3=true \
    -D tearfree=true \
    -D valgrind=false

  meson configure build
  ninja -C build
}

check() {
  meson test -C build
}

package() {
  DESTDIR="$pkgdir" ninja -C build install

  install -Dm644 "${srcdir}"/${_pkgname}-xlibre-${_pkgname}-${pkgver}/COPYING "${pkgdir}"/usr/share/licenses/${pkgname}/LICENSE
}

sha256sums=('c37410bc6efd5789adcd9fd2c91308a73101b08419309d4755bd6b5fa5254eff')
