# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Contributor: Daniel Egeberg <daniel.egeberg@gmail.com>
# Contributor: Justin Dray <justin@dray.be>
# Helpful URL: https://services.sonarr.tv/v1/releases

pkgname=sonarr
_pkgname=Sonarr
pkgver=4.0.10.2544
pkgrel=3
pkgdesc='Smart PVR for newsgroup and torrent users.'
arch=('x86_64' 'aarch64' 'armv7h')
url='https://sonarr.tv'
license=('GPL-3.0-or-later')
groups=('servarr')
depends=(
  'aspnet-runtime-6.0'
  'gcc-libs'
  'glibc'
  'sqlite'
  'ffmpeg'
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
options=(!debug)
source=(
  "${pkgname}-${pkgver}.tar.gz::https://github.com/Sonarr/Sonarr/archive/refs/tags/v${pkgver}.tar.gz"
  'package_info'
  'sonarr.service'
  'sonarr.sysusers'
  'sonarr.tmpfiles'
)
sha256sums=('d6f1b1d04ea572cfd2aba72d8adac1925f544c43b99f31f3a047588f8369be78'
            '19112dc0051224b4de66f28077c93b6ee06e163b5194e6aecf62dedf66ff45a9'
            'b26aa01e07e5864b588ebe51a2993eaafb03fa0f7ec3806f2996dd2daf46aee7'
            '047585a1d448ad2c6e2962fb60d4f71e01a2529e464b25d340bb0d31b8e0f08f'
            '7bf87304383b7d58ecab59b3686d00a8f1b6fbe4af3a86da35a887e4cebee411')

case ${CARCH} in
  x86_64)  _CARCH='x64';;
  aarch64) _CARCH='arm64';;
  armv7h)  _CARCH='arm';;
esac

_framework='net6.0'
_runtime="linux-${_CARCH}"
_output="_output"
_artifacts="${_output}/${_framework}/${_runtime}/publish"
_branch='main'

prepare() {
  cd "${srcdir}/${_pkgname}-${pkgver}"

  # Remove upstream dotnet version
  rm global.json

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
    -t:PublishAllRids \
  && dotnet build-server shutdown   # Build servers do not terminate automatically

  # Remove ffprobe, Service Helpers, Update, and Windows files
  rm "${_artifacts}/ffprobe"
  rm "${_artifacts}/ServiceInstall"*
  rm "${_artifacts}/ServiceUninstall"*
  rm "${_artifacts}/Sonarr.Windows."*
  rm -rf "${_output}/Sonarr.Update"

  # Build frontend
  yarn run build --env production
}

check() {
  cd "${srcdir}/${_pkgname}-${pkgver}"
  local _filters="Category!=ManualTest&Category!=AutomationTest&Category!=WINDOWS"

  # Skip Tests:
  # These tests fail because /etc/arch-release doesn't contain a ${VERSION_ID}
  # See: https://github.com/Sonarr/Sonarr/issues/7299
  # Integration tests also completely fail
  _filters="${_filters}&FullyQualifiedName!~should_get_version_info"
  _filters="${_filters}&FullyQualifiedName!~should_get_version_info_from_actual_linux"
  _filters="${_filters}&Category!=IntegrationTest"

  # Prepare for tests
  mkdir -p ~/.config/Radarr

  # Test backend
  dotnet test src \
    --runtime "${_runtime}" \
    --configuration Release \
    --filter "${_filters}" \
    --no-build
}

package() {
  cd "${srcdir}/${_pkgname}-${pkgver}"
  install -dm755 "${pkgdir}/usr/lib/sonarr/bin/UI"

  # Copy backend
  cp -dpr --no-preserve=ownership "${_artifacts}/"* "${pkgdir}/usr/lib/sonarr/bin"
  # Copy frontend
  cp -dpr --no-preserve=ownership "${_output}/UI/"* "${pkgdir}/usr/lib/sonarr/bin/UI"

  # Use system ffprobe
  ln -s /usr/bin/ffprobe "${pkgdir}/usr/lib/sonarr/bin/ffprobe"

  # License
  install -Dm644 "${srcdir}/${_pkgname}-${pkgver}/LICENSE.md" "${pkgdir}/usr/share/licenses/${pkgname}"

  # Disable built in updater.
  install -Dm644 "${srcdir}/package_info" "${pkgdir}/usr/lib/sonarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/sonarr/package_info"

  install -Dm644 "${srcdir}/sonarr.service" "${pkgdir}/usr/lib/systemd/system/sonarr.service"
  install -Dm644 "${srcdir}/sonarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/sonarr.conf"
  install -Dm644 "${srcdir}/sonarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/sonarr.conf"
}
