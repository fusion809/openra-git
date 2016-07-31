# $Id$
# Maintainer: mnovick1988 <contact through AUR please>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Matthew Bowra-Dean <matthew@ijw.co.nz>
pkgname=openra-git
_pkgname=OpenRA
pkgver=75de73a
pkgrel=1
pkgdesc="An open-source implementation of the Red Alert engine using .NET/Mono and OpenGL"
arch=('any')
url="http://www.openra.net"
license=('GPL3')
install=openra.install
depends=('mono' 'ttf-dejavu' 'openal' 'libgl' 'freetype2' 'sdl2' 'lua51' 'hicolor-icon-theme' 'gtk-update-icon-cache'
         'desktop-file-utils' 'xdg-utils' 'zenity')
makedepends=('git' 'unzip')
provides=('openra')
conflicts=('openra')
options=(!strip)
source=('OpenRA::git://github.com/OpenRA/OpenRA.git#branch=bleed')
md5sums=('SKIP')

pkgver() {
  cd ${srcdir}/${_pkgname}
  git rev-parse --short=7 HEAD
}

build() {
  cd OpenRA

  make dependencies
  make core
  make tools
}

package() {
  cd OpenRA

  make prefix=/usr DESTDIR="$pkgdir" install-all
  make prefix=/usr DESTDIR="$pkgdir" install-linux-shortcuts
  make prefix=/usr DESTDIR="$pkgdir" install-linux-mime
  make prefix=/usr DESTDIR="$pkgdir" install-linux-appdata
}
