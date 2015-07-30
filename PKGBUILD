# Maintainer: ulis <fer.obbee at gmail dot com>

pkgname=otf-fira-code-git
pkgver=0.5.r2.g44d5c4e
pkgrel=1
pkgdesc="Fira Code: monospaced font with programming ligatures."
arch=('any')
url='https://github.com/tonsky/FiraCode.git'
license=('OFL')
depends=('fontconfig' 'xorg-font-utils')
makedepends=('git')
install=otf.install
source=('git+https://github.com/tonsky/FiraCode.git' 'otf.install')
md5sums=('SKIP'
         '9b9dbd9712ac6fefe896f704609b659c')

_gitname=FiraCode

pkgver() {
	cd "$srcdir/$_gitname"
	git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

package() {
	cd "$srcdir/$_gitname"

	local font='FiraCode-Regular.otf'
	install -Dm644 "$font" "${pkgdir}/usr/share/fonts/OTF/$font"
}
