# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Contributor: Donald Webster <fryfrog@gmail.com>

pkgname=recyclarr-bin
_pkgname=${pkgname%%-bin}
pkgver=7.2.4
pkgrel=1
pkgdesc="Automatically synchronize recommended settings from the TRaSH guides to your Sonarr/Radarr instances."
arch=('x86_64' 'aarch64' 'armv7h')
url="https://recyclarr.dev"
license=('MIT')
depends=(
  'gcc-libs'
  'git'
  'glibc'
  'sqlite'
  'zlib'
)
optdepends=(
  'sonarr: Smart PVR for newsgroup and bittorrent users.'
  'radarr: Movie organizer/manager for usenet and torrent users.'
)
provides=(recyclarr)
conflicts=(recyclarr)
backup=('etc/recyclarr/recyclarr.yml')
options=('!debug' '!strip')
source=(
  'recyclarr.yml'
  'recyclarr.service'
  'recyclarr.timer'
  'recyclarr.tmpfiles'
  'recyclarr.sysusers'
)
source_x86_64=("${_pkgname}-${pkgver}.linux-x64.tar.xz::https://github.com/recyclarr/recyclarr/releases/download/v${pkgver}/recyclarr-linux-x64.tar.xz")
source_aarch64=("${_pkgname}-${pkgver}.linux-arm64.tar.xz::https://github.com/recyclarr/recyclarr/releases/download/v${pkgver}/recyclarr-linux-arm64.tar.xz")
source_armv7h=("${_pkgname}-${pkgver}.linux-arm.tar.xz::https://github.com/recyclarr/recyclarr/releases/download/v5.4.0/recyclarr-linux-arm.tar.xz")
sha256sums=('f0b6b437fad6072f55be0eb57c4eaf6a44eecda4588633edd5ad716ea3e41c7d'
            'c03ea99bdea959b9da8a71556f58980ffd3967ee6cbab4c11945ab1aebd52246'
            'e8a2959e079a6a77c3eefaf77defd69e76944c2a1378257dcaf0286abde002a6'
            '458b7c0550f3c2e41f63bac197ce55a5699432ee24080f7917b001c0eec2c7ec'
            '3d2a1b3690d956a8f195c2cd1b28c28beecda354023e8de78471ca35610fb57d')
sha256sums_x86_64=('07a3749ca5883e6702ea0e8b9f505429d791917ad502f4e60d78ce54d67561a6')
sha256sums_aarch64=('618379e5183a459d7904954e93507e606a80671f097d5e2967acd21a93341d6a')
sha256sums_armv7h=('e0dee2586c738520fe83012ebb587dacd5e4a0cdb6d74542ea43b57f6a09c93e')

package() {
  install -Dm755 "${srcdir}/recyclarr" "${pkgdir}/usr/bin/recyclarr"
  install -Dm600 "${srcdir}/recyclarr.yml" "${pkgdir}/etc/recyclarr/recyclarr.yml"
  install -Dm644 "${srcdir}/recyclarr.service" "${pkgdir}/usr/lib/systemd/system/recyclarr.service"
  install -Dm644 "${srcdir}/recyclarr.timer" "${pkgdir}/usr/lib/systemd/system/recyclarr.timer"
  install -Dm644 "${srcdir}/recyclarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/recyclarr.conf"
  install -Dm644 "${srcdir}/recyclarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/recyclarr.conf"
}
