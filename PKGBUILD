#Maintainer: Doug Newgard <scimmia22 at outlook dot com>

pkgname=etrophy-svn
pkgver=80958
pkgrel=1
pkgdesc="EFL library that manages scores, trophies and unlockables"
arch=('i686' 'x86_64')
url="http://www.enlightenment.org"
license=('BSD')
depends=('elementary')
makedepends=('subversion')
conflicts=('etrophy')
provides=('etrophy')
options=('!libtool' '!emptydirs')

_svntrunk="http://svn.enlightenment.org/svn/e/trunk/PROTO/etrophy"
_svnmod="etrophy"

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

  ./autogen.sh --prefix=/usr

  make
}

package() {
  cd "$srcdir/$_svnmod-build"
  make DESTDIR="$pkgdir" install

# install license files
  install -Dm644 "$srcdir/$_svnmod-build/COPYING" \
        "$pkgdir/usr/share/licenses/$pkgname/COPYING"
  install -Dm644 "$srcdir/$_svnmod-build/AUTHORS" \
        "$pkgdir/usr/share/licenses/$pkgname/AUTHORS"

  rm -r "$srcdir/$_svnmod-build"
}
