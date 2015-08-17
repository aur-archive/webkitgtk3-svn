# Maintainer: Jonas Heinrich <onny@project-insanity.org>

pkgname=webkitgtk3-svn
pkgver=146744
pkgrel=6
pkgdesc="GTK+ Web content engine library for GTK+ 3.0"
conflicts=('libwebkit3')
provides=("libwebkit3=${pkgver}")
replaces=('libwebkit3' 'webkitgtk3')
arch=('i686' 'x86_64')
url="http://webkitgtk.org/"
license=('custom')
depends=('libwebp' 'libxt' 'libxslt' 'sqlite' 'libsoup' 'enchant' 'libgl' 'geoclue' 'gtk2' 'gtk3' 'gst-plugins-base-libs')
makedepends=('subversion' 'libxt' 'libxslt' 'sqlite' 'libsoup' 'enchant' 'libgl' 'geoclue' 'gtk2' 'gtk3' 'gst-plugins-base-libs' 'gstreamer0.10-base' 'gperf' 'gobject-introspection' 'mesa' 'ruby' 'gtk-doc' 'libsecret' 'python')
options=('!libtool' '!emptydirs')

_svntrunk="http://svn.webkit.org/repository/webkit/trunk"
_svnmod="webkitgtk3-svn"

build() {
  cd "$srcdir"
  msg "Connecting to SVN server...."

  if [[ -d "$_svnmod/.svn" ]]; then
    (cd "$_svnmod" && svn up -r "$pkgver")
  else
    svn co "$_svntrunk" --config-dir ./ -r "$pkgver" "$_svnmod"
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_svnmod-build"
  svn export "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  cd "$srcdir/$_svnmod-build"

  #
  # BUILD HERE
  #
  ./autogen.sh
  ./configure --prefix=/usr \
    --enable-introspection \
    --disable-silent-rules \
    --libexecdir=/usr/lib/webkitgtk3 \
    --with-gstreamer=1.0

  make -j4 all stamp-po
}

package() {
  cd "$srcdir/$_svnmod-build"
  make -j1 DESTDIR="$pkgdir/" install
  install -Dm644 Source/WebKit/LICENSE "$pkgdir/usr/share/licenses/${pkgname}/LICENSE"
}
