# Contributor: Vladimir Ermakov <vooon341@gmail.com>

pkgname=stage-svn
pkgver=7618
pkgrel=1
pkgdesc="Robotic systems environment"
arch=(i686 x86_64)
url="http://playerstage.sf.net"
license=('GPL')
depends=('player-svn' 'fltk' 'glib2' 'libpng')
provides=('stage')
conflicts=('stage')
makedepends=('pkgconfig' 'cmake' 'subversion')
install=
source=()

_svntrunk=http://playerstage.svn.sourceforge.net/svnroot/playerstage/code/stage/trunk/
_svnmod=$pkgname

build() {
  cd $startdir/src

  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn up -r $pkgver)
  else
    svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
  fi

  msg "SVN checkout done or server timeout"

  rm -rf $_svnmod-build
  cp -r $_svnmod $_svnmod-build
  cd $_svnmod-build

  msg "Patching..."
  patch -Np0 < ../fix-cmake-set-error.patch

  msg "Starting CMake..."
  cmake -D CMAKE_INSTALL_PREFIX=/usr -D BUILD_DOCUMENTATION=ON . || return 1
  make || return 1
  make DESTDIR=$startdir/pkg install || return 1
}
# vim:set ts=2 sw=2 et:
