# Maintainer: Maurizio Porrato <maurizio.porrato@gmail.com>

pkgname=janus-gateway
pkgver=0.2.6
pkgrel=1
pkgdesc="WebRTC gateway."
arch=('i686' 'x86_64')
url="http://janus.conf.meetecho.com/"
license=('GPL3')
depends=('libmicrohttpd' 'jansson' 'libnice' 'openssl' 'libsrtp'
         'sofia-sip' 'libusrsctp-git' 'libwebsockets' 'libevent' 
         'librabbitmq-c' 'opus' 'libogg' 'glib2' 'ding-libs')
makedepends=('gengetopt' 'paho-mqtt-c-git')
provides=(janus-gateway)
backup=('etc/janus/janus.cfg')
install=janus-gateway.install

source=("https://github.com/meetecho/janus-gateway/archive/v${pkgver}.tar.gz")
sha256sums=('35cbbd5e7b7067e3a0b5ba221ea845814e5b59c733302f88fea80bcf98ce416f')

build() {
  cd "$srcdir/$pkgname-$pkgver"
  ./autogen.sh
  ./configure --prefix=/usr --sysconfdir=/etc --disable-docs
  DESTDIR="$pkgdir/" make
}

package() { 
  cd "$srcdir/$pkgname-$pkgver"
  DESTDIR="$pkgdir/" make install configs
  rm -f "$pkgdir/usr/share/janus/certs/mycert.{key,pem}"
} 
