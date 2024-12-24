# Maintainer: Janek Thomaschewski <janek@thomaschewski.dev>
# Contributor: Steve Engledow <steve@engledow.me>

pkgname=amazon-workspaces-bin
pkgver=2024.8.5130
pkgrel=1
_aptdist=focal
pkgdesc='Amazon Workspace Client'
arch=('x86_64')
url="https://clients.amazonworkspaces.com/"
license=('non-free')
depends=(
    'gtk3'
    'webkit2gtk'
    'icu63'
    'libsoup'
    'graphicsmagick'
    'hiredis0.14'
    'openssl-1.1'
    'libva'
)
options=('staticlibs')
makedepends=(
  'binutils'
  'tar'
)
source=(
    "$pkgname-$pkgver.deb::https://d3nt0h4h6pmmc4.cloudfront.net/new_workspacesclient_${_aptdist}_amd64.deb"
    "$pkgname-$pkgver.info::https://d3nt0h4h6pmmc4.cloudfront.net/ubuntu/dists/${_aptdist}/main/binary-amd64/Packages"
    lsb_release
    workspacesclient-wrapper
)

sha256sums=('880157ef0361b696b34e5ad0957252bc3fd91b8b7a3b992623901fd3880120ca'
            'b86729bff47a50f07005b6d3df8449bb594d07ef5fc07cb928b3982763bfb164'
            'SKIP'
            'SKIP')

prepare() {
    # Verify the checksum
    echo "$(grep SHA256 "$pkgname-$pkgver.info" | cut -d" " -f2) $pkgname-$pkgver.deb" >sum
    sha256sum -c sum

    ar x "$pkgname-$pkgver.deb"
    tar axvf data.tar.xz
    tar axvf control.tar.xz

    # Put "fake" lsb_release into PATH to report Ubuntu 20.x
    mkdir -p ${srcdir}/usr/lib/${arch}-linux-gnu/workspacesclient/helper-bin
    install -m 0755 ${srcdir}/lsb_release ${srcdir}/usr/lib/${arch}-linux-gnu/workspacesclient/helper-bin/

    install -m 0755 ${srcdir}/workspacesclient-wrapper ${srcdir}/usr/bin/

    sed -i -e 's/Exec=workspacesclient/Exec=workspacesclient-wrapper/' ${srcdir}/usr/share/applications/com.amazon.workspacesclient.desktop
}

pkgver() {
    grep Version control | cut -d" " -f2
}

package() {
  mkdir -p $pkgdir/usr

  cp -a $srcdir/usr/* $pkgdir/usr/
}
