# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://readarr.servarr.com/v1/update/develop?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=readarr-develop-bin
pkgver=0.4.1.2648
pkgrel=1
pkgdesc='Ebook and audiobook collection manager for newsgroup and torrent users (develop branch)'
arch=('x86_64' 'aarch64' 'armv7h')
url='https://readarr.com'
license=('GPL-3.0-or-later')
groups=('servarr-develop-bin')
depends=(
  'gcc-libs'
  'glibc'
  'zlib'
  'sqlite'
)
optdepends=(
  'postgresql: postgresql database'
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
  'prowlarr: torrent and usenet indexer proxy'
  'autobrr: irc, torrent and usenet indexer proxy'
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
sha256sums=('09386a5a87038f227e4a0995b37ac7ba561712712ad610323ea7ee8a1bf18c32'
            '3030252218445e3cb27025a1b567deef287ff3d5e2f32abc2d640a771d39ddd5'
            '1576aa21914edaa336d2b37d41ebf54fbaff6eb5099a3f46407cd79164ccdc67'
            'c53f8d84eea20eb57f4fa200d18ccfee7ddac57e087f3ef00efb8e22862c9dde')
sha256sums_x86_64=('a0474ef55f502cf5e4661f9053256394c636d62153d69f22d83a0f5a94774258')
sha256sums_aarch64=('4a32c2ca8d8e73c48c965d5cad3c87fd33dfee3c792ee97ace66d5cbb9b72dc3')
sha256sums_armv7h=('73f9c266a0c3c1865c62384cd82d7b7bc34ef76a3552283adf73cb5b89ca9f09')

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
