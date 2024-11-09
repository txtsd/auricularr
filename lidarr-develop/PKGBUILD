# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://services.lidarr.audio/v1/update/develop?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=lidarr-develop
_pkgname=Lidarr
pkgver=2.8.0.4431
pkgrel=3
pkgdesc='Music collection manager for newsgroup and torrent users (develop branch)'
arch=('x86_64' 'aarch64' 'armv7h')
url='https://lidarr.audio'
license=('GPL-3.0-or-later')
groups=('servarr-develop')
depends=(
  'aspnet-runtime-6.0'
  'chromaprint'
  'gcc-libs'
  'glibc'
  'sqlite'
)
makedepends=('dotnet-sdk-6.0' 'yarn')
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
provides=(lidarr)
conflicts=(lidarr)
options=(!debug)
install=lidarr.install
source=(
  "${pkgname}-${pkgver}.tar.gz::https://github.com/Lidarr/Lidarr/archive/refs/tags/v${pkgver}.tar.gz"
  'package_info'
  'lidarr.service'
  'lidarr.sysusers'
  'lidarr.tmpfiles'
  'lidarr.install'
)
sha256sums=('a68b71f978d81856393dc26940b2d5e01e9bbb11a60dd99c3b923bca2d4e9f67'
            'ea7786c48dce48db0ecfe2feace923d122981f6af86a01985c3662a76153a620'
            '1a542d493eafbd28ac268c5f9ef29688ffa6e9326436d2ef05eb66413c18a082'
            'cd3eac6e50c421460b6215b0a6322374a0cc190fb841fddad4b196a759fc00d6'
            'd71e37213ac65722e42f6f2c5772d4515c2d28a77b9f7608dc05c787d86ebaa5'
            '2f3eeca41a77cec8e86a107365b34a29bf1fc2c5251173f7b200d81b318bca40')

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
  cd "${srcdir}/${_pkgname}-${pkgver}"

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
  cd "${srcdir}/${_pkgname}-${pkgver}"

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

  # Remove fpcalc, Service Helpers, Update, and Windows files
  rm "${_artifacts}/fpcalc"
  rm "${_artifacts}/ServiceInstall"*
  rm "${_artifacts}/ServiceUninstall"*
  rm "${_artifacts}/Lidarr.Windows."*

  # Build frontend
  yarn run build --env production
}

check() {
  cd "${srcdir}/${_pkgname}-${pkgver}"
  local _filters="Category!=ManualTest&Category!=AutomationTest&Category!=WINDOWS"

  # Skip Tests:
  # These tests fail because /etc/arch-release doesn't contain a ${VERSION_ID}
  # See: https://github.com/Lidarr/Lidarr/issues/7299
  _filters="${_filters}&FullyQualifiedName!~should_get_version_info"
  _filters="${_filters}&FullyQualifiedName!~should_get_version_info_from_actual_linux"
  _filters="${_filters}&Category!=IntegrationTest"

  # Prepare for tests
  mkdir -p ~/.config/Lidarr

  # Test backend
  dotnet test src \
    --runtime "${_runtime}" \
    --configuration Release \
    --filter "${_filters}" \
    --no-build
}

package() {
  cd "${srcdir}/${_pkgname}-${pkgver}"
  install -dm755 "${pkgdir}/usr/lib/lidarr/bin/UI"

  # Copy backend
  cp -dpr --no-preserve=ownership "${_artifacts}/"* "${pkgdir}/usr/lib/lidarr/bin"
  # Copy frontend
  cp -dpr --no-preserve=ownership "${_output}/UI/"* "${pkgdir}/usr/lib/lidarr/bin/UI"

  # License
  install -Dm644 "${srcdir}/${_pkgname}-${pkgver}/LICENSE.md" "${pkgdir}/usr/share/licenses/${pkgname}"

  # Disable built in updater.
  install -Dm644 "${srcdir}/package_info" "${pkgdir}/usr/lib/lidarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/lidarr/package_info"

  install -Dm644 "${srcdir}/lidarr.service" "${pkgdir}/usr/lib/systemd/system/lidarr.service"
  install -Dm644 "${srcdir}/lidarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/lidarr.conf"
  install -Dm644 "${srcdir}/lidarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/lidarr.conf"
}
