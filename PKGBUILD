# Maintainer: Terin Stock <terinjokes@gmail.com>

pkgname=gojq
pkgver=0.12.5
_pkgrev=727b4b5
pkgrel=1
pkgdesc='Pure go implementation of jq'
arch=('x86_64')
url="https://github.com/itchyny/gojq"
license=('MIT')
makedepends=('go')
depends=('glibc')
source=("${pkgname}-${pkgver}.tar.gz::https://github.com/itchyny/gojq/archive/v${pkgver}.tar.gz")
sha256sums=('69616c38cc9bcdef06d32692e1cc1f330980c523fc777b07a851a52b51fffdac')

prepare(){
  cd "${pkgname}-${pkgver}"
  mkdir -p build/
}

build() {
  cd "${pkgname}-${pkgver}"
  export CGO_LDFLAGS="${LDFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export GOFLAGS="-buildmode=pie -trimpath -ldflags=-linkmode=external -mod=readonly -modcacherw"
  go build -o build -ldflags="-X github.com/itchyny/gojq/cli.revision=${_pkgrev}" ./cmd/gojq
}

check() {
  cd "${pkgname}-${pkgver}"
  go test ./...
}

package() {
  cd "${pkgname}-${pkgver}"

  install -Dm755 "build/${pkgname}" "${pkgdir}/usr/bin/${pkgname}"
  install -Dm755 _gojq "${pkgdir}/usr/share/zsh/site-functions/_gojq"
  install -Dm644 LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}"
}
