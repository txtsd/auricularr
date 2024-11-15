# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Helpful URL: https://radarr.servarr.com/v1/update/develop?version=0.0.0.0&os=linux&runtime=netcore&arch=x64

pkgname=radarr-develop
_pkgname=Radarr
pkgver=5.15.0.9412
pkgrel=4
pkgdesc='Movie organizer/manager for usenet and torrent users (develop branch)'
arch=(x86_64 aarch64 armv7h)
url='https://radarr.video'
license=('GPL-3.0-or-later')
groups=(servarr-develop)
depends=(
  aspnet-runtime-6.0
  ffmpeg
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
provides=(radarr)
conflicts=(radarr)
install=radarr.install
source=(
  "${pkgname}-${pkgver}.tar.gz::https://github.com/Radarr/Radarr/archive/refs/tags/v${pkgver}.tar.gz"
  package_info
  radarr.install
  radarr.service
  radarr.sysusers
  radarr.tmpfiles
)
sha256sums=('271135fa4d7bb24e406850c170e66f3c7a23c680abd122a86c0c9e63c7d83833'
            '4a41a56ab30f8b6001a666e867c7012bfe23760ec29eac957bf90e1dcb4ee36e'
            '243ded7d0e9d59b9adf912bb4e35ba63247d85577b417b54dcd74f16f0cfbd26'
            '8ca13537e98380b91f1a950187d6b9f021f8a4d68871f709444742a4911bc5a6'
            'bb73e0c55711d7ddbf74140b3beb39cb8674ae92be8387c3dd8109bcd53faca8'
            'c68efcb3778cb497d7c256dc97df7413ce09f07ea341e4d2683e7fee321cbcbb')

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
  rm "${_artifacts}/Radarr.Windows."*
  rm -rf "${_output}/Radarr.Update"

  # Build frontend
  yarn run build --env production
}

package() {
  cd "${_pkgname}-${pkgver}"
  install -dm755 "${pkgdir}/usr/lib/radarr/bin/UI"

  # Copy backend
  cp -dpr --no-preserve=ownership "${_artifacts}/"* "${pkgdir}/usr/lib/radarr/bin"
  # Copy frontend
  cp -dpr --no-preserve=ownership "${_output}/UI/"* "${pkgdir}/usr/lib/radarr/bin/UI"

  # Use system ffprobe
  ln -s /usr/bin/ffprobe "${pkgdir}/usr/lib/radarr/bin/ffprobe"

  # License
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}"

  cd "${srcdir}"
  # Disable built in updater.
  install -Dm644 package_info "${pkgdir}/usr/lib/radarr"
  echo "PackageVersion=${pkgver}-${pkgrel}" >> "${pkgdir}/usr/lib/radarr/package_info"

  install -Dm644 radarr.service "${pkgdir}/usr/lib/systemd/system/radarr.service"
  install -Dm644 radarr.sysusers "${pkgdir}/usr/lib/sysusers.d/radarr.conf"
  install -Dm644 radarr.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/radarr.conf"
}
