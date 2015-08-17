# Maintainer: ZipFile <lin.aaa.lin@gmail.com>

pkgname=oggenc-gt3
_pkgname=vorbis-tools
pkgver=1.4.0
pkgrel=2
pkgdesc="Ogg Vorbis encoder statically linked with libvorbis GT3 beta 1"
arch=('i686' 'x86_64')
url="http://sjeng.org/vorbisgt3.html"
license=('BSD' 'GPL2')
depends=('libogg' 'flac')
options=('strip')

source=(quality.patch
        http://sjeng.org/ftp/vorbis/vorbisgt3b1src.zip
        http://downloads.xiph.org/releases/vorbis/${_pkgname}-${pkgver}.tar.gz)
md5sums=('189666723be709af52eba179a457c24e'
         'ef20679d66b18ccd2742479e3528ebb4'
         '567e0fb8d321b2cd7124f8208b8b90e6')

build() {
    cd "${srcdir}/vorbisgt3b1src"
    aclocal $ACLOCAL_FLAGS
    libtoolize --automake
    automake --add-missing $AUTOMAKE_FLAGS
    autoconf
    CFLAGS=${CFLAGS/-march=$CARCH} ./configure --prefix=/tmp \
        --disable-shared \
        --enable-static

    make DESTDIR="${srcdir}" install

    cd "${srcdir}/${_pkgname}-${pkgver}"
    patch -p0 < ../quality.patch;
    LIBS="-L${srcdir}/tmp/lib" ./configure --prefix=/tmp \
        --enable-static=vorbis \
        --disable-ogg123 \
        --disable-vorbistest \
        --disable-oggtest \
        --disable-curltest \
        --disable-oggdec \
        --disable-vcut \
        --disable-ogginfo \
        --disable-vorbiscomment \
        --without-kate \
        --without-speex

    make
}

check() {
    cd "${srcdir}/vorbisgt3b1src"
    make -j1 check
}

package() {
    cd "${srcdir}/${_pkgname}-${pkgver}"
    install -D -m755 oggenc/oggenc "${pkgdir}/usr/bin/${pkgname}"
    install -D -m644 COPYING "${pkgdir}/usr/share/licenses/$pkgname/LICENSE"

    cd "${srcdir}/vorbisgt3b1src"
    install -D -m644 COPYING "${pkgdir}/usr/share/licenses/$pkgname/LICENSE-GT3"
}
