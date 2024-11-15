# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://prowlarr.servarr.com/v1/update/master?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=prowlarr
_pkgname=Prowlarr
pkgver=1.25.4.4818
pkgrel=5
pkgdesc='Indexer manager/proxy for usenet and torrent users.'
arch=(x86_64 aarch64 armv7h)
url='https://prowlarr.com'
license=('GPL-3.0-or-later')
groups=(servarr)
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
  'sonarr: automatically integrates with and syncs indexers'
  'radarr: automatically integrates with and syncs indexers'
  'lidarr: automatically integrates with and syncs indexers'
  'readarr: automatically integrates with and syncs indexers'
  'whisparr: automatically integrates with and syncs indexers'
)
source=(
  "${pkgname}-${pkgver}.tar.gz::https://github.com/Prowlarr/Prowlarr/archive/refs/tags/v${pkgver}.tar.gz"
  package_info
  prowlarr.install
  prowlarr.service
  prowlarr.sysusers
  prowlarr.tmpfiles
)
sha256sums=('6987634df7e668b09a6f7ca40d044adf42ab1eaac0cb63a73325fdfa225e2d98'
            '1f9f8018436bd1e29a36c203639083d614722b65a4db64e22bf0c3295fc03fb8'
            '328fda2e2ed5f93440f3837a7d4f2c91e35fb4f6c51aeb33eacb80ecceb941a9'
            '8253f405c6dd1261ed321702867b08b413f8d69ef3d081949583d9106e3f812b'
            'ee61f5621eae6ab932fb093a4f75a0ab11bf9e3ca829f0d34c25014f68aeff7d'
            '4c3f9b5fa71810697efbe60f20a2cba24fd1b997d5372c3726457b197d61ccb5')

case ${CARCH} in
  x86_64) _CARCH='x64' ;;
  aarch64) _CARCH='arm64' ;;
  armv7h) _CARCH='arm' ;;
esac

_framework='net6.0'
_runtime="linux-${_CARCH}"
_output='_output'
_artifacts="${_output}/${_framework}/${_runtime}/publish"
_branch='master'

prepare() {
  cd "${_pkgname}-${pkgver}"

  # Prepare backend
  export DOTNET_CLI_TELEMETRY_OPTOUT=1
  export DOTNET_NOLOGO=1
  export DOTNET_SKIP_FIRST_TIME_EXPERIENCE=1
  dotnet restore "src/${_pkgname}.sln" \
    --runtime "${_runtime}" \
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
  dotnet build "src/${_pkgname}.sln" \
    --framework "${_framework}" \
    --runtime "${_runtime}" \
    --no-self-contained \
    --no-restore \
    --configuration Release \
    -p:Platform=Posix \
    -p:AssemblyVersion=${pkgver} \
    -p:AssemblyConfiguration=${_branch} \
    -p:RuntimeIdentifiers=${_runtime} \
    -t:PublishAllRids \
    && dotnet build-server shutdown # Build servers do not terminate automatically

  # Remove Service Helpers, Update, and Windows files
  rm "${_artifacts}/ServiceInstall"*
  rm "${_artifacts}/ServiceUninstall"*
  rm "${_artifacts}/Prowlarr.Windows."*

  # Build frontend
  yarn run build --env production
}

package() {
  cd "${_pkgname}-${pkgver}"
  install -dm755 "${pkgdir}/usr/lib/prowlarr/bin/UI"

  # Copy backend
  cp -dpr --no-preserve=ownership "${_artifacts}/"* "${pkgdir}/usr/lib/prowlarr/bin"
  # Copy frontend
  cp -dpr --no-preserve=ownership "${_output}/UI/"* "${pkgdir}/usr/lib/prowlarr/bin/UI"

  # License
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}"

  cd "${srcdir}"
  # Disable built in updater.
  install -Dm644 package_info "${pkgdir}/usr/lib/prowlarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/prowlarr/package_info"

  install -Dm644 prowlarr.service "${pkgdir}/usr/lib/systemd/system/prowlarr.service"
  install -Dm644 prowlarr.sysusers "${pkgdir}/usr/lib/sysusers.d/prowlarr.conf"
  install -Dm644 prowlarr.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/prowlarr.conf"
}
