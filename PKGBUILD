# Maintainer: Donald Webster <fryfrog@gmail.com>
# Contributor: txtsd <aur.archlinux@ihavea.quest>
# Contributor: Justin Dray <justin@dray.be>
# Contributor: Daniel Egeberg <daniel.egeberg@gmail.com>
# Helpful URL: https://services.sonarr.tv/v1/releases

pkgname=sonarr-bin
pkgver=4.0.9.2244
pkgrel=1
pkgdesc='Smart PVR for newsgroup and bittorrent users.'
arch=('x86_64' 'aarch64' 'armv7h')
url='https://sonarr.tv'
license=('GPL-3.0-or-later')
groups=('servarr')
depends=(
  'gcc-libs'
  'glibc'
  'zlib'
  'sqlite'
)
optdepends=(
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
source_x86_64=("https://github.com/Sonarr/Sonarr/releases/download/v${pkgver}/Sonarr.main.${pkgver}.linux-x64.tar.gz")
source_aarch64=("https://github.com/Sonarr/Sonarr/releases/download/v${pkgver}/Sonarr.main.${pkgver}.linux-arm64.tar.gz")
source_armv7h=("https://github.com/Sonarr/Sonarr/releases/download/v${pkgver}/Sonarr.main.${pkgver}.linux-arm.tar.gz")
sha256sums=('19112dc0051224b4de66f28077c93b6ee06e163b5194e6aecf62dedf66ff45a9'
            'b26aa01e07e5864b588ebe51a2993eaafb03fa0f7ec3806f2996dd2daf46aee7'
            '047585a1d448ad2c6e2962fb60d4f71e01a2529e464b25d340bb0d31b8e0f08f'
            '7bf87304383b7d58ecab59b3686d00a8f1b6fbe4af3a86da35a887e4cebee411')
sha256sums_x86_64=('0bad4b09e61c90ee3a13f5a6fbae03c964b82c19789a690516663b14b75f15b0')
sha256sums_aarch64=('04a6b5b24fde70441727ee131ce1db91bbab80be7f70167bfea6444d12f1a265')
sha256sums_armv7h=('71b6d9d99e2937ebf5d35f6b05731c7bca7ee270014d09d0547a416cf67085b8')

package() {
  install -dm755 "${pkgdir}/usr/lib/sonarr/bin"

  # License
  install -Dm644 "${srcdir}/Sonarr/LICENSE.md" "${pkgdir}/usr/share/licenses/${pkgname}"
  rm "${srcdir}/Sonarr/LICENSE.md"

  cp -dpr --no-preserve=ownership "${srcdir}/Sonarr/"* "${pkgdir}/usr/lib/sonarr/bin"

  # Disable built in updater.
  rm -rf "${srcdir}/Sonarr/Sonarr.Update"
  install -Dm644 "${srcdir}/package_info" "${pkgdir}/usr/lib/sonarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/sonarr/package_info"

  install -Dm644 "${srcdir}/sonarr.service" "${pkgdir}/usr/lib/systemd/system/sonarr.service"
  install -Dm644 "${srcdir}/sonarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/sonarr.conf"
  install -Dm644 "${srcdir}/sonarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/sonarr.conf"

}
