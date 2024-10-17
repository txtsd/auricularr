# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Contributor: Donald Webster <fryfrog@gmail.com>
# Contributor: Sven Frenzel <aur@frenzel.dk>
# Helpful URL: https://services.lidarr.audio/v1/update/master?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=lidarr
_pkgname=Lidarr
pkgver=2.6.4.4402
pkgrel=1
pkgdesc='Music collection manager for newsgroup and torrent users.'
arch=('x86_64' 'aarch64' 'armv7h')
url='https://lidarr.tv'
license=('GPL-3.0-or-later')
groups=('servarr')
depends=(
  'aspnet-runtime-6.0'
  'chromaprint'
  'gcc-libs'
  'glibc'
  'sqlite'
)
makedepends=('dotnet-sdk-6.0' 'yarn')
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
  'prowlarr: torrent and usenet indexer proxy'
)
options=(!debug)
source=(
  "${pkgname}-${pkgver}.tar.gz::https://github.com/Lidarr/Lidarr/archive/refs/tags/v${pkgver}.tar.gz"
  'package_info'
  'lidarr.service'
  'lidarr.sysusers'
  'lidarr.tmpfiles'
)
sha256sums=('15929ac7f37a88e6dab523579330200fdbeb6b54bc0b8f9a032650bdafa7dc7f'
            '19435dff2251782714875af95a38d8491cee6c178e8f32e3c3b6e566b3edc931'
            'dcbe3d2a3d64a78a4b2b84a3486991a8b90fdd6900d7345004827a168f8b5645'
            '19b36aefd2ef93d4a630ceaefe582573ecdaa72ec21bfb48ce3941ead7b967fb'
            'abde8989e7ab9dc62b6a501644da5a9253953b6394b890565e00a69cbfd89068')

case ${CARCH} in
  x86_64)  _CARCH='x64';;
  aarch64) _CARCH='arm64';;
  armv7h)  _CARCH='arm';;
esac

_framework='net6.0'
_runtime="linux-${_CARCH}"
_output="_output"
_artifacts="${_output}/${_framework}/${_runtime}/publish"

prepare() {
  cd "${srcdir}/${_pkgname}-${pkgver}"

  # Fix CVE-2024-43485
  sed 's/System\.Text\.Json" Version="6\.0\.9"/System\.Text\.Json" Version="6\.0\.10"/' -i src/NzbDrone.Common/Lidarr.Common.csproj
  sed 's/System\.Text\.Json" Version="6\.0\.9"/System\.Text\.Json" Version="6\.0\.10"/' -i src/NzbDrone.Core/Lidarr.Core.csproj

  yarn install --frozen-lockfile --network-timeout 120000
}

build() {
  cd "${srcdir}/${_pkgname}-${pkgver}"

  export DOTNET_CLI_TELEMETRY_OPTOUT=1
  dotnet build src/${_pkgname}.sln \
    --framework ${_framework} \
    --runtime ${_runtime} \
    --no-self-contained \
    --configuration Release \
    -p:Platform=Posix \
    -p:AssemblyVersion=${pkgver} \
    -p:AssemblyConfiguration=main \
    -p:RuntimeIdentifiers=${_runtime} \
    -t:PublishAllRids \
  && dotnet build-server shutdown   # Build servers do not terminate automatically

  # Remove Service Helpers, Update, and Windows files
  rm "${_artifacts}/ServiceInstall."*
  rm "${_artifacts}/ServiceUninstall."*
  rm "${_artifacts}/Lidarr.Windows."*

  # Use fpcalc from chromaprint package
  rm -f "${_artifacts}/fpcalc"

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

  # Link build to tests
  ln -sf ../../${_artifacts} _tests/${_framework}/${_runtime}/bin
  mkdir -p ~/.config/Lidarr

  dotnet test src \
    --runtime "${_runtime}" \
    --configuration Release \
    --filter "${_filters}" \
    --no-build
}

package() {
  cd "${srcdir}/${_pkgname}-${pkgver}"
  install -dm755 "${pkgdir}/usr/lib/lidarr/bin/UI"

  cp -dpr --no-preserve=ownership "${_artifacts}/"* "${pkgdir}/usr/lib/lidarr/bin"
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
