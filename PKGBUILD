# Maintainer: Sven Frenzel <aur@frenzel.dk>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Helpful url: https://services.lidarr.audio/v1/update/master?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname='lidarr'
pkgver=2.4.3.4248
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
sha512sums_x86_64=('f07ed833f6423812cfa37a1c74babbfe6791c62a636889a031f53824ceb185827d6cfbbef40db1ff28e5c3a4631678beef020bc509aeb14a483346a8d2462918')
sha512sums_aarch64=('731934f84a07d2e3b48ca25724713d51c2ecf093fb1e11f731fdb7fca1f7a1bf56bbad43b946c9516980b4a8e746b82ab7f6b3983e0a02ccefa4861dabd6e885')
sha512sums_armv7h=('ef78c76577ec98ce8aa7aa583ffa57970226881cedf782de091550630bf225078784d89960868b6b2c2db40c3f3a3e6f5dfa8959eb375311777d6a781823c32d')



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
