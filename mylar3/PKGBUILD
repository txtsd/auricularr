# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>

pkgname=mylar3
pkgver=0.8.0
pkgrel=2
pkgdesc='Comic book download automation for usenet and torrent users'
arch=(any)
url='https://github.com/mylar3/mylar3'
license=('GPL-3.0-or-later')
depends=(
  bash
  python
  python-apscheduler
  python-beautifulsoup4
  python-cfscrape
  python-cheroot
  python-cherrypy
  python-configparser
  python-feedparser
  python-mako
  python-natsort
  python-packaging
  python-pillow
  python-portend
  python-pycryptodome
  python-pystun3
  python-pytz
  python-rarfile
  python-requests
  python-simplejson
  python-six
  python-tenacity
  python-tzlocal
  python-urllib3
  python-user_agent2
  unrar
)
optdepends=(
  'sabnzbd: usenet downloader'
  'nzbget: usenet downloader'
  'qbittorrent: torrent downloader'
  'deluge: torrent downloader'
  'rtorrent: torrent downloader'
  'vuze: torrent downloader'
  'transmission-cli: torrent downloader (CLI and daemon)'
  'transmission-gtk: torrent downloader (GTK+)'
  'transmission-qt: torrent downloader (Qt)'
  'prowlarr: automatically integrates with and syncs indexers'
)
source=(
  "${pkgname}-${pkgver}.tar.gz::${url}/archive/v${pkgver}.tar.gz"
  mylar3.service
  mylar3.tmpfiles
  mylar3.sysusers
)

sha256sums=('14bc2b0578f58f26f3b63958637f889573f7b5eee13ed4eaf75375f2a0faf678'
            'bfe7c28f5c773b77e6bc0014bd4473de2f4650d049cfedb177aedef74187c932'
            'e39441d41acd5d66b63631872e06f8e5425bf0725044e5c3843626d4a090eba5'
            'e26cad50c8c8501fa12e2e2b2443f57aaf309e28f28d6dd4944958f9cb20f3cd')

prepare() {
  cd "${pkgname}-${pkgver}"

  sed -i 's/urllib2.HTTPError,e/\(urllib2.HTTPError,e\)/' lib/utorrent/client.py
}

package() {
  cd "${pkgname}-${pkgver}"

  install -dm755 "${pkgdir}/usr/lib/mylar3"
  cp -dpr --no-preserve=ownership * "${pkgdir}/usr/lib/mylar3"
  python -m compileall "${pkgdir}"

  cd "${srcdir}"
  install -Dm644 "mylar3.service" "${pkgdir}/usr/lib/systemd/system/mylar3.service"
  install -Dm644 "mylar3.sysusers" "${pkgdir}/usr/lib/sysusers.d/mylar3.conf"
  install -Dm644 "mylar3.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/mylar3.conf"
}
