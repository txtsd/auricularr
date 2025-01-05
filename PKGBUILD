# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Contributor: Devin Buhl <devin.kray@gmail.com>
# Helpful URL: https://radarr.servarr.com/v1/update/master?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=radarr-bin
pkgver=5.17.2.9580
pkgrel=1
pkgdesc='Movie organizer/manager for usenet and torrent users'
arch=(x86_64 aarch64 armv7h)
url='https://radarr.video'
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
  'prowlarr: torrent and usenet indexer proxy'
  'autobrr: irc, torrent and usenet indexer proxy'
)
provides=(radarr)
conflicts=(radarr)
options=(!debug)
install=radarr.install
source=(
  package_info
  radarr.service
  radarr.sysusers
  radarr.tmpfiles
)
source_x86_64=("Radarr.master.${pkgver}.linux-core-x64.tar.gz::https://radarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Radarr.master.${pkgver}.linux-core-arm64.tar.gz::https://radarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Radarr.master.${pkgver}.linux-core-arm.tar.gz::https://radarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha256sums=('dd9a40cb2885bcc80d0057c50920707f003a64012df03ab6dad0bf67e651e591'
            '66ed1fae2d09b09fa53cd740b14bbc8d536042aaa931e5e5fbbd1cbe40557054'
            'bb73e0c55711d7ddbf74140b3beb39cb8674ae92be8387c3dd8109bcd53faca8'
            'c68efcb3778cb497d7c256dc97df7413ce09f07ea341e4d2683e7fee321cbcbb')
sha256sums_x86_64=('d3c8d085a3cf4b8cc442e276a2f3ff66c3571b5749be26b85fa46b5f2c0735aa')
sha256sums_aarch64=('ef42160aee3c8efa131e8f10ffbf27fd90e6845c26f4f64e617525ff5ee29ec8')
sha256sums_armv7h=('20de4998b83c5c1e5de7e94a51205996935b81125de1375c1be07226bd4e0770')

package() {
  install -dm755 "${pkgdir}/usr/lib/radarr/bin"

  # License
  install -Dm644 Radarr/LICENSE "${pkgdir}/usr/share/licenses/${pkgname}"
  rm Radarr/LICENSE

  # Remove Service Helpers, and Update files
  rm Radarr/ServiceInstall*
  rm Radarr/ServiceUninstall*
  rm -rf Radarr/Radarr.Update

  # Disable built in updater.
  install -Dm644 package_info "${pkgdir}/usr/lib/radarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/radarr/package_info"

  # Copy Radarr
  cp -dpr --no-preserve=ownership Radarr/* "${pkgdir}/usr/lib/radarr/bin"

  # Systemd
  install -Dm644 radarr.service "${pkgdir}/usr/lib/systemd/system/radarr.service"
  install -Dm644 radarr.sysusers "${pkgdir}/usr/lib/sysusers.d/radarr.conf"
  install -Dm644 radarr.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/radarr.conf"
}
