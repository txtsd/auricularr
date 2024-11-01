# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Contributor: Daniel Egeberg <daniel.egeberg@gmail.com>
# Contributor: Justin Dray <justin@dray.be>
# Helpful URL: https://services.sonarr.tv/v1/releases

pkgname=sonarr-develop-bin
pkgver=4.0.10.2579
pkgrel=6
pkgdesc='Smart PVR for newsgroup and torrent users (develop branch)'
arch=('x86_64' 'aarch64' 'armv7h')
url='https://sonarr.tv'
license=('GPL-3.0-or-later')
groups=('servarr-develop-bin')
depends=(
  'gcc-libs'
  'glibc'
  'zlib'
  'sqlite'
  'ffmpeg'
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
provides=(sonarr)
conflicts=(sonarr)
options=(!debug)
source=(
  'package_info'
  'sonarr.service'
  'sonarr.sysusers'
  'sonarr.tmpfiles'
)
source_x86_64=("Sonarr.develop.${pkgver}.linux-x64.tar.gz::https://services.sonarr.tv/v1/update/develop/download?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Sonarr.develop.${pkgver}.linux-arm64.tar.gz::https://services.sonarr.tv/v1/update/develop/download?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Sonarr.develop.${pkgver}.linux-arm.tar.gz::https://services.sonarr.tv/v1/update/develop/download?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha256sums=('1a609451c6de4fe0f03c8019e2232b04a552bea98e5863f7e49bba9f00ed05ba'
            '3fe3cef29b846bd294eb1f7b24451c18a5cc5ceed6142ca4994de3788c4db665'
            '00141d4cbf34daa6d91b26179d4847ec970e2767382e18fdf9af2ec84a0ff43e'
            '0acb3697a5001b00f79269581cd08645f9d5e1e9f0a57cc3e7deeb12d66accc9')
sha256sums_x86_64=('224b3017693884ee9a6e8a810c2c0a1e0e750b3aed62de028f1b15a9799df303')
sha256sums_aarch64=('49fc91abac753f36b2fd451336165d4780ab0d9ab6ad4995784cedeef7e576e9')
sha256sums_armv7h=('be06539a15aab50101dd9654fca414abef33bc291af537a420723880a72f5017')

package() {
  install -dm755 "${pkgdir}/usr/lib/sonarr/bin"

  # License
  install -Dm644 "${srcdir}/Sonarr/LICENSE.md" "${pkgdir}/usr/share/licenses/${pkgname}"
  rm "${srcdir}/Sonarr/LICENSE.md"

  # Remove ffprobe, Service Helpers, and Update files
  rm "${srcdir}/Sonarr/ffprobe"
  rm "${srcdir}/Sonarr/ServiceInstall"*
  rm "${srcdir}/Sonarr/ServiceUninstall"*
  rm -rf "${srcdir}/Sonarr/Sonarr.Update"

  # Use system ffprobe
  ln -s /usr/bin/ffprobe "${pkgdir}/usr/lib/sonarr/bin/ffprobe"

  # Disable built in updater.
  install -Dm644 "${srcdir}/package_info" "${pkgdir}/usr/lib/sonarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/sonarr/package_info"

  cp -dpr --no-preserve=ownership "${srcdir}/Sonarr/"* "${pkgdir}/usr/lib/sonarr/bin"

  install -Dm644 "${srcdir}/sonarr.service" "${pkgdir}/usr/lib/systemd/system/sonarr.service"
  install -Dm644 "${srcdir}/sonarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/sonarr.conf"
  install -Dm644 "${srcdir}/sonarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/sonarr.conf"
}
