# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Contributor: Devin Buhl <devin.kray@gmail.com>
# Helpful URL: https://radarr.servarr.com/v1/update/master?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=radarr-bin
pkgver=5.15.1.9463
pkgrel=1
pkgdesc='Movie organizer/manager for usenet and torrent users'
arch=(x86_64 aarch64 armv7h)
url='https://radarr.video'
license=('GPL-3.0-or-later')
groups=(servarr-bin)
depends=(
  ffmpeg
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
  radarr.install
  radarr.service
  radarr.sysusers
  radarr.tmpfiles
)
source_x86_64=("Radarr.master.${pkgver}.linux-core-x64.tar.gz::https://radarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Radarr.master.${pkgver}.linux-core-arm64.tar.gz::https://radarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Radarr.master.${pkgver}.linux-core-arm.tar.gz::https://radarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha256sums=('dd9a40cb2885bcc80d0057c50920707f003a64012df03ab6dad0bf67e651e591'
            '243ded7d0e9d59b9adf912bb4e35ba63247d85577b417b54dcd74f16f0cfbd26'
            '8ca13537e98380b91f1a950187d6b9f021f8a4d68871f709444742a4911bc5a6'
            'bb73e0c55711d7ddbf74140b3beb39cb8674ae92be8387c3dd8109bcd53faca8'
            'c68efcb3778cb497d7c256dc97df7413ce09f07ea341e4d2683e7fee321cbcbb')
sha256sums_x86_64=('6b520bbf75969fc81f4e958dd7fd3614f632bc1020b2cdbc2fcc5904bd453d8b')
sha256sums_aarch64=('3267e3abf6459bdf02d736e9e9025fd40c1ec00ac85bff04bff6fbedb542d452')
sha256sums_armv7h=('423fad6a0682ed590bee792820372e6517574bdd2be83a2b23cb6cd87fce0544')

package() {
  install -dm755 "${pkgdir}/usr/lib/radarr/bin"

  # License
  install -Dm644 Radarr/LICENSE "${pkgdir}/usr/share/licenses/${pkgname}"
  rm Radarr/LICENSE

  # Remove ffprobe, Service Helpers, and Update files
  rm Radarr/ffprobe
  rm Radarr/ServiceInstall*
  rm Radarr/ServiceUninstall*
  rm -rf Radarr/Radarr.Update

  # Use system ffprobe
  ln -s /usr/bin/ffprobe "${pkgdir}/usr/lib/radarr/bin/ffprobe"

  # Disable built in updater.
  install -Dm644 package_info "${pkgdir}/usr/lib/radarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/radarr/package_info"

  cp -dpr --no-preserve=ownership Radarr/* "${pkgdir}/usr/lib/radarr/bin"

  install -Dm644 radarr.service "${pkgdir}/usr/lib/systemd/system/radarr.service"
  install -Dm644 radarr.sysusers "${pkgdir}/usr/lib/sysusers.d/radarr.conf"
  install -Dm644 radarr.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/radarr.conf"
}
