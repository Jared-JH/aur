# Maintainer: netcrusher < tobias AT miglix DOT eu >
# Contributor: Gordian Edenhofer <gordian.edenhofer@gmail.com>

_basename=zoom
pkgname="$_basename-system-qt"
pkgver=5.0.408598.0517
pkgrel=1
pkgdesc="Video Conferencing and Web Conferencing Service - system Qt libraries"
arch=('x86_64')
license=('custom')
url='https://zoom.us/'
conflicts=('zoom')
makedepends=('patchelf')
depends=('ttf-font' 'pulseaudio-alsa'
    'qt5-base' 'qt5-graphicaleffects' 'qt5-imageformats' 'qt5-quickcontrols' 'qt5-quickcontrols2'
    'qt5-svg' 'qt5-script' 'qt5-declarative')
optdepends=('qt5-webengine: SSO login support'
			'picom: extra compositor needed by some window managers for screen sharing'
			'xcompmgr: extra compositor needed by some window managers for screen sharing')
options=(!strip)
source=("${pkgname}-${pkgver}_orig_x86_64.pkg.tar.xz"::"https://zoom.us/client/${pkgver}/zoom_x86_64.pkg.tar.xz")
sha512sums=('5fee6a73e84cef1dfc8f6f7f212c698c701763fbc42681a163ba7dd4e9b0684a950b2236ebd793e19989a82cbd26bb61a647912836489112f19ae4219419e815')

package() {
	cp -dpr --no-preserve=ownership opt usr "${pkgdir}"

    cd "${pkgdir}/opt/zoom"
    patchelf --shrink-rpath zoom
    patchelf --shrink-rpath zopen

    rm -f libQt5*.so{,.*}
    rm -f libicu*.so{,.*}

    rm -rf *integrations
    rm -rf audio
    rm -rf generic
    rm -rf iconengines
    rm -rf imageformats
    rm -rf platforms
    rm -rf platforminputcontexts
    rm -rf platformthemes
    rm -rf Qt{,GraphicalEffects,Qml,Quick,Quick.2}
    rm -f libmpg123.so
    rm -f libfaac1.so
    rm -f libturbojpeg.so{,.*}
    rm -f libquazip.so{,.*}

    rm qt.conf

    chmod -x *.pcm
    chmod -x *.pem
    chmod -x sip/*.{wav,WAV}
}
