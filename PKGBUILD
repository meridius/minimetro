# Maintainer: XZS <d dot f dot fischer at web dot de>
# Contributor: Martin Lukes <martin.meridius@gmail.com>
# Contributor: Samuel Littley <samuel.littley@toastwaffle.com>
# Contributor: William Giokas <1007380@gmail.com>

pkgname=minimetro
_pkgname=MiniMetro
pkgver=gamma12
pkgrel=1
pkgdesc='minimalistic subway layout game'
url="http://dinopoloclub.com/${pkgname}/"
license=('custom:None')
arch=('i686' 'x86_64')
depends=('libgl' 'libx11' 'glu' 'desktop-file-utils' 'gtk-update-icon-cache')
makedepends=('imagemagick')
DLAGENTS+=('hib::/usr/bin/echo "Could not find %u. Manually download it to \"$(pwd)\", or set up a hib:// DLAGENT in /etc/makepkg.conf."; exit 1')
install=desktop.install
source=("hib://${_pkgname}-${pkgver}-linux.tar.gz"
    "${pkgname}.desktop"
    "${pkgname}.png::http://dinopoloclub.com/press/mini_metro/images/icon.png")
md5sums=('61f1aa0108aa595f284d459f2a146353'
    '0fe19f989f9606a825b67b9b379c9d6c'
    '3919f3d0e165b41fe3ef2dfe59252090')

prepare() {
    case $CARCH in
        i686) _notarch=x86_64 ;;
        x86_64) _notarch=x86 ;;
    esac
    find -name "*$_notarch" -exec rm -r {} +
}

package() {
    # First, install the game itself.
    destdir="$pkgdir/opt/$pkgname"
    install -dm 755 "$destdir"
    cp -r ${_pkgname}{_,.}* "$destdir"

    # Make the log file writable by the 'games' group. This is needed to run.
    log="$destdir"/${_pkgname}_Data/log.txt
    touch "$log"
    chmod 664 "$log"
    chown :games "$log"

    # Now, care for supplementary files.
    local sizes=(16 22 24 32 36 48 64 72 96 128 192 256 384 512)
    for size in "${sizes[@]}"; do
        size=${size}x${size}
        install -dm 755 "$pkgdir"/usr/{bin,share/{applications,icons/hicolor/${size}/apps}} "$destdir"
        convert ${pkgname}.png -resize ${size} "$pkgdir"/usr/share/icons/hicolor/${size}/apps/${pkgname}.png
    done

    echo "#!/opt/$pkgname/${_pkgname}.$CARCH" > "$pkgdir"/usr/bin/${pkgname}
    chmod +x "$pkgdir"/usr/bin/$pkgname

    cp ${pkgname}.desktop "$pkgdir"/usr/share/applications
}
