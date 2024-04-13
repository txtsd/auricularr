# Maintainer: Sven Frenzel <aur@frenzel.dk>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Helpful url: https://services.lidarr.audio/v1/update/master?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname='lidarr'
pkgver=2.2.5.4141
pkgrel=1
pkgdesc="Music download automation for usenet and torrents."
arch=('x86_64' 'aarch64' 'armv7h')
url="https://github.com/lidarr/Lidarr"
license=("GPL3")
depends=('sqlite' 'chromaprint')
options=('!strip' 'staticlibs')
optdepends=(
  'sabnzbd: usenet downloader'
  'nzbget: usenet downloader'
  'transmission-cli: torrent downloader (CLI and daemon)'
  'transmission-gtk: torrent downloader (GTK+)'
  'transmission-qt: torrent downloader (Qt)'
  'deluge: torrent downloader'
  'rtorrent: torrent downloader'
  'qbittorrent: torrent downloader'
  'qbittorrent-nox: torrent downloader (no X)'
  'jackett: torrent indexer proxy'
  'libgdiplus: provides a gdi+ compatible api'
)

source_x86_64=("lidarr-${pkgver}-linux-core-x64.tar.gz::https://services.lidarr.audio/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("lidarr-${pkgver}-linux-core-arm64.tar.gz::https://services.lidarr.audio/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("lidarr-${pkgver}-linux-core-arm.tar.gz::https://services.lidarr.audio/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")

source=(
  'lidarr.service'
  'lidarr.tmpfiles'
  'lidarr.sysusers'
  'package_info'
)

sha512sums=('fdf34b2105bbdd546bd4e9520e8c69ae594b87ea8c2581b8bdb69b54d7c54b6647187aa4dcb09d48d679a6f5b9ebf405ee90103dd8ef5535c4cf6d40e1178f66'
            'e40ce79a3e1741e7e06312797e652a85d199bd6d719ef953ea8c3c030756ee44e202956ac9e13cff17fac38312c27398f457f79923a7d0f56bd563a69af6ab63'
            'ffd466960527256d8de1d9887d90d4da87486eff062950c46cbc4fd4af1ef89e7d5c070ef1e649b23a95fbab15651e289fd5bdc6d34649e4a6ecdf2f6da06622'
            'cabbd3d6387f4198097573da23032d80c1fec11835c180e55a28ee54c79c821d3c49c039762cc3ddc5126efc37d50d93cdd89a75ff809a3d533533a30353933e')
sha512sums_x86_64=('7a86888bb17fd48beb85e32406dda360aa28ebd2ba976cda38537ab77b0ce88d1e29c5548d97a37119c3f629313d2fc1aa44f7514868b3998427ccd6b262d282')
sha512sums_aarch64=('8569d7ec5d628b44eb2abb044a92916fa94848964be4dbd26f740b463bd0b4fa6b34a16ed19fe74e7df7a0754e3ba1db7d05aeb6578ff5179a016c0f29cfb3ea')
sha512sums_armv7h=('a268edffcbf5711ebaf69eba14291f31bd87170a7a096c978115a4e85b02276f447efeb0c330b5b34db1a40cde2f7ac9db6b8b5b30e4c0fb2ebf92d6282d4a3e')



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
