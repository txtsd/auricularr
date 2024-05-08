# Maintainer: Donald Webster <fryfrog@gmail.com>
# Contributor: Pieter Goetschalckx <3.14.e.ter <at> gmail <dot> com>

pkgname=bazarr
pkgver=1.4.3
_pkgsubver="beta.32"
pkgrel=1
pkgdesc="Subtitle management and download automation for Sonarr and Radarr."
arch=('any')
url="https://github.com/morpheus65535/bazarr"
license=('GPL3')
options=('!strip' 'staticlibs')
makedepends=(
  'unzip'
  'python-setuptools'
)
depends=(
  'sqlite'
  'python'
  'python-gevent'
  'python-gevent-websocket'
  'python-lxml'
  'python-numpy'
  'python-pillow'
  'python-webrtcvad'
  'ffmpeg'
  'unrar'
)
source=(
  "bazarr-${pkgver}-${_pkgsubver}.zip::https://github.com/morpheus65535/bazarr/releases/download/v${pkgver}-${_pkgsubver}/bazarr.zip"
  'bazarr.service'
  'bazarr.sysusers'
  'bazarr.install'
  'bazarr.tmpfiles'
)
noextract=("bazarr-${pkgver}-${_pkgsubver}.zip")

sha256sums=('d2f9abe120008afe4244222b7c9bec4c12f67c158cca93a3ad1b57a40614db1e'
            'a6f6a045e8a5d3ce6185148ee7b0b9fd323b6480ae284f6ceead4ca71e6fd109'
            '92fd48cbd7e5fe3a0388bbe756a52098fc461ef2dc87d9e886452e4f15acdcdc'
            '573beeac951d427e980332ce4d8645ae2299082e6c9c04f96e2a41a98c3acc60'
            'e7055260d0f3554e8b628d9560d8e12a40f720d76542048df0dfc838db88357b')

prepare() {
  unzip -qq -o -d bazarr-${pkgver}-${_pkgsubver} bazarr-${pkgver}-${_pkgsubver}.zip
}

package() {
  install -d -m 755 "${pkgdir}/usr/lib/bazarr"

  cp -dpr --no-preserve=ownership "${srcdir}/bazarr-${pkgver}-${_pkgsubver}/"* "${pkgdir}/usr/lib/bazarr"

  install -D -m 644 "${srcdir}/bazarr.service" "${pkgdir}/usr/lib/systemd/system/bazarr.service"
  install -D -m 644 "${srcdir}/bazarr.sysusers" "${pkgdir}/usr/lib/sysusers.d/bazarr.conf"
  install -D -m 644 "${srcdir}/bazarr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/bazarr.conf"
}
