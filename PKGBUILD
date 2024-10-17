# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Contributor: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://whisparr.servarr.com/v1/update/nightly?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=whisparr-nightly-bin
pkgver=2.0.0.548
pkgrel=1
pkgdesc="Adult movie organizer/manager for usenet and torrent users (nightly builds)"
arch=('x86_64' 'aarch64' 'armv7h')
url="https://whisparr.com"
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
  'libgdiplus: provides a gdi+ compatible api'
)
provides=(whisparr)
conflicts=(whisparr)
options=('!debug')
source=(
  'whisparr.service'
  'whisparr.tmpfiles'
  'whisparr.sysusers'
  'package_info'
)
source_x86_64=("Whisparr.nightly.${pkgver}.linux-core-x64.tar.gz::https://whisparr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Whisparr.nightly.${pkgver}.linux-core-arm64.tar.gz::https://whisparr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Whisparr.nightly.${pkgver}.linux-core-arm.tar.gz::https://whisparr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha512sums=('e7a0a341473fde7ee092b94ef7a86a7ce48c5187e4953efa2246e861047acff40c5381a4e7b57cabf38c7b3c907d85402b277b83f847d48d463d6eeebd807460'
            '86bf3eeed370680eae5ccad9be0caa1fa271461daca0d36c99b6b1a11a4e57a507e664e48ac27c454dea891db04a09766c76899169c335aa386ee90972cd6108'
            '0fb4efdcf88316a0677945714ff2d022a68459d2762c8c1fdf49a37abc2d67a807c05b7afd809e6fc360ff9b943b7c6623a6c95906db87bfbe476225f44a78ec'
            'd63e53264055e7281a0befaa9165f7651ecc2cf105787cc47b551ad4d412dfaa90c03a75d20108e87e0b4975655941a2823aaf22c50083e2250a2cb0a5db0491')
sha512sums_x86_64=('d1037b9a8604451e29d53fe014e2092f8463a86bad30fba9cfc521a4542bb165fe7151b5e25a24d51a1b3758bd6b4e59a82e3c29ad09ad1d83bec8c5a127d7e3')
sha512sums_aarch64=('005637258da0edd74b74268887fed5d8f18f25fd9a757997c74065cd65db9b823d9d68c6c386ad13e27af375f9975b1bed9f891dfcdb8d33cfd24b2ae281edfb')
sha512sums_armv7h=('5175f97d7e6d376423e2dab9335461e338c546a0ad81c3e937ad7fe2b9bbdadfe1ac80d3efe13cbfb2afc49b368ec86c11930b6af965de19fc885e56b51d8030')

package() {
  install -dm755 "${pkgdir}/usr/lib/whisparr/bin"

  # License
  install -Dm644 "${srcdir}/Whisparr/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}"
  rm "${srcdir}/Whisparr/LICENSE"

  # Disable built in updater.
  rm -rf "${srcdir}/Whisparr/Whisparr.Update"
  install -Dm644 "${srcdir}/package_info" "${pkgdir}/usr/lib/whisparr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/whisparr/package_info"

  cp -dpr --no-preserve=ownership "${srcdir}/Whisparr/"* "${pkgdir}/usr/lib/whisparr/bin"

  install -Dm644 "${srcdir}/whisparr.service" "${pkgdir}/usr/lib/systemd/system/whisparr.service"
  install -Dm644 "${srcdir}/whisparr.sysusers" "${pkgdir}/usr/lib/sysusers.d/whisparr.conf"
  install -Dm644 "${srcdir}/whisparr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/whisparr.conf"
}
