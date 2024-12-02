# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Contributor: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://readarr.servarr.com/v1/update/nightly?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=readarr-nightly-bin
pkgver=0.4.5.2696
pkgrel=1
pkgdesc='Ebook and audiobook collection manager for newsgroup and torrent users (nightly builds)'
arch=(x86_64 aarch64 armv7h)
url='https://readarr.com'
license=('GPL-3.0-or-later')
groups=(servarr-nightly-bin)
depends=(
  gcc-libs
  glibc
  sqlite
  zlib
)
optdepends=(
  'postgresql: postgresql database'
  'sabnzbd: usenet downloader'
  'nzbget: usenet downloader'
  'qbittorrent: torrent downloader'
  'deluge: torrent downloader'
  'rtorrent: torrent downloader'
  'nodejs-flood: torrent downloader'
  'vuze: torrent downloader'
  'aria2: torrent downloader'
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
options=(!debug)
install=readarr.install
source=(
  package_info
  readarr.service
  readarr.sysusers
  readarr.tmpfiles
)
source_x86_64=("Readarr.nightly.${pkgver}.linux-core-x64.tar.gz::https://readarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Readarr.nightly.${pkgver}.linux-core-arm64.tar.gz::https://readarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Readarr.nightly.${pkgver}.linux-core-arm.tar.gz::https://readarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha256sums=('9b505e7e93a71c9d2fdc4689cf4a3cd691e3927b419cf5bb6e1aed43b5a91edc'
            '4696e52bc1cd8b7860eeb1ffaefc4153c3bf27defd7e7e91da03ad8aa28aa3df'
            '2a3a67ccd03d85396d7c08e8c5ef530b31c6d847102b35af705b43c0714a4a3a'
            'a4cfdf882ab62dea54d85dfae4a633cf21bce597a19c3287d90c024e3ff399ce')
sha256sums_x86_64=('f07f224995a4896ee2fc2158031494043a8469cf096c800c4dd0e66ab101bb1e')
sha256sums_aarch64=('bd0a808515064b5d24701571651780c06b7d75fc7ca3f7534f840c9fb1fa9a76')
sha256sums_armv7h=('4c402cfd691bfdd08a7bac998f0ccb5c4c184ced6c0713d5a4d3d507f954496c')

package() {
  install -dm755 "${pkgdir}/usr/lib/readarr/bin"

  # License
  install -Dm644 Readarr/LICENSE.md "${pkgdir}/usr/share/licenses/${pkgname}"
  rm Readarr/LICENSE.md

  # Remove Service Helpers, and Update files
  rm Readarr/ServiceInstall*
  rm Readarr/ServiceUninstall*
  rm -rf Readarr/Readarr.Update

  # Disable built in updater.
  install -Dm644 package_info "${pkgdir}/usr/lib/readarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/readarr/package_info"

  # Copy Readarr
  cp -dr Readarr/* "${pkgdir}/usr/lib/readarr/bin"

  # Systemd
  install -Dm644 readarr.service "${pkgdir}/usr/lib/systemd/system/readarr.service"
  install -Dm644 readarr.sysusers "${pkgdir}/usr/lib/sysusers.d/readarr.conf"
  install -Dm644 readarr.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/readarr.conf"
}
