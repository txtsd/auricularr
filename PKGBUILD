# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://prowlarr.servarr.com/v1/update/master?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=prowlarr-bin
pkgver=1.25.4.4818
pkgrel=4
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
  'sonarr: automatically integrates with and syncs indexers'
  'radarr: automatically integrates with and syncs indexers'
  'lidarr: automatically integrates with and syncs indexers'
  'readarr: automatically integrates with and syncs indexers'
  'whisparr: automatically integrates with and syncs indexers'
)
provides=(prowlarr)
conflicts=(prowlarr)
options=(!debug)
source=(
  package_info
  prowlarr.install
  prowlarr.service
  prowlarr.sysusers
  prowlarr.tmpfiles
)
source_x86_64=("Prowlarr.master.${pkgver}.linux-core-x64.tar.gz::https://prowlarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Prowlarr.master.${pkgver}.linux-core-arm64.tar.gz::https://prowlarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Prowlarr.master.${pkgver}.linux-core-arm.tar.gz::https://prowlarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha256sums=('6a674e395e0ab4d8b213e02e7ec72871049ca0dc0d1805fafdad0be32267903a'
            '328fda2e2ed5f93440f3837a7d4f2c91e35fb4f6c51aeb33eacb80ecceb941a9'
            '8253f405c6dd1261ed321702867b08b413f8d69ef3d081949583d9106e3f812b'
            'ee61f5621eae6ab932fb093a4f75a0ab11bf9e3ca829f0d34c25014f68aeff7d'
            '4c3f9b5fa71810697efbe60f20a2cba24fd1b997d5372c3726457b197d61ccb5')
sha256sums_x86_64=('120a7e53f23d8b53d073f32bb8645a3a926d0d0510032466bb89c2ef57bc7d63')
sha256sums_aarch64=('c74850fc464633c5e322aa4f18ccf14ec2fba1483860cbdcff135fbe653fb091')
sha256sums_armv7h=('4c5efa52ec67b4d18637a4a4418cf3c2f2f079decd1cb22accc5631d89857f68')

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
