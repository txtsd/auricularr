# Maintainer: Donald Webster <fryfrog@gmail.com>
# Maintainer: Devin Buhl <devin.kray@gmail.com>
pkgname="radarr"
pkgver="0.2.0.196"
pkgrel=1
pkgdesc="Movie downloader for usenet and torrents."
arch=(any)
url="https://github.com/galli-leo/Radarr"
license=('GPL3')
depends=('mono' 'libmediainfo' 'sqlite')
optdepends=('sabnzbd: an NZB downloader'
            'nzbget: an NZB downloader'
            'transmission-cli: a torrent downloader (CLI and daemon version)'
            'transmission-gtk: a torrent downloader (GTK+ version)'
            'transmission-qt: a torrent downloader (Qt version)'
            'deluge: a torrent downloader')
install='radarr.install'
provides=('radarr')
source=("https://github.com/galli-leo/Radarr/releases/download/v${pkgver}/Radarr.develop.${pkgver}.linux.tar.gz"
        "radarr.sh"
        "radarr.service")
noextract=()

sha512sums=('51da69d36d73031b2182ebf4a69f746b459f814a288d345e2ac5b62a94c2b0dbb696b8e0e2410bff69148a847cdc96c3c6a878ba6c2123d43df8b428c9775e3b'
            '17c0a54de94ab6e4523ab5f3f65bd40b592cc6723acaf123db8bbf0d2c6200e4b5877878c542fe849c09b9748ab8c8887cbd94365ba431cf71fc1256f24b43ec'
            '58260b3d1e0638aa2269fb81a077686c203ae638d6d93712eb0552e93b8b5f517f6d104464b119d5d38ea521e130e3ee80579113526433337f0578576917b7e1')

package() {
    cd "$srcdir"

    install -d -m 755 "${pkgdir}/var/lib/radarr"

    msg2 "Install Radarr in /usr/lib"
    install -d -m 755 "${pkgdir}/usr/lib/radarr"
    cp -dpr --no-preserve=ownership "${srcdir}/Radarr/"* "${pkgdir}/usr/lib/radarr"

    msg2 "Fixing permissions in /usr/lib/radarr/"
    find "${pkgdir}/usr/lib/radarr" -type f -exec chmod 644 '{}' ';'

    msg2 "Install executable into /usr/bin"
    install -D -m755 "${srcdir}/radarr.sh" "${pkgdir}/usr/bin/radarr"

    msg2 "Install radarr.service"
    install -D -m 644 "${srcdir}/radarr.service" "${pkgdir}/usr/lib/systemd/system/radarr.service"
}
