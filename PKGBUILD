# Contributor: Caleb Maclennan <caleb@alerque.com>
# Contributor: Jacob Mischka <jacob@mischka.me>
# Contributor: Manuel Mazzuola <origin.of@gmail.com>
# Contributor: Simón Oroño <simonorono@protonmail.com>
# Contributor: now-im <now im 627 @ gmail . com>
# Contributor: Giusy Digital <kurmikon at libero dot it>
# Mantainer: Andrés Rodríguez <hello@andres.codes>
# https://aur.archlinux.org/packages/brave-bin/

pkgname=brave-bin
pkgver=1.24.85
pkgrel=2
epoch=1
pkgdesc="Web browser that blocks ads and trackers by default (binary release)."
arch=("x86_64")
url="https://brave.com/download"
license=("MPL2" "BSD" "custom:chromium")
depends=("gtk3" "nss" "alsa-lib" "libxss" "ttf-font")
optdepends=("cups: Printer support"
            "libgnome-keyring: Enable GNOME keyring support"
            "libnotify: Native notification support")
provides=("${pkgname%-bin}" "brave-browser")
conflicts=("${pkgname%-bin}")
source=("$pkgname-$pkgver.zip::https://github.com/brave/brave-browser/releases/download/v${pkgver}/brave-browser-${pkgver}-linux-amd64.zip"
        "$pkgname.sh"
        "brave-browser.desktop"
        "logo.png")
options=(!strip)
sha512sums=("efb94318bcedac6c2c467e7c7addafb465ff777ff17a13a3d771ae43f11cac403de1c17a540d0c8f4a8a400b551d973a9e8c66a7c2c0be3d3cf6c7fef4be372f"
            "6912f1e0b20f16078e15ce75eca61b29fd5c415c63df9bd69a547d402bf00599fdcbe1c0898b8d2dc865dc1b604f81e2ec21525ecc61a0fcafba0d9f87434540"
            "847b3f5f7c58b36d45579d75bd0086e21bf6aef3948d24a1e8f091bc9d53dfc1e430a0a6cc43dc02ae9d9041aabdf924ff47704993d747e28329fe3c1d70e23e"
            "d7bef52e336bd908d24bf3a084a1fc480831d27a3c80af4c31872465b6a0ce39bdf298e620ae9865526c974465807559cc75610b835e60b4358f65a8a8ff159e")
noextract=("$pkgname-$pkgver.zip")

prepare() {
  mkdir -p brave
  bsdtar -xf $pkgname-$pkgver.zip -C brave
  chmod +x brave/brave
}

_bsdtardir="brave"

package() {
    install -d -m0755 "$pkgdir/usr/lib"
    cp -a $_bsdtardir "$pkgdir/usr/lib/$pkgname"

    # allow firejail users to get the suid sandbox working
    chmod 4755 $pkgdir/usr/lib/brave-bin/chrome-sandbox

    install -Dm0755 "$pkgname.sh" "$pkgdir/usr/bin/brave"
    install -Dm0644 -t "$pkgdir/usr/share/applications" "brave-browser.desktop"
    install -Dm0644 "logo.png" "$pkgdir/usr/share/pixmaps/brave-desktop.png"
    LICENSES_DIR="$pkgdir/usr/share/licenses/$pkgname"
    mkdir -p "$LICENSES_DIR"
    if [ -f "$pkgdir/usr/lib/$pkgname/LICENSE" ] && [ -f "$pkgdir/usr/lib/$pkgname/LICENSES.chromium.html" ]; then
      mv "$pkgdir/usr/lib/$pkgname/"{LICENSE,LICENSES.chromium.html} "$LICENSES_DIR"
    fi
}
