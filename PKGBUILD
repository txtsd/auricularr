# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Contributor: Daniel Egeberg <daniel.egeberg@gmail.com>
# Contributor: Justin Dray <justin@dray.be>
# Helpful URL: https://services.sonarr.tv/v1/releases

pkgname=sonarr-develop
_pkgname=Sonarr
pkgver=4.0.10.2656
pkgrel=3
pkgdesc='Smart PVR for newsgroup and torrent users (develop branch)'
arch=(x86_64 aarch64 armv7h)
url='https://sonarr.tv'
license=('GPL-3.0-or-later')
groups=(servarr-develop)
depends=(
  aspnet-runtime-6.0
  gcc-libs
  glibc
  sqlite
  ffmpeg
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
provides=(sonarr)
conflicts=(sonarr)
install=sonarr.install
source=(
  "${pkgname}-${pkgver}.tar.gz::https://github.com/Sonarr/Sonarr/archive/refs/tags/v${pkgver}.tar.gz"
  package_info
  sonarr.service
  sonarr.sysusers
  sonarr.tmpfiles
  sonarr.install
)
sha256sums=('7865b1a4d7b6eca7ccbc3fc199c368b4ad8beeafdbaa042aded9c3a4699f415d'
            'a6b37e75143a309b1d8c163c3f90f7f0275fd730015c3f74e3ad27c278b1ae90'
            '64d1e9bafeaa6f47329222d31fbcf2dbb575566d391334ea034a857e144dfe62'
            '3a52a20b0fd62d3ce830089347610d4f0f914d2ad6fdd278320f784c8b5d9087'
            'd6b18a83dd9c213470d984f71ddcefcd64d12bb87f68225cc4ebf5fa4a831703'
            '3d912d367eeb89ead06dc9dc45de093f48ddc601188731d54775c33e04e369aa')

case ${CARCH} in
  x86_64) _CARCH='x64' ;;
  aarch64) _CARCH='arm64' ;;
  armv7h) _CARCH='arm' ;;
esac

_framework='net6.0'
_runtime="linux-${_CARCH}"
_output='_output'
_artifacts="${_output}/${_framework}/${_runtime}/publish"
_branch='develop'

prepare() {
  cd "${_pkgname}-${pkgver}"

  # Fix CVE-2024-43485
  sed 's/System\.Text\.Json" Version="6\.0\.9"/System\.Text\.Json" Version="6\.0\.10"/' -i src/NzbDrone.Common/Sonarr.Common.csproj
  sed 's/System\.Text\.Json" Version="6\.0\.9"/System\.Text\.Json" Version="6\.0\.10"/' -i src/NzbDrone.Core/Sonarr.Core.csproj

  # Remove upstream dotnet version
  rm global.json

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
    -p:RuntimeIdentifiers="${_runtime}" \
    -t:PublishAllRids \
    && dotnet build-server shutdown # Build servers do not terminate automatically

  # Remove ffprobe, Service Helpers, Update, and Windows files
  rm "${_artifacts}/ffprobe"
  rm "${_artifacts}/ServiceInstall"*
  rm "${_artifacts}/ServiceUninstall"*
  rm "${_artifacts}/Sonarr.Windows."*
  rm -rf "${_output}/Sonarr.Update"

  # Build frontend
  yarn run build --env production
}

package() {
  cd "${_pkgname}-${pkgver}"
  install -dm755 "${pkgdir}/usr/lib/sonarr/bin/UI"

  # Copy backend
  cp -dpr --no-preserve=ownership "${_artifacts}/"* "${pkgdir}/usr/lib/sonarr/bin"
  # Copy frontend
  cp -dpr --no-preserve=ownership "${_output}/UI/"* "${pkgdir}/usr/lib/sonarr/bin/UI"

  # Use system ffprobe
  ln -s /usr/bin/ffprobe "${pkgdir}/usr/lib/sonarr/bin/ffprobe"

  # License
  install -Dm644 LICENSE.md "${pkgdir}/usr/share/licenses/${pkgname}"

  cd "${srcdir}"
  # Disable built in updater.
  install -Dm644 package_info "${pkgdir}/usr/lib/sonarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/sonarr/package_info"

  # Systemd
  install -Dm644 sonarr.service "${pkgdir}/usr/lib/systemd/system/sonarr.service"
  install -Dm644 sonarr.sysusers "${pkgdir}/usr/lib/sysusers.d/sonarr.conf"
  install -Dm644 sonarr.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/sonarr.conf"
}
