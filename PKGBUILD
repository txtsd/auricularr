# Maintainer: Donald Webster <fryfrog@gmail.com>

pkgname="lidarr-nightly"
pkgver=1.2.2.3222
pkgrel=1
pkgdesc="Music download automation for usenet and torrents."
arch=('x86_64' 'aarch64' 'armv7h')
url="https://github.com/lidarr/Lidarr"
license=("GPL3")
depends=('sqlite' 'chromaprint')
options=('!strip' 'staticlibs')
optdepends=('sabnzbd: usenet downloader'
            'nzbget: usenet downloader'
            'transmission-cli: torrent downloader (CLI and daemon)'
            'transmission-gtk: torrent downloader (GTK+)'
            'transmission-qt: torrent downloader (Qt)'
            'deluge: torrent downloader'
            'rtorrent: torrent downloader'
            'qbittorrent: torrent downloader'
            'qbittorrent-nox: torrent downloader (no X)'
            'jackett: torrent indexer proxy'
            'libgdiplus: provides a gdi+ compatible api')

provides=('lidarr')
conflicts=('lidarr' 'lidarr-develop')

source_x86_64=("lidarr-${pkgver}-linux-core-x64.tar.gz::https://services.lidarr.audio/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("lidarr-${pkgver}-linux-core-arm64.tar.gz::https://services.lidarr.audio/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("lidarr-${pkgver}-linux-core-arm.tar.gz::https://services.lidarr.audio/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")

source=('lidarr.service'
        'lidarr.tmpfiles'
        'lidarr.sysusers'
        'package_info')

sha512sums=('156c1437b4d71858e01c60b5af65c0a52aab4554e3b26c3b5685291e230d93596a756ae4bcc5828238ad265bf066d6ebeea91f0e2c3e1732cdbb5ebdf9517238'
            '3800547b7c3d2b6e0a590ebd2db5ed48a6c31249098a4c36876faca36dd81e84ac45f3679d16bfe101be594dc6d5d0cfc91bfc66ed8cba35ec05f22027ab2441'
            'ffd466960527256d8de1d9887d90d4da87486eff062950c46cbc4fd4af1ef89e7d5c070ef1e649b23a95fbab15651e289fd5bdc6d34649e4a6ecdf2f6da06622'
            'c9bd8707cceed880ecebd974f0c5db0801cfafd714bf939e9c688212e4b092cc9e6452c899ae30d8d5c5acc9a4e41c9fa0f1e5c1daef7cdb0bd5d66c6be0eb4a')
sha512sums_x86_64=('74380023b14e7432af7db6e0c4833cc3ee15482f107952b3c47491bce7807ebd4fd2b434a1fde5683acf4bd07af7085cdf7de097080c6cb6d6fb228ca75f2cae')
sha512sums_aarch64=('f4aed9a1d28a0a6266073ef06271e0d63296fb14a952329e0a1edea5839a4c0b3655b02c8b7d56a0d3e618c19df6675c5bf794358a5f06d45b915202485d8036')
sha512sums_armv7h=('b628dd57fa0d0634638093b8e8a06fb8aab87c91c792d194cb959e470eb9bce56261086a345ca278e7c7f500c39ee407042770d1305efc91289657497a7568cc')



package() {
  # Update environment isn't needed.
  rm -rf "${srcdir}/Lidarr/Lidarr.Update"

  # The fpcalc binary comes from chromaprint.
  rm -rf "${srcdir}/Lidarr/fpcalc"

  install -d -m 755 "${pkgdir}/usr/lib/lidarr/bin"
  cp -dpr --no-preserve=ownership "${srcdir}/Lidarr/"* "${pkgdir}/usr/lib/lidarr/bin"
  chmod -R a=,a+rX,u+w "${pkgdir}/usr/lib/lidarr/bin"
  chmod +x "${pkgdir}/usr/lib/lidarr/bin/Lidarr"

  # Disable built in updater.
  install -D -m 644 "${srcdir}/package_info" "${pkgdir}/usr/lib/lidarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/lidarr/package_info"

  install -D -m 644 "${srcdir}/lidarr.service" "${pkgdir}/usr/lib/systemd/system/lidarr.service"
  install -D -m 644 "${srcdir}/lidarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/lidarr.conf"
  install -D -m 644 "${srcdir}/lidarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/lidarr.conf"
}
