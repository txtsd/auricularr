# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Contributor: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://readarr.servarr.com/v1/update/nightly?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=readarr-nightly-bin
pkgver=0.4.5.2688
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
install=readarr.install
source=(
  'readarr.service'
  'readarr.tmpfiles'
  'readarr.sysusers'
  'readarr.install'
  'package_info'
)
source_x86_64=("Readarr.nightly.${pkgver}.linux-core-x64.tar.gz::https://readarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=x64")
source_aarch64=("Readarr.nightly.${pkgver}.linux-core-arm64.tar.gz::https://readarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm64")
source_armv7h=("Readarr.nightly.${pkgver}.linux-core-arm.tar.gz::https://readarr.servarr.com/v1/update/nightly/updatefile?version=${pkgver}&os=linux&runtime=netcore&arch=arm")
sha256sums=('4696e52bc1cd8b7860eeb1ffaefc4153c3bf27defd7e7e91da03ad8aa28aa3df'
            'a4cfdf882ab62dea54d85dfae4a633cf21bce597a19c3287d90c024e3ff399ce'
            '2a3a67ccd03d85396d7c08e8c5ef530b31c6d847102b35af705b43c0714a4a3a'
            '244ef89032a7af9fc22e9a9a600a3b9e407137c7d7d99ed227cafe2efb800a85'
            '9b505e7e93a71c9d2fdc4689cf4a3cd691e3927b419cf5bb6e1aed43b5a91edc')
sha256sums_x86_64=('8e118b563f175847035483eb057fe1bf15b8ce558954273b89727a79f2111ddf')
sha256sums_aarch64=('5afab2f81b097707400458180337b12740e8e21bf4a739c347dfafd013528701')
sha256sums_armv7h=('9b85047e23ee74b699638d018ed72e3a8c78409f7fed4383d816cd67efe1f309')

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
