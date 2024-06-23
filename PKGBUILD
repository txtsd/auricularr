# Maintainer: Donald Webster <fryfrog@gmail.com>
# Helpful url: https://prowlarr.servarr.com/v1/update/master?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname="prowlarr"
pkgver=1.19.0.4568
pkgrel=1
pkgdesc="Usenet and torrent aggregator, similar to nzbhydra2 and jackett."
arch=('x86_64' 'aarch64' 'armv7h')
url="https://github.com/Prowlarr/Prowlarr"
license=('GPL3')
options=('!strip' 'staticlibs')
depends=('sqlite')
optdepends=(
  'sonarr: automatically adds and remove indexers/trackers'
  'radarr: automatically adds and remove indexers/trackers'
  'lidarr: automatically adds and remove indexers/trackers'
  'readarr: automatically adds and remove indexers/trackers'
)

source_x86_64=("prowlarr.master.${pkgver}.linux-core-x64.tar.gz::https://prowlarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64") 
source_aarch64=("prowlarr.master.${pkgver}.linux-core-arm64.tar.gz::https://prowlarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64") 
source_armv7h=("prowlarr.master.${pkgver}.linux-core-arm.tar.gz::https://prowlarr.servarr.com/v1/update/master/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")

source=(
  'prowlarr.service'
  'prowlarr.tmpfiles'
  'prowlarr.sysusers'
  'package_info'
)

sha512sums=('66a0fa8cdfb6119db300bb00bdead9c472776e8d5175fd313039116de4ed5b761ed13973229393106519e6ed4a45ec9709bd2c6099cd655448c5a1af2baf7b89'
            '9159ceda0955f2ebc495dd470c9d6234d8534a120ab81fa58fefae94a8ecfdc8fe883fb1287bc10429e7b4f35ac59d36232d716c161a242a4bfcdff768f1b9a2'
            '6ebd6f268e5aa7446e3c77540f5c95b3237959892e8800f5f380a0f979c71ec0d6f7664c1a58f7d10a255bc21a19bad0fef8609b02b4d5e15f340e66364017d2'
            '13562cac353f9fbf3545613d49186b5c7b35a3726b003133044212f65ec51eece88fb06017a90d0a67fa5cf694fed142c20704cc174b408016f9e3176296c5b6')
sha512sums_x86_64=('e0b7af538f2caae22835f8f2afcc31704d3a4d1c9845aca22feec1b47a27eed9ae4efd6a30771015e95f702ebc0c3a264dbc1c934a5fb5146ced7a44f0ec3cf8')
sha512sums_aarch64=('f908a2cb354740b4d0d4b57929537fad50a3168203afb64cb11c1b2b3096a686b81ff881c56428dcad8cc7d7301da8b5708c293ae6cc0de24314d914edc26267')
sha512sums_armv7h=('cc6c9fba111b8c72fd903e65ea21a089d5172fa8c91c70ec0ce6e9a09d234084a86dbc3fcf64c60aba63b3f8454f9d3c93e649a0a80585f4730eb8972459040b')


package() {
  rm -rf "${srcdir}/Prowlarr/prowlarr.Update"
  install -d -m 755 "${pkgdir}/usr/lib/prowlarr/bin"
  cp -dpr --no-preserve=ownership "${srcdir}/Prowlarr/"* "${pkgdir}/usr/lib/prowlarr/bin"
  chmod -R a=,a+rX,u+w "${pkgdir}/usr/lib/prowlarr/bin"
  chmod +x "${pkgdir}/usr/lib/prowlarr/bin/Prowlarr"

  # Disable built in updater.
  install -D -m 644 "${srcdir}/package_info" "${pkgdir}/usr/lib/prowlarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/prowlarr/package_info"

  install -D -m 644 "${srcdir}/prowlarr.service" "${pkgdir}/usr/lib/systemd/system/prowlarr.service"
  install -D -m 644 "${srcdir}/prowlarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/prowlarr.conf"
  install -D -m 644 "${srcdir}/prowlarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/prowlarr.conf"
}
