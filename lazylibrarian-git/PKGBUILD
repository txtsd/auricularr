# Maintainer: txtsd <aur.archlinux@ihavea.quest>
# Maintainer: Donald Webster <fryfrog@gmail.com>

pkgname=lazylibrarian-git
pkgver=r6050.8a9bc8e6
pkgrel=1
pkgdesc='Ebook, audiobook and magazine collection manager for newsgroup and torrent users'
arch=(any)
url='https://gitlab.com/LazyLibrarian/LazyLibrarian/'
license=('GPL-3.0-or-later')
depends=(
  apprise
  bash
  python
  python-beautifulsoup4
  python-certifi
  python-charset-normalizer
  python-cherrypy
  python-cherrypy-cors
  python-cryptography
  python-dateutil
  python-deluge-client
  python-html5lib
  python-httpagentparser
  python-httplib2
  python-idna
  python-irc
  python-jaraco.stream
  python-levenshtein
  python-magic
  python-mako
  python-markupsafe
  python-pillow
  python-portend
  python-pyopenssl
  python-pyparsing
  python-pypdf3
  python-rapidfuzz
  python-requests
  python-setuptools
  python-six
  python-soupsieve
  python-thefuzz
  python-urllib3
  python-webencodings
  python-yaml
  unrar
)
makedepends=(git)
provides=(lazylibrarian)
conflicts=(lazylibrarian)
install=lazylibrarian.install
source=(
  'git+https://gitlab.com/LazyLibrarian/LazyLibrarian.git'
  lazylibrarian.service
  lazylibrarian.sysusers
  lazylibrarian.tmpfiles
)

sha256sums=('SKIP'
            'b2a04e1184c4e592187e32fbeb30b8c95742951b809814971f830bd2c05e0fc2'
            '4bfc8d0836e328ed28ef28f366e8e367f7b39b85f472ae9d34012c8b749bf6fd'
            '6cddd4de91618e5ee62b15916bcb623b612dfdf1ddcef7b3aa74d3eb62587604')

prepare() {
  cd LazyLibrarian
  sed -i -e 's/except (OSError, IOError), e:/except (OSError, IOError, e):/g' \
    -e 's/except IOError, e:/except (IOError, e):/g' \
    -e 's/except pywintypes.error, e:/except (pywintypes.error, e):/g' \
    -e 's/except OSError, e:/except (OSError, e):/g' \
    -e 's/except select.error, e:/except (select.error, e):/g' \
    -e 's/print$/print()/g' \
    -e 's/print "\(.*\)"/print("\1")/g' \
    -e 's/print >>sys.stderr, "\(.*\)"\(.*\)/print("\1", file=sys.stderr\2)/g' \
    -e 's/print \(.*\)/print(\1)/g' \
    cherrypy/_cpcompat_subprocess.py
  sed -i -e 's/except SSL.SysCallError, e:/except (SSL.SysCallError, e):/g' \
    -e 's/except SSL.Error, e:/except (SSL.Error, e):/g' \
    cherrypy/wsgiserver/ssl_pyopenssl.py
  sed -i -e 's/except socket.error, e:/except (socket.error, e):/g' \
    -e 's/except socket.error, serr:/except (socket.error, serr):/g' \
    -e 's/raise exc_info\[0\], exc_info\[1\], exc_info\[2\]/raise error.with_traceback(exc_info\[2\])/' \
    cherrypy/wsgiserver/wsgiserver2.py
  sed -i 's/print "\(.*\)"/print("\1")/g' example_mega_link.py
  sed -i 's/print "\(.*\)"/print("\1")/g' gsconvert.py
}

pkgver() {
  cd LazyLibrarian
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

package() {
  install -dm755 "${pkgdir}/usr/lib/lazylibrarian"
  cp -dpr --no-preserve=ownership LazyLibrarian/* "${pkgdir}/usr/lib/lazylibrarian"
  rm -rf "${pkgdir}/usr/lib/lazylibrarian/init"
  rm -rf "${pkgdir}/usr/lib/lazylibrarian/LazyLibrarian.app"
  rm -rf "${pkgdir}/usr/lib/lazylibrarian/unittests"
  rm "${pkgdir}/usr/lib/lazylibrarian/ISSUE_TEMPLATE.md"
  rm "${pkgdir}/usr/lib/lazylibrarian/README.md"
  rm "${pkgdir}/usr/lib/lazylibrarian/UNITTESTING.md"

  python -m compileall "${pkgdir}/usr/lib/lazylibrarian"

  install -dm755 "${pkgdir}/usr/share/licenses/${pkgname}"
  ln -s "/usr/lib/lazylibrarian/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  install -Dm644 lazylibrarian.service "${pkgdir}/usr/lib/systemd/system/lazylibrarian.service"
  install -Dm644 lazylibrarian.sysusers "${pkgdir}/usr/lib/sysusers.d/lazylibrarian.conf"
  install -Dm644 lazylibrarian.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/lazylibrarian.conf"
}
