# Maintainer: jab416171 <jab416171@gmail.com>
# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Contributor: Donald Webster <fryfrog@gmail.com>
# Contributor: Martins Mozeiko <martins.mozeiko@gmail.com>

pkgname=jellyseerr
pkgver=2.0.1
pkgrel=4
pkgdesc='Request management and media discovery tool for the Plex ecosystem'
arch=('x86_64' 'aarch64')
url='https://github.com/Fallenbagel/jellyseerr'
license=('MIT')
depends=(
  'bash'
  'glibc'
)
optdepends=(
  'jellyfin-server: The Free Software Media System'
  'plex-media-server: Plex Media Server'
  'emby-server: The open media solution'
)
makedepends=('pnpm' 'nodejs-lts-iron')
options=('!strip' '!debug')
backup=(
  'etc/conf.d/jellyseerr'
  'etc/jellyseerr/settings.json'
)
source=(
  "${pkgname}-${pkgver}.tar.gz::https://github.com/Fallenbagel/jellyseerr/archive/v${pkgver}.tar.gz"
  'jellyseerr.sysusers'
  'jellyseerr.tmpfiles'
  'jellyseerr.service'
  'jellyseerr.conf.d'
  '0001-Don-t-use-husky.patch'
  '0002-Remove-need-for-npm-npx.patch'
)
sha256sums=('3ea5ae6ffd68eac767acb9a36adb41f1dc4018232cd6dca0cc31668c1f5c005b'
            '372ee94f76040ea76af49fd2f9db851375559458ba1b55ea41f1b2768fe10cb8'
            '5a986d594045facc646f3f17ef0f3e053bc95df86f6f8fff5a793d9bdbbab0ed'
            'da984bb844c6dfbcb489bb43f8552b5e36cc25f5d975b63afd31757093ca82e8'
            '5a446cc8fa0a47a49dbbd6920d49eb4569f988e808cbb0bdbb609ab179a94426'
            '1d2ee741644c3dc4520c2e4f0213343429eadfda7231950e03170dddae73d9bc'
            'e2777886a0e7000bd680fbdc17547d42faeb4cebd7b0ddc6493630d70b3f9f42')

prepare() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  patch -Np1 -i "${srcdir}/0001-Don-t-use-husky.patch"
  patch -Np1 -i "${srcdir}/0002-Remove-need-for-npm-npx.patch"

  echo "{\"commitTag\": \"${pkgver}\"}" > committag.json

  export NEXT_TELEMETRY_DISABLED=1
  pnpm install --frozen-lockfile
}

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  mkdir -p .next "${srcdir}/.jellyseer_cache"
  rm -rf .next/cache  # In case previous builds have it as real folder
  ln -s "${srcdir}/.jellyseer_cache" .next/cache

  export NEXT_TELEMETRY_DISABLED=1
  export CYPRESS_INSTALL_BINARY=0
  pnpm build
  pnpm prune --prod --ignore-scripts
}

package() {
  depends+=('nodejs')
  cd "${srcdir}/${pkgname}-${pkgver}"

  install -dm755 "${pkgdir}/usr/lib/jellyseerr"
  install -dm755 "${pkgdir}/usr/lib/jellyseerr/.next/standalone/.next"
  cp -dr --no-preserve='ownership' "${srcdir}/${pkgname}-${pkgver}/"{.next,dist,public,node_modules} "${pkgdir}/usr/lib/jellyseerr"
  cp -d --no-preserve='ownership' "${srcdir}/${pkgname}-${pkgver}/"{package.json,overseerr-api.yml} "${pkgdir}/usr/lib/jellyseerr"

  find "${pkgdir}/usr/lib/jellyseerr/.next" -type f -print0 | xargs -0 sed -i "s^${srcdir}/${pkgname}-${pkgver}^/usr/lib/jellyseerr^g"

  ln -s "/var/lib/jellyseerr" "${pkgdir}/usr/lib/jellyseerr/config"

  install -Dm644 'LICENSE' "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install -Dm644 "${srcdir}/jellyseerr.conf.d"   "${pkgdir}/etc/conf.d/jellyseerr"
  install -Dm644 "${srcdir}/jellyseerr.sysusers" "${pkgdir}/usr/lib/sysusers.d/jellyseerr.conf"
  install -Dm644 "${srcdir}/jellyseerr.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/jellyseerr.conf"
  install -Dm644 "${srcdir}/jellyseerr.service"  "${pkgdir}/usr/lib/systemd/system/jellyseerr.service"
}
