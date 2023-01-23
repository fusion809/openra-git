# Maintainer: Brenton Horne <brentonhorne77@gmail.com>

pkgname=openra-git
pkgver=29112.git.3dd1fd6
#_commit=c55c65f
#_pr=17592
pkgrel=1
pkgdesc="An open-source recreation of the early Command & Conquer games, built from latest git snapshot"
arch=('any')
url="https://www.openra.net"
license=('GPL3')
install=${pkgname}.install
depends=('mono' 'ttf-dejavu' 'openal' 'libgl' 'freetype2' 'sdl2' 'lua51' 'hicolor-icon-theme' 'gtk-update-icon-cache'
         'desktop-file-utils' 'xdg-utils' 'zenity')
makedepends=('git' 'unzip' 'mono-msbuild')
conflicts=('openra' 'openra-bleed')
options=(!strip)
if [[ -n ${_pr} ]]; then
source=("git+https://github.com/OpenRA/OpenRA.git"
"https://raw.githubusercontent.com/wiki/OpenRA/OpenRA/Changelog.md")
else
source=("git+https://github.com/OpenRA/OpenRA.git"
"https://raw.githubusercontent.com/wiki/OpenRA/OpenRA/Changelog.md")
fi
sha256sums=('SKIP'
            '28798bd8ff9c696524812b33122df591daf03baa03619a8f612f25d10d90e371')

pkgver() {
	cd $srcdir/OpenRA
	if [[ -n ${_commit} ]]; then
		git checkout ${_commit}
		no=$(git rev-list --count ${_commit})
		hash=${_commit}
		printf ${no}.git.${_commit}
	else
		no=$(git rev-list --count HEAD)
		hash=$(git log | head -n 1 | cut -d ' ' -f 2 | head -c 7)
		printf "${no}.git.${hash}"
        fi
}

prepare() {
    cd $srcdir/OpenRA
    make version VERSION="${pkgver}"
}

build() {
    cd $srcdir/OpenRA
    make all RUNTIME=mono DEBUG=false TARGETPLATFORM=unix-generic
}

package() {
    cd $srcdir/OpenRA
    mkdir -p $pkgdir/usr/lib/openra
    make prefix=/usr DESTDIR="$pkgdir" install DEBUG=false RUNTIME=mono
    make prefix=/usr DESTDIR="$pkgdir" install-linux-shortcuts DEBUG=false   
    make prefix=/usr DESTDIR="$pkgdir" install-linux-appdata DEBUG=false
    cp $pkgdir/usr/bin/openra-ra $pkgdir/usr/bin/openra-ts
    cp $pkgdir/usr/share/applications/openra-ra.desktop $pkgdir/usr/share/applications/openra-ts.desktop
    sed -i -e "s|-ra|-ts|g" \
       -e "s|=ra|=ts|g" \
       -e "s|Red Alert|Tiberian Sun|g" $pkgdir/usr/bin/openra-ts
    sed -i -e "s|-ra|-ts|g" \
       -e "s|Red Alert|Tiberian Sun|g" $pkgdir/usr/share/applications/openra-ts.desktop
}
