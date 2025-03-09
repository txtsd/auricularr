# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Contributor: Daniel Egeberg <daniel.egeberg@gmail.com>
# Contributor: Justin Dray <justin@dray.be>
# Helpful URL: https://services.sonarr.tv/v1/releases

pkgname=sonarr-develop-bin
pkgver=4.0.13.2934
pkgrel=1
pkgdesc='Smart PVR for newsgroup and torrent users (develop branch)'
arch=(x86_64 aarch64 armv7h)
url='https://sonarr.tv'
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
  'prowlarr: torrent and usenet indexer proxy'
  'autobrr: irc, torrent and usenet indexer proxy'
)
provides=(sonarr)
conflicts=(sonarr)
options=(!debug)
install=sonarr.install
source=(
  package_info
  sonarr.service
  sonarr.sysusers
  sonarr.tmpfiles
  sonarr.install
)
source_x86_64=("Sonarr.develop.${pkgver}.linux-x64.tar.gz::https://services.sonarr.tv/v1/update/develop/download?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Sonarr.develop.${pkgver}.linux-arm64.tar.gz::https://services.sonarr.tv/v1/update/develop/download?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Sonarr.develop.${pkgver}.linux-arm.tar.gz::https://services.sonarr.tv/v1/update/develop/download?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha256sums=('1a609451c6de4fe0f03c8019e2232b04a552bea98e5863f7e49bba9f00ed05ba'
            '04701303b54b2e0c913e5e0808759b77ed41a4dac8f091e6552cf0ef96cc8415'
            '00141d4cbf34daa6d91b26179d4847ec970e2767382e18fdf9af2ec84a0ff43e'
            'd6b18a83dd9c213470d984f71ddcefcd64d12bb87f68225cc4ebf5fa4a831703'
            '3d912d367eeb89ead06dc9dc45de093f48ddc601188731d54775c33e04e369aa')
sha256sums_x86_64=('4aa40fa171696ee8003fe3b2996d92542897cc6b218c1b51bc40cc51a1864a29')
sha256sums_aarch64=('715855723cdef897a2e6b43f40a2252de9ae56ce1bad0a102767d826ef4f2df8')
sha256sums_armv7h=('13bdb148a643c05e3881738cb1a6864ae3cc8ccc88f7542ef1aa9304f79b598f')

package() {
  install -dm755 "${pkgdir}/usr/lib/sonarr/bin"

  # License
  install -Dm644 Sonarr/LICENSE.md "${pkgdir}/usr/share/licenses/${pkgname}"
  rm Sonarr/LICENSE.md

  # Remove Service Helpers, and Update files
  rm Sonarr/ServiceInstall*
  rm Sonarr/ServiceUninstall*
  rm -rf Sonarr/Sonarr.Update

  # Disable built in updater.
  install -Dm644 package_info "${pkgdir}/usr/lib/sonarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/sonarr/package_info"

  # Copy Sonarr
  cp -dr Sonarr/* "${pkgdir}/usr/lib/sonarr/bin"

  # Systemd
  install -Dm644 sonarr.service "${pkgdir}/usr/lib/systemd/system/sonarr.service"
  install -Dm644 sonarr.sysusers "${pkgdir}/usr/lib/sysusers.d/sonarr.conf"
  install -Dm644 sonarr.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/sonarr.conf"
}
