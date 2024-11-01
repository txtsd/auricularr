# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>
# Contributor: George Rawlinson <grawlinson@archlinux.org>

pkgname=autobrr
pkgver=1.48.0
pkgrel=2
pkgdesc='The modern download automation tool for torrents'
arch=('x86_64')
url='https://autobrr.com'
license=('GPL-2.0-or-later')
depends=('glibc')
makedepends=(
  'git'
  'go'
  'pnpm'
)
optdepends=(
  'postgresql: postgresql database'
  'sabnzbd: usenet downloader'
  'qbittorrent: torrent downloader'
  'deluge: torrent downloader'
  'rtorrent: torrent downloader'
  'transmission-cli: torrent downloader (CLI and daemon)'
  'transmission-gtk: torrent downloader (GTK+)'
  'transmission-qt: torrent downloader (Qt)'
  'porla: A high performance BitTorrent client for servers and seedboxes'
  'sonarr: Smart PVR for newsgroup and torrent users'
  'radarr: Movie organizer/manager for usenet and torrent users'
  'lidarr: Music collection manager for newsgroup and torrent users'
  'readarr: Ebook and audiobook collection manager for newsgroup and torrent users'
  'whisparr: Adult movie organizer/manager for usenet and torrent users'
)
options=()
source=(
  "${pkgname}-${pkgver}::git+https://github.com/autobrr/autobrr#tag=v${pkgver}"
  'autobrr.service'
  'autobrr.sysusers'
  'autobrr.tmpfiles'
)
sha256sums=('53ff55e5ac1811225b8a05454fcac341d554429abcaff837f0b7bd167f18a09a'
            '90b920d9a1b0ad616e340556f826e8354ce75a331548370726de253ad5c302d6'
            '780adccd60ca1ca465668d163d19bb892df7c42e03e442bda0c095ca3ee6eed1'
            'c8d7972c67963aa3f720176e2fb351f3c0ea8be8f1ed84a46b7a13ea8d032bb3')

prepare() {
  cd "${pkgname}-${pkgver}"

  # Create directory for build output
  mkdir build

  # Download go dependencies
  export GOPATH="${srcdir}"
  go mod download

  # Download node dependencies
  cd web
  pnpm install --frozen-lockfile
}

build() {
  cd "${pkgname}-${pkgver}"

  # Ensure build date is reproducible
  local _build_date="$(date --utc --date="@${SOURCE_DATE_EPOCH:-$(date +%s)}" +%Y-%m-%d)"

  # Build web app
  pushd web
  pnpm run build
  popd

  # Set Go flags
  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export CGO_LDFLAGS="${LDFLAGS}"
  export GOPATH="${srcdir}"

  # Build binaries
  go build -v \
    -buildmode=pie \
    -mod=readonly \
    -modcacherw \
    -ldflags "-compressdwarf=false \
      -linkmode external \
      -X main.version=${pkgver} \
      -X main.commit=$(git rev-parse --verify HEAD) \
      -X main.date=${_build_date}" \
    -o build \
    ./cmd/...
}

check() {
  cd "${pkgname}-${pkgver}"
  go test -v ./test/...
}

package() {
  # Systemd integration
  install -Dm644 autobrr.service "${pkgdir}/usr/lib/systemd/system/${pkgname}.service"
  install -Dm644 autobrr.sysusers "${pkgdir}/usr/lib/sysusers.d/${pkgname}.conf"
  install -Dm644 autobrr.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/${pkgname}.conf"

  cd "${pkgname}-${pkgver}"

  # Binary
  install -Dm755 -t "${pkgdir}/usr/bin" build/*

  # Documentation
  install -Dm644 -t "${pkgdir}/usr/share/doc/${pkgname}" README.md
}
