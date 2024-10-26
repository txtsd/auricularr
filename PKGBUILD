# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Contributor: Daniel Egeberg <daniel.egeberg@gmail.com>
# Contributor: Justin Dray <justin@dray.be>
# Helpful URL: https://services.sonarr.tv/v1/releases

pkgname=sonarr-bin
pkgver=4.0.10.2544
pkgrel=1
pkgdesc='Smart PVR for newsgroup and torrent users.'
arch=('x86_64' 'aarch64' 'armv7h')
url='https://sonarr.tv'
license=('GPL-3.0-or-later')
groups=('servarr-bin')
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
provides=(sonarr)
conflicts=(sonarr)
options=(!debug)
source=(
  'package_info'
  'sonarr.service'
  'sonarr.sysusers'
  'sonarr.tmpfiles'
)
source_x86_64=("Sonarr.main.${pkgver}.linux-x64.tar.gz::https://services.sonarr.tv/v1/update/main/download?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Sonarr.main.${pkgver}.linux-arm64.tar.gz::https://services.sonarr.tv/v1/update/main/download?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Sonarr.main.${pkgver}.linux-arm.tar.gz::https://services.sonarr.tv/v1/update/main/download?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha256sums=('ea2073b568f98dc9d7a91ce11f279d2d8b5a5bb8bad01136a05cb7907e00bd47'
            'b26aa01e07e5864b588ebe51a2993eaafb03fa0f7ec3806f2996dd2daf46aee7'
            '047585a1d448ad2c6e2962fb60d4f71e01a2529e464b25d340bb0d31b8e0f08f'
            '7bf87304383b7d58ecab59b3686d00a8f1b6fbe4af3a86da35a887e4cebee411')
sha256sums_x86_64=('8d1ac597b7f4bc54f9cd8e8abfcf99773e05644ea9bdcd96303b4aef6da629c8')
sha256sums_aarch64=('72b8f940b1a15a0cac1806bcbf248c8f08a0f4c8bfa3504771d543cc2d8e2571')
sha256sums_armv7h=('cad540f3f5f7ef044508d638fec193b21b374d18b8cdce9b572fad46fee2ea1c')

package() {
  install -dm755 "${pkgdir}/usr/lib/sonarr/bin"

  # License
  install -Dm644 "${srcdir}/Sonarr/LICENSE.md" "${pkgdir}/usr/share/licenses/${pkgname}"
  rm "${srcdir}/Sonarr/LICENSE.md"

  # Disable built in updater.
  rm -rf "${srcdir}/Sonarr/Sonarr.Update"
  install -Dm644 "${srcdir}/package_info" "${pkgdir}/usr/lib/sonarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/sonarr/package_info"

  cp -dpr --no-preserve=ownership "${srcdir}/Sonarr/"* "${pkgdir}/usr/lib/sonarr/bin"

  install -Dm644 "${srcdir}/sonarr.service" "${pkgdir}/usr/lib/systemd/system/sonarr.service"
  install -Dm644 "${srcdir}/sonarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/sonarr.conf"
  install -Dm644 "${srcdir}/sonarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/sonarr.conf"

}
