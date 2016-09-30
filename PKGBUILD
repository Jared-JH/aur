# Maintainer: Please see AUR package page for current maintainer(s) and contact information.

pkgname=brave-bin
pkgver=0.12.3
pkgrel=1
pkgdesc="A web browser that stops ads and trackers by default. Binary release."
arch=('x86_64') # Upstream supports x86_64 only
url="https://www.brave.com/"
license=('custom:several')
depends=('gtk2' 'nss' 'alsa-lib' 'gconf' 'libxtst' 'libxss' 'libgnome-keyring' 'ttf-font')
optdepends=('cups: Printer support')
provides=('brave' 'brave-browser')
conflicts=('brave')
replaces=('brave-browser-bin')
#source=("$pkgname-$pkgver.tar.bz2::https://github.com/brave/browser-laptop/releases/download/"$pkgver"dev/Brave.tar.bz2"
source=("$pkgname-0.12.1dev.tar.bz2::https://github.com/brave/browser-laptop/releases/download/0.12.1dev/Brave.tar.bz2"
        "brave.png::https://github.com/brave/browser-laptop/raw/master/res/app.png"
        "MPL2::https://raw.githubusercontent.com/brave/browser-laptop/master/LICENSE.txt"
        "brave.desktop")
options=(!strip)
sha384sums=('bf45817ac83d15e5b66ff0416da4c04141930645ef3ddfc523751524fc923f902b37fe8a198028cefd37fc2967f62448'
            'dd60b1f5ee6817784db42d9595e318b02d60a9eb4375546eced89de8e5eaa4c778f9110869c4d8e795e7f69d9c410568'
            'b27caa103555393992e6e1de1c2663f3ecf8339054e1aee8961406c8cbc9d677ba78b4bab6efe7210143818f9207d16b'
            'f950675fb4a3f9e48374f8a2667e7a45889206a3062c8182e474143607fc26bd17e852a1ef494607dbd3ff4de325e05f')


package() {
  install -d -m0755 "$pkgdir"/usr/lib

  cp -a --reflink=auto "$srcdir"/Brave-linux-x64 "$pkgdir"/usr/lib/brave

  install -d -m0755 "$pkgdir"/usr/bin

  ln -s /usr/lib/brave/brave "$pkgdir"/usr/bin/brave

  install -dm0755 "$pkgdir"/usr/share/applications

  cp --reflink=auto "$startdir"/brave.desktop "$pkgdir"/usr/share/applications/brave.desktop

  install -dm0755 "$pkgdir"/usr/share/pixmaps

  cp --reflink=auto "$srcdir"/brave.png "$pkgdir"/usr/share/pixmaps/

  install -d -m0755 "$pkgdir"/usr/share/licenses/brave-bin

  cp --reflink=auto "$srcdir"/MPL2 "$pkgdir"/usr/share/licenses/brave-bin/MPL2

  mv "$pkgdir"/usr/lib/brave/{LICENSE,LICENSES.chromium.html} "$pkgdir"/usr/share/licenses/brave-bin
}
