
# Maintainer: Donald Webster <fryfrog@gmail.com>

pkgname="radarr"
pkgver=5.9.1.9070
pkgrel=1
pkgdesc="Movie download automation for usenet and torrents."
arch=('x86_64' 'aarch64' 'armv7h')
url="https://github.com/Radarr/Radarr"
license=('GPL3')
options=('!strip' 'staticlibs')
depends=('sqlite')
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

source_x86_64=("https://github.com/Radarr/Radarr/releases/download/v${pkgver}/Radarr.master.${pkgver}.linux-core-x64.tar.gz")
source_aarch64=("https://github.com/Radarr/Radarr/releases/download/v${pkgver}/Radarr.master.${pkgver}.linux-core-arm64.tar.gz")
source_armv7h=("https://github.com/Radarr/Radarr/releases/download/v${pkgver}/Radarr.master.${pkgver}.linux-core-arm.tar.gz")

source=('radarr.service'
        'radarr.tmpfiles'
        'radarr.sysusers'
        'package_info')

sha512sums=('64bfe917560e1de43b36a9c9bd2ac653f9fb231ed1971522b7b72ad308e72c9a29b5457aaa2faa84d1851148e5de9c707f43c81a42fcee40da98bf1d593c1928'
            '2ef38a061f4438349f7dafe209328fe9de5f44712f16435805e2622050be45a3fffd1838d9fccef2d6aa7603cf05b01280df6fed957b66d2d67ceaeeedcd5f6f'
            'c1ee3925eced182ea7fffa55a6dc2a4e099ccf18636fc237ef0a2fc9517a38cfc2a819ae5a7bc546b63e383506f9f47e89454a71e34106c579d7454d71b2299e'
            'c5a3387a72b0ee51d111b351527f47c59a2000696d165d93ffa86d1e4c732aee3e156c4e9132517efe1c9804223b3a655f089e234ce0f0783f7d5877d1d6af86')
sha512sums_x86_64=('8d6f8f18c8598008f68673fd4121c6d38bf0abb32785610185857e7c4b9aa4aa2bc36a18ad675a364a035d9ba80ac8804d5f9b6fcc24819d65457434c58bd25c')
sha512sums_aarch64=('2c0e981a1d5d03385b840c6a627589c7ef7fd07cf74db91cfbcba79025c0c3233ab757edc2aaecea0adefc4328081cdc61897d4d267c0c049138eaa7078dd58e')
sha512sums_armv7h=('fde6e4352a399e1f88030b8bdf328738205aaaf463bebf51fa65102334036949f143324401cea0498ff999fdb100eddfdc96795c5e9e6cd67735f43662f8c028')


package() {
  rm -rf "${srcdir}/Radarr/Radarr.Update"
  install -d -m 755 "${pkgdir}/usr/lib/radarr/bin"
  cp -dpr --no-preserve=ownership "${srcdir}/Radarr/"* "${pkgdir}/usr/lib/radarr/bin"
  chmod -R a=,a+rX,u+w "${pkgdir}/usr/lib/radarr/bin"
  chmod +x "${pkgdir}/usr/lib/radarr/bin/Radarr" "${pkgdir}/usr/lib/radarr/bin/ffprobe"

  # Disable built in updater.
  install -D -m 644 "${srcdir}/package_info" "${pkgdir}/usr/lib/radarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/radarr/package_info"

  install -D -m 644 "${srcdir}/radarr.service" "${pkgdir}/usr/lib/systemd/system/radarr.service"
  install -D -m 644 "${srcdir}/radarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/radarr.conf"
  install -D -m 644 "${srcdir}/radarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/radarr.conf"
}
