# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Contributor: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://services.lidarr.audio/v1/update/nightly?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=lidarr-develop-bin
pkgver=2.6.4.4402
pkgrel=1
pkgdesc="Music collection manager for newsgroup and torrent users (develop branch)"
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
source_x86_64=("Lidarr.develop.${pkgver}.linux-core-x64.tar.gz::https://lidarr.servarr.com/v1/update/develop/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Lidarr.develop.${pkgver}.linux-core-arm64.tar.gz::https://lidarr.servarr.com/v1/update/develop/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Lidarr.develop.${pkgver}.linux-core-arm.tar.gz::https://lidarr.servarr.com/v1/update/develop/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha512sums=('bd7de51c329a9f036b3f9e6575f5388b7414f77b7cb73e895dd87d7b09ae4160088f0ca62c8b873b89b4ebd26933bbde71c17a7ff036689bceebea789639d953'
            '3800547b7c3d2b6e0a590ebd2db5ed48a6c31249098a4c36876faca36dd81e84ac45f3679d16bfe101be594dc6d5d0cfc91bfc66ed8cba35ec05f22027ab2441'
            '64346eeeac7fb87f883bd41bdae08b30d1a45c3f12e9ad3a45d0e527a51924c8227bbefb4b2df889a86b7a083a22fcff1a2b048cd34bd7d720dc544d41a70f08'
            'c9bd8707cceed880ecebd974f0c5db0801cfafd714bf939e9c688212e4b092cc9e6452c899ae30d8d5c5acc9a4e41c9fa0f1e5c1daef7cdb0bd5d66c6be0eb4a')
sha512sums_x86_64=('585b6ab96b06c4950fe03d9b3bb89c25095d22665a699e3d9b6dd202f89d507589b48f2d9ffa5be240b470bc5cbaae4568e5b58917aea3d95489fee5619add3d')
sha512sums_aarch64=('c3ac715189a81ad76bf7f6ca9d6902ba04474ba73d4cf87556a5cbccdc78ffc3e236131136470de0fae6d38ff72502fe1b9e414482088d1fa605f38c229270eb')
sha512sums_armv7h=('2038b0e1c9c2f6711fc7078a3564360fa357dedc8454aa5cd354fd3b146770b50adc3e6b09e0be42753ee61da14a6cd2a8b6981a560926b6cc3ec9db79d39c99')

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
