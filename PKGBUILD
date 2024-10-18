# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Contributor: Donald Webster <fryfrog@gmail.com>
# Helpful url: https://prowlarr.servarr.com/v1/update/develop?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=prowlarr-nightly-bin
pkgver=1.25.3.4804
pkgrel=1
pkgdesc="Indexer manager/proxy for usenet and torrent users (nightly builds)"
arch=('x86_64' 'aarch64' 'armv7h')
url="https://prowlarr.com"
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
)
provides=(prowlarr)
conflicts=(prowlarr)
options=('!debug')
source=(
  'prowlarr.service'
  'prowlarr.tmpfiles'
  'prowlarr.sysusers'
  'package_info'
)
source_x86_64=("Prowlarr.nightly.${pkgver}.linux-core-x64.tar.gz::https://prowlarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Prowlarr.nightly.${pkgver}.linux-core-arm64.tar.gz::https://prowlarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Prowlarr.nightly.${pkgver}.linux-core-arm.tar.gz::https://prowlarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha256sums=('21ca63506b3cffcca8dcd95e1bdf3fa8415f1bc134c31a153b51b573dc31d390'
            '4c3f9b5fa71810697efbe60f20a2cba24fd1b997d5372c3726457b197d61ccb5'
            '08d51099f09721b173233e58172c486025c16034dd89e73ccb42b647dcc34c4b'
            'ced75b25b228ef5524b07e6b81efb2e6b7afd9a15bb133a8d5a364e9968f0ec0')
sha256sums_x86_64=('a980028f520420bf99adfac5fefd6bec2e9830fa0a04501102259bcb88da5131')
sha256sums_aarch64=('c67c768c1ac3500f808f86475501dcb1f94953c9079c2c2e51fead55c4729cca')
sha256sums_armv7h=('f14f59fe76cf854aefbbcdd73aee641e87d5f23143a51ab654dba39cf1616bf6')

package() {
  install -dm755 "${pkgdir}/usr/lib/prowlarr/bin"

  # License
  install -Dm644 "${srcdir}/Prowlarr/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}"
  rm "${srcdir}/Prowlarr/LICENSE"

  # Disable built in updater.
  rm -rf "${srcdir}/Prowlarr/Prowlarr.Update"
  install -Dm644 "${srcdir}/package_info" "${pkgdir}/usr/lib/prowlarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/prowlarr/package_info"

  cp -dpr --no-preserve=ownership "${srcdir}/Prowlarr/"* "${pkgdir}/usr/lib/prowlarr/bin"

  install -Dm644 "${srcdir}/prowlarr.service" "${pkgdir}/usr/lib/systemd/system/prowlarr.service"
  install -Dm644 "${srcdir}/prowlarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/prowlarr.conf"
  install -Dm644 "${srcdir}/prowlarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/prowlarr.conf"
}
