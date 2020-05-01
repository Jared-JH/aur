# Maintainer: Nigel Kukard <nkukard@lbsd.net>
# Maintainer: Caleb Maclennan <caleb@alerque.com>
# Contributor: Maxim Kurnosenko <asusx2@mail.ru>
# Contributor: Xavier Devlamynck <magicrhesus@ouranos.be>
# Contributor: Alessio Biancalana <dottorblaster@gmail.com>
# Contributor: Maik Broemme <mbroemme@libmpq.org>
# Contributor: Denis 'GNUtoo' Carikli <GNUtoo@cyberdimension.org>

pkgname=asterisk
pkgver=17.4.0
pkgrel=1
pkgdesc='A complete PBX solution'
arch=('x86_64' 'i686' 'aarch64' 'armv7h')
url='http://www.asterisk.org'
license=('GPL')
depends=('alsa-lib'
         'curl'
         'jansson'
         'libedit'
         'libvorbis'
         'libxml2'
         'libxslt'
         'opus'
         'pjproject'
         'popt'
         'speex')
makedepends=('gsm'
             'sqlite3')
optdepends=('dahdi'
            'iksemel'
            'libpri'
            'libsrtp'
            'libss7'
            'lua51'
            'openr2'
            'postgresql'
            'unixodbc')
install=$pkgname.install
source=("https://downloads.asterisk.org/pub/telephony/$pkgname/releases/$pkgname-$pkgver.tar.gz"
        "$pkgname.sysusers"
        "$pkgname.logrotated"
        "$pkgname.tmpfile")
sha256sums=('d16ab82ab1cfe505e800a60346d380cd6a2b605a5659d06797a9a4255c0a27dc'
            'fc2e42f79e1672cc25b9b8ad2ba99616fbba0047641c986d30718655d0e7d4d8'
            'caa24cfec5c6b4f8cea385269e39557362acad7e2a552994c3bc24080e3bdd4e'
            '673c0c55bce8068c297f9cdd389402c2d5d5a25e2cf84732cb071198bd6fa78a')

build() {
  cd "$pkgname-$pkgver"
  ./configure \
      --prefix=/usr \
      --sysconfdir=/etc \
      --localstatedir=/var \
      --sbindir=/usr/bin \
      --with-pjproject-bundled=no
  make menuselect.makeopts
  ./menuselect/menuselect --disable BUILD_NATIVE
  make
}

package(){
  cd "$pkgname-$pkgver"
  make DESTDIR="$pkgdir" install
  make DESTDIR="$pkgdir" install-headers
  make DESTDIR="$pkgdir" samples

  # Note you must build the package before you can update meta data!
  backup+=($(cd "$pkgdir" && echo "etc/$pkgname/"*))

  sed -i -e 's,/var/run,/run,' "$pkgdir/etc/asterisk/asterisk.conf"
  install -Dm644 -t "$pkgdir/usr/share/doc/$pkgname/examples" "$pkgdir/etc/asterisk/"*

  mv "$pkgdir/var/run" "$pkgdir"

  pushd contrib/systemd
  install -Dm644 -t "$pkgdir/usr/lib/systemd/system/" "$pkname"*.{service,socket}

  pushd "$srcdir"
  install -Dm644 "$pkgname.sysusers" "$pkgdir/usr/lib/sysusers.d/$pkgname.conf"
  install -Dm644 "$pkgname.logrotated" "$pkgdir/etc/logrotate.d/$pkgname"
  install -Dm644 "$pkgname.tmpfile" "$pkgdir/usr/lib/tmpfiles.d/$pkgname.conf"
}
