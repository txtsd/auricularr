# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Contributor: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://services.lidarr.audio/v1/update/nightly?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=lidarr-nightly-bin
pkgver=2.8.1.4440
pkgrel=1
pkgdesc='Music collection manager for newsgroup and torrent users (nightly builds)'
arch=('x86_64' 'aarch64' 'armv7h')
url='https://lidarr.audio'
license=('GPL-3.0-or-later')
groups=('servarr-nightly-bin')
depends=(
  'chromaprint'
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
provides=(lidarr)
conflicts=(lidarr)
options=(!debug)
source=(
  'lidarr.service'
  'lidarr.tmpfiles'
  'lidarr.sysusers'
  'package_info'
)
source_x86_64=("Lidarr.nightly.${pkgver}.linux-core-x64.tar.gz::https://lidarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Lidarr.nightly.${pkgver}.linux-core-arm64.tar.gz::https://lidarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Lidarr.nightly.${pkgver}.linux-core-arm.tar.gz::https://lidarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha256sums=('dcbe3d2a3d64a78a4b2b84a3486991a8b90fdd6900d7345004827a168f8b5645'
            'abde8989e7ab9dc62b6a501644da5a9253953b6394b890565e00a69cbfd89068'
            '19b36aefd2ef93d4a630ceaefe582573ecdaa72ec21bfb48ce3941ead7b967fb'
            '90a1960fef0d3833cd3635cedd16af3ee9ae6c7b95babc3021f6031d4e44c200')
sha256sums_x86_64=('b06d70cbfe325427a724eb38108347fe50a877f387816fec278003a26abe5644')
sha256sums_aarch64=('e8b133c915504d9bc6ad0d441182c278e80a991c1cb029ff80f687f723b3a90c')
sha256sums_armv7h=('e516e93563d36e6c934d3473620196d92a59744dcd7e461c98aaf21867fe18ab')

package() {
  install -dm755 "${pkgdir}/usr/lib/lidarr/bin"

  # License
  install -Dm644 "${srcdir}/Lidarr/LICENSE.md" "${pkgdir}/usr/share/licenses/${pkgname}"
  rm "${srcdir}/Lidarr/LICENSE.md"

  # Remove fpcalc, Service Helpers, and Update files
  rm "${srcdir}/Lidarr/fpcalc"
  rm "${srcdir}/Lidarr/ServiceInstall"*
  rm "${srcdir}/Lidarr/ServiceUninstall"*
  rm -rf "${srcdir}/Lidarr/Lidarr.Update"

  # Disable built in updater.
  install -Dm644 "${srcdir}/package_info" "${pkgdir}/usr/lib/lidarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/lidarr/package_info"

  cp -dpr --no-preserve=ownership "${srcdir}/Lidarr/"* "${pkgdir}/usr/lib/lidarr/bin"

  install -Dm644 "${srcdir}/lidarr.service" "${pkgdir}/usr/lib/systemd/system/lidarr.service"
  install -Dm644 "${srcdir}/lidarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/lidarr.conf"
  install -Dm644 "${srcdir}/lidarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/lidarr.conf"
}
