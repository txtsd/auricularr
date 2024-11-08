# Maintainer: txtsd <aur.archlinux@ihavea.quest>

pkgname=bitmagnet
pkgver=0.9.5
pkgrel=2
pkgdesc='A self-hosted BitTorrent indexer, DHT crawler, content classifier and torrent search engine with web UI, GraphQL API and Servarr stack integration.'
arch=(x86_64 aarch64 armv7h)
url='https://bitmagnet.io'
license=('MIT')
depends=(glibc)
makedepends=(go)
optdepends=(
  'grafana: dashboards'
  'loki: logs'
  'postgresql: database'
  'prometheus: metrics'
  'pyroscope: profiling'
  'redis: cache'
)
backup=(etc/bitmagnet/config.yml)
source=(
  "${pkgname}-${pkgver}.tar.gz::https://github.com/bitmagnet-io/bitmagnet/archive/refs/tags/v${pkgver}.tar.gz"
  config.yml
  bitmagnet.service
  bitmagnet.sysusers
  bitmagnet.tmpfiles
)
sha256sums=('022ff59dc8066dbdbe8273fc1aff050391806994fbaf8ec0381dc10e950c7cfc'
            'e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855'
            'e9f8ee3b561297d673ca52c6e2dd98a061a4d5569542d3cd80703c25e854d404'
            '9eab02f76a22e3ff627bd6f7fe609480ced67c076f079e985a34d41d13fe8f08'
            '0972b20f948b5e685c659dfefa7a4e200f75fa12dfe9d4779ef3ad5c8ffdca86')

prepare() {
  cd "${pkgname}-${pkgver}"

  export GOPATH="${srcdir}"
  go mod download
}

build() {
  cd "${pkgname}-${pkgver}"

  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export CGO_LDFLAGS="${LDFLAGS}"
  export GOPATH="${srcdir}"
  export GOFLAGS="\
    -buildmode=pie \
    -mod=readonly \
    -modcacherw \
    -trimpath \
  "
  local _ld_flags=" \
    -compressdwarf=false \
    -linkmode=external \
    -X github.com/bitmagnet-io/bitmagnet/internal/version.GitTag=v${pkgver} \
  "
  go build \
    -ldflags "${_ldflags}" \
    -o "${pkgname}"
}

package() {
  cd "${pkgname}-${pkgver}"

  install -Dm755 "${pkgname}" "${pkgdir}/usr/bin/${pkgname}"
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  install -Dm644 "${srcdir}/config.yml" "${pkgdir}/etc/bitmagnet/config.yml"
  install -Dm644 "${srcdir}/bitmagnet.service" "${pkgdir}/usr/lib/systemd/system/bitmagnet.service"
  install -Dm644 "${srcdir}/bitmagnet.sysusers" "${pkgdir}/usr/lib/sysusers.d/bitmagnet.conf"
  install -Dm644 "${srcdir}/bitmagnet.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/bitmagnet.conf"

  install -dm755 "${pkgdir}/usr/share/${pkgname}"
  cp -dpr --no-preserve=ownership observability "${pkgdir}/usr/share/${pkgname}/"
}
