# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://readarr.servarr.com/v1/update/develop?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=readarr-develop
_pkgname=Readarr
pkgver=0.4.4.2686
pkgrel=2
pkgdesc='Ebook and audiobook collection manager for newsgroup and torrent users (develop branch)'
arch=(x86_64 aarch64 armv7h)
url='https://readarr.com'
license=('GPL-3.0-or-later')
groups=(servarr-develop)
depends=(
  aspnet-runtime-6.0
  gcc-libs
  glibc
  sqlite
)
makedepends=(dotnet-sdk-6.0 yarn)
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
  "${pkgname}-${pkgver}.tar.gz::https://github.com/Readarr/Readarr/archive/refs/tags/v${pkgver}.tar.gz"
  package_info
  readarr.service
  readarr.sysusers
  readarr.tmpfiles
)
sha256sums=('619c62d08cadff327ea442c0e0a378837ff20c219fe2d9a32a6346e48e3b946f'
            '12235af27b47fe1a353bc79fdfdf8c0e03fca5c0eb08f9eb57c0b66532c37648'
            '4696e52bc1cd8b7860eeb1ffaefc4153c3bf27defd7e7e91da03ad8aa28aa3df'
            '2a3a67ccd03d85396d7c08e8c5ef530b31c6d847102b35af705b43c0714a4a3a'
            'a4cfdf882ab62dea54d85dfae4a633cf21bce597a19c3287d90c024e3ff399ce')

case ${CARCH} in
  x86_64) _CARCH='x64' ;;
  aarch64) _CARCH='arm64' ;;
  armv7h) _CARCH='arm' ;;
esac

_framework='net6.0'
_runtime="linux-${_CARCH}"
_output="_output"
_artifacts="${_output}/${_framework}/${_runtime}/publish"
_branch='develop'

prepare() {
  cd "${_pkgname}-${pkgver}"

  # Prepare backend
  export DOTNET_CLI_TELEMETRY_OPTOUT=1
  export DOTNET_NOLOGO=1
  export DOTNET_SKIP_FIRST_TIME_EXPERIENCE=1
  dotnet restore src/${_pkgname}.sln \
    --runtime ${_runtime} \
    --locked-mode

  # Prepare frontend
  yarn install --frozen-lockfile --network-timeout 120000
}

build() {
  cd "${_pkgname}-${pkgver}"

  # Build backend
  export DOTNET_CLI_TELEMETRY_OPTOUT=1
  export DOTNET_NOLOGO=1
  export DOTNET_SKIP_FIRST_TIME_EXPERIENCE=1
  dotnet build src/${_pkgname}.sln \
    --framework ${_framework} \
    --runtime ${_runtime} \
    --no-self-contained \
    --no-restore \
    --configuration Release \
    -p:Platform=Posix \
    -p:AssemblyVersion=${pkgver} \
    -p:AssemblyConfiguration=${_branch} \
    -p:RuntimeIdentifiers=${_runtime} \
    -t:PublishAllRids

  # Remove Service Helpers, Update, and Windows files
  rm "${_artifacts}/ServiceInstall"*
  rm "${_artifacts}/ServiceUninstall"*
  rm "${_artifacts}/Readarr.Windows."*

  # Build frontend
  yarn run build --env production
}

package() {
  cd "${_pkgname}-${pkgver}"
  install -dm755 "${pkgdir}/usr/lib/readarr/bin/UI"

  # Copy backend
  cp -dr "${_artifacts}/"* "${pkgdir}/usr/lib/readarr/bin"
  # Copy frontend
  cp -dr "${_output}/UI/"* "${pkgdir}/usr/lib/readarr/bin/UI"

  # License
  install -Dm644 LICENSE.md "${pkgdir}/usr/share/licenses/${pkgname}"

  # Disable built in updater.
  cd "${srcdir}"
  install -Dm644 package_info "${pkgdir}/usr/lib/readarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/readarr/package_info"

  # Systemd
  install -Dm644 readarr.service "${pkgdir}/usr/lib/systemd/system/readarr.service"
  install -Dm644 readarr.sysusers "${pkgdir}/usr/lib/sysusers.d/readarr.conf"
  install -Dm644 readarr.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/readarr.conf"
}
