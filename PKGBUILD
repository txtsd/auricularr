# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://prowlarr.servarr.com/v1/update/master?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=prowlarr-bin
pkgver=1.29.2.4915
pkgrel=1
pkgdesc='Indexer manager/proxy for usenet and torrent users.'
arch=(x86_64 aarch64 armv7h)
url='https://prowlarr.com'
license=('GPL-3.0-or-later')
groups=(servarr-bin)
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
source_x86_64=("Prowlarr.master.${pkgver}.linux-core-x64.tar.gz::https://prowlarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Prowlarr.master.${pkgver}.linux-core-arm64.tar.gz::https://prowlarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Prowlarr.master.${pkgver}.linux-core-arm.tar.gz::https://prowlarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha256sums=('6a674e395e0ab4d8b213e02e7ec72871049ca0dc0d1805fafdad0be32267903a'
            '5aa5a7800453d13948430744ca9f32584bf64f09daadf534e6eb2f6c5c452b4c'
            'ee61f5621eae6ab932fb093a4f75a0ab11bf9e3ca829f0d34c25014f68aeff7d'
            '75591d19518bafc60862c60848ecad84f92c7f2b47b2b4eeafcbbbd650a43043')
sha256sums_x86_64=('3aa668fa414c4d2925c0f963da1afbb003970a1d07bea6de5d6cdf9927165ca5')
sha256sums_aarch64=('72a49a933c2891be329bcf39d8c62ca32015def4bd792c94e3fc611cfb6c0f8a')
sha256sums_armv7h=('fe6589285312d09f3f51fd690dd8efb79fff09eedd4d38052293a9d9559632b3')

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

  # Copy Prowlarr
  cp -dr Prowlarr/* "${pkgdir}/usr/lib/prowlarr/bin"

  # Systemd
  install -Dm644 prowlarr.service "${pkgdir}/usr/lib/systemd/system/prowlarr.service"
  install -Dm644 prowlarr.sysusers "${pkgdir}/usr/lib/sysusers.d/prowlarr.conf"
  install -Dm644 prowlarr.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/prowlarr.conf"
}
