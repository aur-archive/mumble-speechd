# Maintainer: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Lauri Niskanen <ape@ape3000.com>
# Contributor: Sebastian.Salich@gmx.de
# Contributor: Doc Angelo

# If you want support for your G15 Keyboard, please add 'g15daemon'
# to the depends and delete "no-g15" in the configure line below

appname=mumble
pkgname=$appname-speechd
pkgver=1.2.8
pkgrel=1
arch=('i686' 'x86_64')
pkgdesc="A voice chat application similar to TeamSpeak, Speech-Dispatcher support added"
license=('BSD')
depends=('qt4' 'speex' 'lsb-release' 'libxi' 'avahi' 'libsndfile' 'protobuf' 'libpulse' 'opus' 'speech-dispatcher' 'xdg-utils' 'espeak')
makedepends=('boost' 'mesa')
#optdepends=('portaudio: for portaudio back-end' 'g15daemon: G15 Keyboard support')
replaces=('mumble')
conflicts=('mumble')
provides=('mumble')
install=mumble.install
url="http://mumble.sourceforge.net/"
source=("http://downloads.sourceforge.net/mumble/$appname-$pkgver.tar.gz"
"gcc49.patch::https://github.com/mumble-voip/mumble/commit/349436284b5f1baa61836c98ff0d518392140c5d.patch")
md5sums=('1a3ef91489ff674dfc010377d7721a28'
         '9a1c254352dd4bb9fe4ba2f7471fb030')

prepare() {
  cd $srcdir/$appname-$pkgver

  patch -Np1 < $srcdir/gcc49.patch
}

build() {
  cd $srcdir/$appname-$pkgver

  # Building mumble
  qmake-qt4 main.pro \
    CONFIG+="bundled-celt no-bundled-opus no-bundled-speex no-g15 no-xevie no-server \
    no-embed-qt-translations no-update packaged" \
    DEFINES+="PLUGIN_PATH=/usr/lib/mumble" \
    INCLUDEPATH+="/usr/include/speech-dispatcher"
  make release
}

package() {
  cd $srcdir/$appname-$pkgver

  # bin stuff
  install -m755 -D ./release/mumble $pkgdir/usr/bin/mumble
  install -m755 -D ./scripts/mumble-overlay $pkgdir/usr/bin/mumble-overlay

  # lib stuff
  install -m755 -D ./release/libmumble.so.$pkgver $pkgdir/usr/lib/mumble/libmumble.so.$pkgver
  ln -s libmumble.so.$pkgver $pkgdir/usr/lib/mumble/libmumble.so
  ln -s libmumble.so.$pkgver $pkgdir/usr/lib/mumble/libmumble.so.1
  ln -s libmumble.so.$pkgver $pkgdir/usr/lib/mumble/libmumble.so.1.2
  install -m755 -D ./release/plugins/liblink.so $pkgdir/usr/lib/mumble/liblink.so
  install -m755 -D ./release/plugins/libmanual.so $pkgdir/usr/lib/mumble/libmanual.so
  install -m755 -D ./release/libcelt* $pkgdir/usr/lib/mumble/

  # other stuff
  install -m644 -D ./scripts/mumble.desktop $pkgdir/usr/share/applications/mumble.desktop
  install -m755 -d $pkgdir/usr/share/man/man1
  install -m644 -D ./man/mum* $pkgdir/usr/share/man/man1/
  install -m644 -D ./icons/mumble.svg $pkgdir/usr/share/icons/hicolor/scalable/apps/mumble.svg
  install -m644 -D ./LICENSE $pkgdir/usr/share/licenses/$appname/LICENSE
}
# vim: sw=2:ts=2 et:
