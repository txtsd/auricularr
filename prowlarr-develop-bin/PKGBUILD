# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://prowlarr.servarr.com/v1/update/develop?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=prowlarr-develop-bin
pkgver=1.26.1.4844
pkgrel=1
pkgdesc='Indexer manager/proxy for usenet and torrent users (develop branch)'
arch=(x86_64 aarch64 armv7h)
url='https://prowlarr.com'
license=('GPL-3.0-or-later')
groups=(servarr-develop-bin)
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
  'flaresolverr: A proxy server to bypass Cloudflare protection'
  'sonarr: automatically integrates with and syncs indexers'
  'radarr: automatically integrates with and syncs indexers'
  'lidarr: automatically integrates with and syncs indexers'
  'readarr: automatically integrates with and syncs indexers'
  'whisparr: automatically integrates with and syncs indexers'
  'mylar3: automatically integrates with and syncs indexers'
  'lazylibrarian: automatically integrates with and syncs indexers'
)
provides=(prowlarr)
conflicts=(prowlarr)
options=(!debug)
install=prowlarr.install
source=(
  package_info
  prowlarr.service
  prowlarr.sysusers
  prowlarr.tmpfiles
)
source_x86_64=("Prowlarr.develop.${pkgver}.linux-core-x64.tar.gz::https://prowlarr.servarr.com/v1/update/develop/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Prowlarr.develop.${pkgver}.linux-core-arm64.tar.gz::https://prowlarr.servarr.com/v1/update/develop/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Prowlarr.develop.${pkgver}.linux-core-arm.tar.gz::https://prowlarr.servarr.com/v1/update/develop/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha256sums=('c2c998c73ba78eac6ad7519d5d46bfa3354dd6d575a410aefb4b432a2f387f10'
            '8253f405c6dd1261ed321702867b08b413f8d69ef3d081949583d9106e3f812b'
            'ee61f5621eae6ab932fb093a4f75a0ab11bf9e3ca829f0d34c25014f68aeff7d'
            '75591d19518bafc60862c60848ecad84f92c7f2b47b2b4eeafcbbbd650a43043')
sha256sums_x86_64=('6dbdd4cd1af40dfe107a34ac1732fbc7d3e7c942cbdcc2fc08be841a6b2a7a53')
sha256sums_aarch64=('777ffa37172d59b301491ec23c5d8de3a8354c973adca9ca2aa65cbb6a7403cd')
sha256sums_armv7h=('5d3c971ea2671cfe2ba483559ae3d661318786b416bf92eaf4ad5f69937c18f8')

package() {
  install -dm755 "${pkgdir}/usr/lib/prowlarr/bin"

  # License
  install -Dm644 Prowlarr/LICENSE "${pkgdir}/usr/share/licenses/${pkgname}"
  rm Prowlarr/LICENSE

  # Remove Service Helpers, and Update files
  rm Prowlarr/ServiceInstall*
  rm Prowlarr/ServiceUninstall*
  rm -rf Prowlarr/Prowlarr.Update

  # Disable built in updater.
  install -Dm644 package_info "${pkgdir}/usr/lib/prowlarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/prowlarr/package_info"

  cp -dpr --no-preserve=ownership Prowlarr/* "${pkgdir}/usr/lib/prowlarr/bin"

  install -Dm644 prowlarr.service "${pkgdir}/usr/lib/systemd/system/prowlarr.service"
  install -Dm644 prowlarr.sysusers "${pkgdir}/usr/lib/sysusers.d/prowlarr.conf"
  install -Dm644 prowlarr.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/prowlarr.conf"
}
