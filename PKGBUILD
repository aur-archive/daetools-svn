# Maintainer: Raymond Smith <raymondbarrettsmith at gmail dot com>
pkgname=daetools-svn
_pkgname=daetools
pkgver=413
pkgrel=1
pkgdesc="Tools for solving differential algebraic equations (DAEs)"
arch=('i686' 'x86_64')
url="http://daetools.sourceforge.net"
license=('GPL3')
depends=('python2' 'python2-numpy' 'python2-scipy' 'python2-matplotlib' 'python2-pyqt4')
makedepends=('svn' 'python2-distribute' 'mayavi' 'lapack' 'blas' 'qt4' 'qtcreator' 'gcc-fortran'
             'cmake' 'wget' 'freetype2' 'swig' 'libpng12' 'libxext')
provides=('daetools')
conflicts=('daetools')
source=('svn://svn.code.sf.net/p/daetools/code/trunk')
sha256sums=('SKIP')

pkgver() {
  cd "${SRCDEST}/trunk"
  svnversion
}

prepare() {
  cd "${srcdir}/trunk"
  sed -i 's|strap.sh|strap.sh --with-python-version=${PYTHON_VERSION}|' compile_libraries_linux.sh
  sed -i 's|python -c|${PYTHON} -c|' compile_libraries_linux.sh
}

build() {
  cd "${srcdir}/trunk"
  sh compile_libraries_linux.sh --with-python-version 2.7 all
  sh compile_linux.sh --with-python-version 2.7 all
}

package() {
  cd "${srcdir}/trunk/daetools-package"
  python2 setup.py install --prefix=/usr --root="${pkgdir}" --optimize=1
  # Fix the bin scripts to call python2
  cd "${pkgdir}"
  sed -i "s|python|python2|" usr/bin/daeplotter
  sed -i "s|python|python2|" usr/bin/daeexamples
  # Don't put stuff in lib64, which is actually just a symlink to lib
  mv usr/lib64/* usr/lib
  rmdir usr/lib64
}

# vim:set ts=2 sw=2 et tw=0:
