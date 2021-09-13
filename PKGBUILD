# Maintainer: Caleb Maclennan <caleb@alerque.com>

pkgname=chrysalis
pkgdesc='Graphical configurator for Kaleidoscope-powered keyboards'
pkgver=0.8.4
pkgrel=2
arch=(x86_64)
url="https://github.com/keyboardio/${pkgname^}"
license=(GPL3)
_electron=electron12
depends=("$_electron"
         fuse2
         uucp)
makedepends=(git
             jq
             moreutils
             node-gyp
             yarn)
_archive="${pkgname^}-$pkgver"
source=("$_archive.tar.gz::$url/archive/v$pkgver.tar.gz"
        "$pkgname.sh")
sha256sums=('e7442fe571a49b09f4fa1a18bd4ec1655212e312b481d9f72732943ee113b8d3'
            '9de3ff052ca4600862b8663b93bf2b4223cf2e637995c67e1fe4cb4ed893b39f')

prepare() {
	local _electronVersion=$($_electron --version | sed -e 's/^v//')
	cd "$_archive"
	sed -i -e '/plugin:prettier/d' .eslintrc.js
	jq 'del(.devDependencies["electron"])' package.json | sponge package.json
	yarn --cache-folder "$srcdir/node_modules" install --frozen-lockfile --ignore-scripts
	yarn --cache-folder "$srcdir/node_modules" add -D --no-lockfile --ignore-scripts electron@$_electronVersion
}

build() {
	cd "$_archive"
	yarn --cache-folder "$srcdir/node_modules" run build:linux
}

package() {
	sed -E "s/electron/$_electron/" "$pkgname.sh" |
		install -Dm0755 -t "$pkgdir/usr/bin/" /dev/stdin
	cd "$_archive"
	local _dist=dist/linux-unpacked/resources
	install -Dm0644 -t "$pkgdir/usr/lib/$pkgname/" "$_dist/app.asar"
	cp -a "$_dist/static" "$pkgdir/usr/lib/$pkgname"
}
