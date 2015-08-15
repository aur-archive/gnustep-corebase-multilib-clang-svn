# Maintainer: X0rg

_svnname=gnustep-corebase
pkgname=$_svnname-multilib-clang-svn
pkgver=38274
pkgrel=6
pkgdesc="The GNUstep CoreBase Library is a library of general-purpose, non-graphical C objects for multilib, using Clang."
arch=('x86_64')
url="http://www.gnustep.org/"
license=('GPL3' 'LGPL2.1')
groups=('gnustep-multilib-clang-svn')
depends=('gnustep-base-multilib-clang-svn' 'lib32-libdispatch-clang-git' 'libdispatch-clang-git')
makedepends=('svn' 'clang' 'gnustep-make-multilib-clang-svn')
provides=('gnustep-corebase-clang-svn')
conflicts=('gnustep-corebase-git' 'gnustep-corebase-clang-svn')
options=('!emptydirs')
install=merge.install
source=("$_svnname::svn://svn.gna.org/svn/gnustep/libs/corebase/trunk/")
md5sums=('SKIP')

pkgver() {
  cd "$_svnname"
  svnversion | tr -d [A-z]
}

prepare() {
  msg2 "Make a clone of $_svnname"
  cp -Rv "$srcdir/$_svnname" "$srcdir/$_svnname-32"

  msg2 "Fix 32-bit build error..."
  sed -i -e 's|#error 96-bit long double currently not supported!|//#error 96-bit long double currently not supported!|g' "$srcdir/$_svnname-32/Source/GSUnicode.c"
}

build() {
  # 64-bit build
  cd "$srcdir/$_svnname"
  msg2 "Run 'configure' (64-bit)..."
  OBJCFLAGS="-fblocks" CC="clang" CXX="clang++" ./configure --prefix=/usr --sysconfdir=/etc/GNUstep

  msg2 "Run 'make' (64-bit)..."
  make

  # 32-bit build
  cd "$srcdir/$_svnname-32"
  source "/usr/share/GNUstep32/Makefiles/GNUstep.sh"
  export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"

  msg2 "Run 'configure' (32-bit)..."
  OBJCFLAGS="-fblocks" CC="clang -m32" CXX="clang++ -m32" ./configure --prefix=/usr --libdir=/usr/lib32 --sysconfdir=/etc/GNUstep

  msg2 "Run 'make' (32-bit)..."
  make
}

# check() {
#   cd "$srcdir/$_svnname"
#   make check
#
#   cd "$srcdir/$_svnname-32"
#   make check
# }

package() {
  # 64-bit install
  cd "$srcdir/$_svnname"
  msg2 "Install (64-bit)..."
  GNUSTEP_CONFIG_FILE="/etc/GNUstep/GNUstep.conf" make DESTDIR="$pkgdir" install

  # 32-bit install
  cd "$srcdir/$_svnname-32"
  msg2 "Install (32-bit)..."
  GNUSTEP_CONFIG_FILE="/etc/GNUstep/GNUstep32.conf" make DESTDIR="$pkgdir" install
}
