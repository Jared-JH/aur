# Maintainer: Caleb Maclennan <caleb@alerque.com>

_pkgname=mingzat
pkgname=ttf-sil-$_pkgname
_fname=${_pkgname^}
pkgver=1.000
pkgrel=1
pkgdesc='Unicode font based on Jason Glavy’s JG Lepcha'
arch=('any')
url="https://software.sil.org/$_pkgname"
license=('custom:OFL')
depends=('fontconfig' 'xorg-font-utils')
source=("http://software.sil.org/downloads/r/$_pkgname/$_fname-$pkgver.zip")
sha256sums=('7e1fe44012da7843c63263fe161abd12d4309f9cef00cb58d1055dd812d8e6ba')

package() {
    cd "$_fname-$pkgver"
    find -type f -iname "$_fname*.ttf" -execdir \
        install -Dm644 {} -t "$pkgdir/usr/share/fonts/TTF" \;
    install -Dm644 OFL.txt -t "$pkgdir/usr/share/licenses/$pkgname"
}
