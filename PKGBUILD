# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Contributor: Donald Webster <fryfrog@gmail.com>
# Contributor: Sven Frenzel <aur@frenzel.dk>
# Helpful URL: https://services.lidarr.audio/v1/update/master?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=lidarr-bin
pkgver=2.6.4.4402
pkgrel=1
pkgdesc="Music collection manager for newsgroup and torrent users."
arch=('x86_64' 'aarch64' 'armv7h')
url="https://lidarr.audio"
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
provides=(lidarr)
conflicts=(lidarr)
options=('!debug')
source=(
  'lidarr.service'
  'lidarr.tmpfiles'
  'lidarr.sysusers'
  'package_info'
)
source_x86_64=("https://github.com/Lidarr/Lidarr/releases/download/v${pkgver}/Lidarr.master.${pkgver}.linux-core-x64.tar.gz")
source_aarch64=("https://github.com/Lidarr/Lidarr/releases/download/v${pkgver}/Lidarr.master.${pkgver}.linux-core-arm64.tar.gz")
source_armv7h=("https://github.com/Lidarr/Lidarr/releases/download/v${pkgver}/Lidarr.master.${pkgver}.linux-core-arm.tar.gz")
sha256sums=('dcbe3d2a3d64a78a4b2b84a3486991a8b90fdd6900d7345004827a168f8b5645'
            'abde8989e7ab9dc62b6a501644da5a9253953b6394b890565e00a69cbfd89068'
            '19b36aefd2ef93d4a630ceaefe582573ecdaa72ec21bfb48ce3941ead7b967fb'
            '19435dff2251782714875af95a38d8491cee6c178e8f32e3c3b6e566b3edc931')
sha256sums_x86_64=('2554f4ee3d782b7ed543d17e4669eb8b3e29f6d68fa30e676b0527aa68b44f1e')
sha256sums_aarch64=('15ce67026d21c90f1586243651528461b855960f4337c3440bf0cd03cc7cc4a9')
sha256sums_armv7h=('f90631d93c39dae9bafdac1dc0698299f51eb1974ba893b566f29a0fe251a816')

package() {
  install -dm755 "${pkgdir}/usr/lib/lidarr/bin"

  # License
  install -Dm644 "${srcdir}/Lidarr/LICENSE.md" "${pkgdir}/usr/share/licenses/${pkgname}"
  rm "${srcdir}/Lidarr/LICENSE.md"

  # Disable built in updater.
  rm -rf "${srcdir}/Lidarr/Lidarr.Update"
  install -Dm644 "${srcdir}/package_info" "${pkgdir}/usr/lib/lidarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/lidarr/package_info"

  cp -dpr --no-preserve=ownership "${srcdir}/Lidarr/"* "${pkgdir}/usr/lib/lidarr/bin"

  install -Dm644 "${srcdir}/lidarr.service" "${pkgdir}/usr/lib/systemd/system/lidarr.service"
  install -Dm644 "${srcdir}/lidarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/lidarr.conf"
  install -Dm644 "${srcdir}/lidarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/lidarr.conf"
}
