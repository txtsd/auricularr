# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Contributor: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://readarr.servarr.com/v1/update/readarr/updatefile?os=linux&runtime=netcore&arch=x64

pkgname=readarr-develop-bin
pkgver=0.4.0.2634
pkgrel=1
pkgdesc="Ebook and audiobook collection manager for newsgroup and torrent users (develop branch)"
arch=('x86_64' 'aarch64' 'armv7h')
url="https://readarr.com"
license=('GPL-3.0-or-later')
groups=('servarr')
depends=(
  'gcc-libs'
  'glibc'
  'zlib'
  'sqlite'
)
optdepends=(
  'sabnzbd: usenet downloader'
  'nzbget: usenet downloader'
  'qbittorrent: torrent downloader'
  'deluge: torrent downloader'
  'rtorrent: torrent downloader'
  'transmission-cli: torrent downloader (CLI and daemon)'
  'transmission-gtk: torrent downloader (GTK+)'
  'transmission-qt: torrent downloader (Qt)'
  'jackett: torrent indexer proxy'
  'nzbhydra2: torznab and usenet indexer proxy'
)
provides=(readarr)
conflicts=(readarr)
options=('!debug')
source=(
  'readarr.service'
  'readarr.tmpfiles'
  'readarr.sysusers'
  'package_info'
)
source_x86_64=("Readarr.develop.${pkgver}.linux-core-x64.tar.gz::https://readarr.servarr.com/v1/update/develop/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Readarr.develop.${pkgver}.linux-core-arm64.tar.gz::https://readarr.servarr.com/v1/update/develop/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Readarr.develop.${pkgver}.linux-core-arm.tar.gz::https://readarr.servarr.com/v1/update/develop/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha512sums=('0c3ba33452bda80bb52cabed7af2bcb592d9af917358ff9bc73338f9587aeeacce0132fb3aa43d00f92465f6d4b8bde844f6883b176765b3891de50ae62ce7b0'
            'b34389cf2966a7a1a1fe6708303641e144191a95001c5ca6e570e9d50ba334fcbc1603852c3c2bfe008d97aaf54207690c689f00dd63378157af33ceebbbb089'
            'fac4d5c35defac54a6d6fe442499e6aab7498c4082f313fa1b008c9880751581d78fea0523ae2a3f4bfa0cf4175763b5d1ea5068d2f327f98e791f5e87115455'
            'e7d23886761a5052d9c9efa24d938bce7ab52b19713a50cc5338f1273bba6615c49ccf1612c412320fd7ff91fff4bff4e95a58db83ae7bd6b6bc83568ffeb90f')
sha512sums_x86_64=('a4f1c7cc7653f2c54dfdc780ee7035626409344980f4d550bbe7c03b447365d46f8d88a02e88194d9600734df299d30cd5b2060dc7a8c5d23bf4077991bee370')
sha512sums_aarch64=('daee5c1ba390425f9bfeaea3d1119e564466f67428d00aa6761ecb75f56db74d1eb4656c80a91835f32043d9ebe0fdd7715384ec5cf39fb9873683981ea4b49b')
sha512sums_armv7h=('7060e09b45422415c39be462e53f5cf8df48934bbc5df366235d5c16d7c953351c7596e5df6d4fb4c29be33cc206a2ce381c5d3db689262b4e6b42e520ad46e9')

package() {
  install -dm755 "${pkgdir}/usr/lib/readarr/bin"

  # License
  install -Dm644 "${srcdir}/Readarr/LICENSE.md" "${pkgdir}/usr/share/licenses/${pkgname}"
  rm "${srcdir}/Readarr/LICENSE.md"

  # Disable built in updater.
  rm -rf "${srcdir}/Readarr/Readarr.Update"
  install -Dm644 "${srcdir}/package_info" "${pkgdir}/usr/lib/readarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/readarr/package_info"

  cp -dpr --no-preserve=ownership "${srcdir}/Readarr/"* "${pkgdir}/usr/lib/readarr/bin"

  install -Dm644 "${srcdir}/readarr.service" "${pkgdir}/usr/lib/systemd/system/readarr.service"
  install -Dm644 "${srcdir}/readarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/readarr.conf"
  install -Dm644 "${srcdir}/readarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/readarr.conf"
}
