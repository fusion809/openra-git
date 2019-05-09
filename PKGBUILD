# Maintainer: Brenton Horne <brentonhorne77@gmail.com>

pkgname=openra-git
pkgver=25994.git.ba28286
pkgrel=1
pkgdesc="An open-source recreation of the early Command & Conquer games, built from latest git snapshot"
arch=('any')
url="https://www.openra.net"
license=('GPL3')
install=${pkgname}.install
depends=('mono' 'openal' 'libgl' 'freetype2' 'sdl2' 'lua51' 'hicolor-icon-theme' 'desktop-file-utils' 'xdg-utils' 'zenity')
makedepends=('dos2unix' 'msbuild')
conflicts=('openra' 'openra-bleed')
options=(!strip)
source=("git+https://github.com/OpenRA/OpenRA.git"
"https://github.com/OpenRA/OpenRA/pull/16511.patch"
"http://geolite.maxmind.com/download/geoip/database/GeoLite2-Country.mmdb.gz"
"https://raw.githubusercontent.com/wiki/OpenRA/OpenRA/Changelog.md")
sha256sums=('SKIP'
            '8b0c1b0d6c8884753f5b192f6f8055caaf3a36512e4fb2c32b6c120a44f720f4'
            '146df390479eaf249a1b390530b88151cb9ef1f85b52c2baa071ffee46dc770b'
            '28798bd8ff9c696524812b33122df591daf03baa03619a8f612f25d10d90e371')

pkgver() {
	cd $srcdir/OpenRA
	no=$(git rev-list --count HEAD)
	hash=$(git log | head -n 1 | cut -d ' ' -f 2 | head -c 7)
	printf "${no}.git.${hash}"
}

prepare() {
    cd $srcdir/OpenRA
    cp $srcdir/Changelog.md .
    dos2unix Changelog.md
	patch -Np1 -i $srcdir/16511.patch
}

build() {
    cd $srcdir/OpenRA
    make version VERSION="${pkgver}"
    make dependencies
    make core SDK="-sdk:4.5"
}

package() {
    cd $srcdir/OpenRA
    make DESTDIR=${pkgdir} prefix=/usr install-core
    make DESTDIR=${pkgdir} prefix=/usr install-linux-shortcuts
    make DESTDIR=${pkgdir} prefix=/usr install-linux-mime
    make DESTDIR=${pkgdir} prefix=/usr install-linux-appdata
    make DESTDIR=${pkgdir} prefix=/usr install-man-page

    rm $pkgdir/usr/lib/openra/*.sh
    cp -a mods/ts $pkgdir/usr/lib/openra/mods
    mkdir $pkgdir/usr/share/pixmaps -p
    cp mods/ts/icon.png $pkgdir/usr/share/pixmaps/openra-ts.png
    cp $pkgdir/usr/bin/openra-ra $pkgdir/usr/bin/openra-ts
    cp $pkgdir/usr/share/applications/openra-ra.desktop $pkgdir/usr/share/applications/openra-ts.desktop
    sed -i -e "s|-ra|-ts|g" \
       -e "s|=ra|=ts|g" \
       -e "s|Red Alert|Tiberian Sun|g" $pkgdir/usr/bin/openra-ts
    sed -i -e "s|-ra|-ts|g" \
       -e "s|Red Alert|Tiberian Sun|g" $pkgdir/usr/share/applications/openra-ts.desktop
}