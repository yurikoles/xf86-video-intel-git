# Maintainer: Yurii Kolesnykov <yurikoles@gmail.com>
# Contributor: AndyRTR <andyrtr@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgname=xf86-video-intel-git
_pkgname=xf86-video-intel
_branch=master
pkgver=2.99.917+781+gc8990575
pkgrel=1
epoch=1
arch=(i686 x86_64)
url="https://01.org/linuxgraphics"
license=('custom')
install=$pkgname.install
pkgdesc="X.org Intel i810/i830/i915/945G/G965+ video drivers"
depends=('mesa' 'libxvmc' 'pixman' 'xcb-util>=0.3.9' 'libsystemd')
makedepends=('xorg-server-devel' 'X-ABI-VIDEODRV_VERSION=23' 'libx11' 'libxrender'
             # additional deps for intel-virtual-output
             'libxrandr' 'libxinerama' 'libxcursor' 'libxtst' 'libxss'
             # additional for git snapshot
             'git')
optdepends=('libxrandr: for intel-virtual-output'
            'libxinerama: for intel-virtual-output'
            'libxcursor: for intel-virtual-output'
            'libxtst: for intel-virtual-output'
            'libxss: for intel-virtual-output')
replaces=('xf86-video-intel-uxa' 'xf86-video-intel-sna' 'xf86-video-intel')
provides=('xf86-video-intel-uxa' 'xf86-video-intel-sna' 'xf86-video-intel')
conflicts=('xorg-server<1.19' 'X-ABI-VIDEODRV_VERSION<23' 'X-ABI-VIDEODRV_VERSION>=24'
           'xf86-video-intel-sna' 'xf86-video-intel-uxa' 'xf86-video-i810' 'xf86-video-intel-legacy'
           "${_pkgname}")
groups=('xorg-drivers')
source=("$pkgname::git://anongit.freedesktop.org/xorg/driver/${_pkgname}#branch=${_branch}")
sha256sums=('SKIP')

pkgver() {
  cd $pkgname
  git describe --long | sed 's/-/+/g'
}

prepare() {
  cd $pkgname
  NOCONFIGURE=1 ./autogen.sh
}

build() {
  cd $pkgname

  # Since pacman 5.0.2-2, hardened flags are now enabled in makepkg.conf
  # With them, module fail to load with undefined symbol.
  # See https://bugs.archlinux.org/task/55102 / https://bugs.archlinux.org/task/54845
  export CFLAGS=${CFLAGS/-fno-plt}
  export CXXFLAGS=${CXXFLAGS/-fno-plt}
  export LDFLAGS=${LDFLAGS/,-z,now}

  ./configure --prefix=/usr \
    --libexecdir=/usr/lib \
    --with-default-dri=3
  make
}

check() {
  cd $pkgname
  make check
}

package() {
  cd $pkgname

  make DESTDIR="${pkgdir}" install

  install -m755 -d "${pkgdir}/usr/share/licenses/${pkgname}"
  install -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/"
}
