# Maintainer: Jacob Mischka <jacob@mischka.me>
# Comaintainer: Pedro A. López-Valencia <https://aur.archlinux.org/user/vorbote>
pkgname=brave
pkgver=0.12.7
pkgrel=1
pkgdesc='Web browser that blocks ads and trackers by default.'
arch=('x86_64')
url='https://www.brave.com/'
license=('custom')
depends=('gtk2' 'nss' 'alsa-lib' 'gconf' 'libxtst' 'libxss' 'ttf-font')
makedepends=('npm' 'git')
provides=('brave-browser')
source=("https://github.com/brave/browser-laptop/archive/v"$pkgver"dev.tar.gz"
        "MPL2::https://raw.githubusercontent.com/brave/browser-laptop/master/LICENSE.txt"
	'brave.desktop')
sha512sums=('98df467850984545aae3a0df83b4e1d87db63319fbcf51c9fd5b9c3b99714bd0bf4d0cd40aa61dfb59595492e3de21bd858def730dc4c030cf373bb7c5452742'
            'c1e3a0c8f5267fb5c0b65733bda037c62d914ed989bee0f510d8196b1029eec00d40f415ce1514a4996d420ba02d856d04db0915b64573ef4a36033cc5efb94e'
            '337de1b406c89418fb27a49b1a9fa4cfef94eefbf2901bc60f5ddac0b9094924e940fcbcb233cbbeff2742e6b3c26cc6dbdb75cd29798eff8c8be92fce0df2e7')

build() {
	cd "$srcdir"/browser-laptop-"$pkgver"dev
	#cd "$srcdir"/browser-laptop-"$pkgver"dev-RC5

	npm install
	CHANNEL=dev npm run build-package
}

package() {
	cd "$srcdir"/browser-laptop-"$pkgver"dev
	#cd "$srcdir"/browser-laptop-"$pkgver"dev-RC5

	install -dm0755 "$pkgdir"/usr/lib

	cp -a --reflink=auto Brave-linux-x64 "$pkgdir"/usr/lib/brave

	install -Dm0644 "$srcdir"/brave.desktop "$pkgdir"/usr/share/applications/brave.desktop

	install -dm0755 "$pkgdir"/usr/bin

	ln -s /usr/lib/brave/brave "$pkgdir"/usr/bin/brave

	install -Dm0644 res/app.png "$pkgdir"/usr/share/pixmaps/brave.png

	install -dm0755 "$pkgdir"/usr/share/licenses/brave

	cp --reflink=auto "$srcdir"/MPL2  "$pkgdir"/usr/share/licenses/brave/MPL2

	mv "$pkgdir"/usr/lib/brave/{LICENSE,LICENSES.chromium.html} "$pkgdir"/usr/share/licenses/brave/
}
