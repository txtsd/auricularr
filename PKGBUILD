# Maintainer: Donald Webster <fryfrog@gmail.com>
# Contributor: txtsd <aur.archlinux@ihavea.quest>

pkgname=radarr-bin
pkgver=5.12.2.9335
pkgrel=1
pkgdesc="Movie organizer/manager for usenet and torrent users."
arch=('x86_64' 'aarch64' 'armv7h')
url="https://radarr.video"
license=('GPL-3.0-or-later')
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
provides=(radarr)
conflicts=(radarr)
options=('!debug')
source=('radarr.service'
        'radarr.tmpfiles'
        'radarr.sysusers'
        'package_info')
source_x86_64=("https://github.com/Radarr/Radarr/releases/download/v${pkgver}/Radarr.master.${pkgver}.linux-core-x64.tar.gz")
source_aarch64=("https://github.com/Radarr/Radarr/releases/download/v${pkgver}/Radarr.master.${pkgver}.linux-core-arm64.tar.gz")
source_armv7h=("https://github.com/Radarr/Radarr/releases/download/v${pkgver}/Radarr.master.${pkgver}.linux-core-arm.tar.gz")
sha512sums=('4bacd9b3b5f5fcd9de6787f2ec0ee44fa2cc120caf68fb63f0cae52d4ae004efca03b7fef0242ba7d709d60f58eb0d6df7782fffc66f802054fadb88a88d5277'
            '2ef38a061f4438349f7dafe209328fe9de5f44712f16435805e2622050be45a3fffd1838d9fccef2d6aa7603cf05b01280df6fed957b66d2d67ceaeeedcd5f6f'
            '7473b5c307de4f65ca458c5b827d9a40afd10f098f4ee327b51f6a756a086ef9db4ebae5f78dfd7119b28390edd9068a6149d0d89d4d18164218eed83601a72e'
            'c5a3387a72b0ee51d111b351527f47c59a2000696d165d93ffa86d1e4c732aee3e156c4e9132517efe1c9804223b3a655f089e234ce0f0783f7d5877d1d6af86')
sha512sums_x86_64=('245d75251ddf9b44f6ac6fde862cc89734edbb2fb7afc3391f5bad12fcde6d4b7c50b7852b43a77cf823864083e0e39e930c7323c2fcb2c9b15551ef71362a17')
sha512sums_aarch64=('dcb1d0164de4327e6d801bc53d7461525e65ede62f3258c43d1d323b7d3fbca1cd5f75a8ccdd808287c1d054ab69d29314030e453df1c36d88b63c5389a6cc7a')
sha512sums_armv7h=('a8e7ad043d241466b8476793be74ef5a4fa7aa4aa8e9d794b777922fff145d4fac0c37e6834d029a2cc40b98cff8fe209f7324b8e9fd0c8e5404737bf72a7627')

package() {
  install -dm755 "${pkgdir}/usr/lib/radarr/bin"

  # License
  install -Dm644 "${srcdir}/Radarr/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}"
  rm "${srcdir}/Radarr/LICENSE"

  # Disable built in updater.
  rm -rf "${srcdir}/Radarr/Radarr.Update"
  install -Dm644 "${srcdir}/package_info" "${pkgdir}/usr/lib/radarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/radarr/package_info"

  cp -dpr --no-preserve=ownership "${srcdir}/Radarr/"* "${pkgdir}/usr/lib/radarr/bin"

  install -Dm644 "${srcdir}/radarr.service" "${pkgdir}/usr/lib/systemd/system/radarr.service"
  install -Dm644 "${srcdir}/radarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/radarr.conf"
  install -Dm644 "${srcdir}/radarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/radarr.conf"
}
