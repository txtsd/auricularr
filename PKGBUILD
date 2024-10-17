# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Contributor: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://prowlarr.servarr.com/v1/update/develop?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=prowlarr-develop-bin
pkgver=1.24.3.4754
pkgrel=1
pkgdesc="Indexer manager/proxy for usenet and torrent users (develop branch)"
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
source_x86_64=("Prowlarr.develop.${pkgver}.linux-core-x64.tar.gz::https://prowlarr.servarr.com/v1/update/develop/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Prowlarr.develop.${pkgver}.linux-core-arm64.tar.gz::https://prowlarr.servarr.com/v1/update/develop/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Prowlarr.develop.${pkgver}.linux-core-arm.tar.gz::https://prowlarr.servarr.com/v1/update/develop/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha512sums=('2808c6ce071477bb8d43fae334c6b934cac2d1f9661dad21a0690c5d7f8df55b3e869c3fdaccf7c00f12729dbaaf9e021a3085959c77a907fa755801844db114'
            '9159ceda0955f2ebc495dd470c9d6234d8534a120ab81fa58fefae94a8ecfdc8fe883fb1287bc10429e7b4f35ac59d36232d716c161a242a4bfcdff768f1b9a2'
            '45e72f83acb0c82de199cc42a9e4b9c19bd6c00e5962c9e616d8ac7f3bdd9e949fdf9ddb23ececb5728bb7dc27251c9c9ea0051f19a18d0e0a897b3741f2df66'
            'b9a430c0f719ff6492a4add12097057bac1217be8aa17e90edb6b005a3083ae7d3091a23551464b5992f784a1d8985606ba90b22b65a8457699a46d0bd03cba6')
sha512sums_x86_64=('fec46db51a17f4e6dd9626818cd5f8af58b75eddc95f0bd4d5662dca2f290d330629162e0ad2dc58edf0221dd67ca62b8cee4fbd245635d4d389eb997b306a94')
sha512sums_aarch64=('b9168460bbf13717f753ff834fcf4e595d70592b0859c26d05bb7bd91b0b297b2c9ce3e7f599d8c62f76d7d6a6da1b5084eb16e1e90ca3d1a45f5c4d58a088c6')
sha512sums_armv7h=('b8bb1b9503f20a015445cc000c0a1b3edf10aa75a74030613e647807ce190089c1ea70a15b14507c600b0af472e236d0861bc4b55cb30bcc0a95cdd762665353')

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
