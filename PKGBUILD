# Maintainer: Jacob Mischka <jacob@mischka.me>
# Comaintainer: Pedro A. López-Valencia <https://aur.archlinux.org/user/vorbote>
pkgname=brave
pkgver=0.13.2
pkgrel=1
pkgdesc='Web browser that blocks ads and trackers by default.'
arch=('x86_64')
url='https://www.brave.com/'
license=('custom')
depends=('gtk2' 'nss' 'alsa-lib' 'gconf' 'libgnome-keyring' 'libxtst' 'libxss' 'ttf-font')
makedepends=('npm' 'git')
optdepends=('cups: Printer support'
            'pepper-flash: Adobe Flash support')
provides=('brave-browser')
source=("https://github.com/brave/browser-laptop/archive/v"$pkgver"dev.tar.gz"
        'brave'
	'brave.desktop')
sha512sums=('64e49871474da07c156189636ab19b2e17b3649881461c2d302b9f8e98e951472220f0e5913c79bdfcb3843c66ebf42c048c819da705073418a68584f82c0ed6'
            '658f45baee593b88694cbdd4b23d8ea6425a39a54a52363a63abf43951cbdc586086a3f90bbbcb658706676166db30f51339dc548cdfbb26b62f00a62e2a0313'
            '337de1b406c89418fb27a49b1a9fa4cfef94eefbf2901bc60f5ddac0b9094924e940fcbcb233cbbeff2742e6b3c26cc6dbdb75cd29798eff8c8be92fce0df2e7')

build() {
	cd "$srcdir"/browser-laptop-"$pkgver"dev

	npm install
	CHANNEL=dev npm run build-package
}

package() {
	cd "$srcdir"/browser-laptop-"$pkgver"dev

	install -dm0755 "$pkgdir"/usr/lib

	cp -a --reflink=auto Brave-linux-x64 "$pkgdir"/usr/lib/brave

	install -Dm0644 "$srcdir"/brave.desktop "$pkgdir"/usr/share/applications/brave.desktop

	install -Dm0755 "$srcdir"/brave "$pkgdir"/usr/bin/brave

	install -Dm0644 res/app.png "$pkgdir"/usr/share/pixmaps/brave.png

	install -Dm0644 LICENSE.txt "$pkgdir"/usr/share/licenses/brave/MPL2

	mv "$pkgdir"/usr/lib/brave/{LICENSE,LICENSES.chromium.html} "$pkgdir"/usr/share/licenses/brave/

	ln -s /usr/lib/PepperFlash "$pkgdir"/usr/lib/pepperflashplugin-nonfree
}
