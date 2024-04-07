# Maintainer: Donald Webster <fryfrog@gmail.com>

pkgname="lidarr-nightly"
pkgver=2.2.4.4130
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

sha512sums=('fdf34b2105bbdd546bd4e9520e8c69ae594b87ea8c2581b8bdb69b54d7c54b6647187aa4dcb09d48d679a6f5b9ebf405ee90103dd8ef5535c4cf6d40e1178f66'
            '3800547b7c3d2b6e0a590ebd2db5ed48a6c31249098a4c36876faca36dd81e84ac45f3679d16bfe101be594dc6d5d0cfc91bfc66ed8cba35ec05f22027ab2441'
            'ffd466960527256d8de1d9887d90d4da87486eff062950c46cbc4fd4af1ef89e7d5c070ef1e649b23a95fbab15651e289fd5bdc6d34649e4a6ecdf2f6da06622'
            'c9bd8707cceed880ecebd974f0c5db0801cfafd714bf939e9c688212e4b092cc9e6452c899ae30d8d5c5acc9a4e41c9fa0f1e5c1daef7cdb0bd5d66c6be0eb4a')
sha512sums_x86_64=('6e6a0d5b0e4aa852a099d45fbcf5a232c0303749c3d5637c7716bae72a5faa53f495630e50b235f103f3fdbc4e62c9897dfb10a305c261525c80f5573c5e41a0')
sha512sums_aarch64=('691ba3dadd7c9a827c418b12011b6e77d4746b6a04e1dbb8a6193671b0de8a4c78a970e82763bfe2e974e15afdfa01b332180d66165a966bae175a265160bdc4')
sha512sums_armv7h=('1630ea6bd10191faec7826a4e660830ea62a80ccdc41b6aa683eba74a22cca62464bca1bd9d0a0484e5117ac34091860a60458165bc17d19c279d20d1c902524')



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
