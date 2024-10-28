# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Contributor: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://readarr.servarr.com/v1/update/nightly?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=readarr-nightly-bin
pkgver=0.4.3.2662
pkgrel=1
pkgdesc='Ebook and audiobook collection manager for newsgroup and torrent users (nightly builds)'
arch=('x86_64' 'aarch64' 'armv7h')
url='https://readarr.com'
license=('GPL-3.0-or-later')
groups=('servarr-nightly-bin')
depends=(
  'gcc-libs'
  'glibc'
  'zlib'
  'sqlite'
)
optdepends=(
  'postgresql: postgresql database'
  'sabnzbd: usenet downloader'
  'nzbget: usenet downloader'
  'qbittorrent: torrent downloader'
  'deluge: torrent downloader'
  'rtorrent: torrent downloader'
  'nodejs-flood: torrent downloader'
  'vuze: torrent downloader'
  'aria2: torrent downloader'
  'transmission-cli: torrent downloader (CLI and daemon)'
  'transmission-gtk: torrent downloader (GTK+)'
  'transmission-qt: torrent downloader (Qt)'
  'jackett: torrent indexer proxy'
  'nzbhydra2: torznab and usenet indexer proxy'
  'prowlarr: torrent and usenet indexer proxy'
  'autobrr: irc, torrent and usenet indexer proxy'
)
provides=(readarr)
conflicts=(readarr)
options=(!debug)
source=(
  'readarr.service'
  'readarr.tmpfiles'
  'readarr.sysusers'
  'package_info'
)
source_x86_64=("Readarr.nightly.${pkgver}.linux-core-x64.tar.gz::https://readarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Readarr.nightly.${pkgver}.linux-core-arm64.tar.gz::https://readarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Readarr.nightly.${pkgver}.linux-core-arm.tar.gz::https://readarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha256sums=('09386a5a87038f227e4a0995b37ac7ba561712712ad610323ea7ee8a1bf18c32'
            '3030252218445e3cb27025a1b567deef287ff3d5e2f32abc2d640a771d39ddd5'
            '1576aa21914edaa336d2b37d41ebf54fbaff6eb5099a3f46407cd79164ccdc67'
            '9b505e7e93a71c9d2fdc4689cf4a3cd691e3927b419cf5bb6e1aed43b5a91edc')
sha256sums_x86_64=('e9e8e386df862c79f7421ffd0929794eaed00be36e8e87b7c5f81c457702573c')
sha256sums_aarch64=('362563012a9aeed4aa1641c5e55dbc98b726e12158f50ace89c3f65ad087ff3b')
sha256sums_armv7h=('2bf1609daa120f96221bfcdde67409e60f4009c8ce31adf3cb63cf7f5f98ccc6')

package() {
  install -dm755 "${pkgdir}/usr/lib/readarr/bin"

  # License
  install -Dm644 "${srcdir}/Readarr/LICENSE.md" "${pkgdir}/usr/share/licenses/${pkgname}"
  rm "${srcdir}/Readarr/LICENSE.md"

  # Remove Service Helpers, and Update files
  rm "${srcdir}/Readarr/ServiceInstall"*
  rm "${srcdir}/Readarr/ServiceUninstall"*
  rm -rf "${srcdir}/Readarr/Readarr.Update"

  # Disable built in updater.
  install -Dm644 "${srcdir}/package_info" "${pkgdir}/usr/lib/readarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/readarr/package_info"

  cp -dpr --no-preserve=ownership "${srcdir}/Readarr/"* "${pkgdir}/usr/lib/readarr/bin"

  install -Dm644 "${srcdir}/readarr.service" "${pkgdir}/usr/lib/systemd/system/readarr.service"
  install -Dm644 "${srcdir}/readarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/readarr.conf"
  install -Dm644 "${srcdir}/readarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/readarr.conf"
}
