# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Contributor: Anatol Pomozov <anatol dot pomozov at gmail>

pkgname=meson
pkgver=0.41.1+8+g1be77026
pkgrel=2
pkgdesc='High productivity build system'
url='http://mesonbuild.com/'
arch=('any')
license=('Apache')
depends=('python' 'ninja')
makedepends=('python-setuptools' 'git')
checkdepends=('gcc-objc' 'vala' 'rust' 'gcc-fortran' 'mono' 'boost' 'qt4' 'qt5-base' 'git' 'gnustep-base'
              'cython' 'gtkmm3' 'gtest' 'gmock' 'protobuf' 'wxgtk' 'python-gobject' 'gobject-introspection'
              'itstool' 'gtk3' 'valgrind' 'ldc' 'java-environment>=8' 'gtk-doc' 'llvm' 'clang' 'sdl2'
              'doxygen')
_commit=1be77026373469ce4d71650adbba236df031a9f4  # 0.41
source=("git+https://github.com/mesonbuild/meson#commit=$_commit"
        0001-pkgconfig-avoid-appending-slash-at-Cflags.patch)
sha512sums=('SKIP'
            'ed7a147cb353981c1558bbb3d870f42e214b3ee0d33366ec8f289479bcbfed467183f6398fb6ab3091f2ff334f17943da22985cc500ee0b25d5ffee99726b85f')
validpgpkeys=('95181F4EED14FDF4E41B518D3BF4693BFEEB9428') # Jussi Pakkanen <jpakkane@gmail.com>

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $pkgname
  # fix FS#54763
  patch -Np1 -i ../0001-pkgconfig-avoid-appending-slash-at-Cflags.patch
}

build() {
  cd $pkgname
  python setup.py build
}

check() {
  cd $pkgname
  unset CLASSPATH  # GNUstep breaks java tests
  # Installing graphviz breaks doxygen tests
  LC_CTYPE=en_US.UTF-8 DC=ldc ./run_tests.py
}

package() {
  cd $pkgname
  python setup.py install --root="${pkgdir}" --optimize=1 --skip-build
  install -Dm 644 syntax-highlighting/vim/ftdetect/meson.vim -t "${pkgdir}/usr/share/vim/vimfiles/ftdetect"
  install -Dm 644 syntax-highlighting/vim/indent/meson.vim -t "${pkgdir}/usr/share/vim/vimfiles/indent"
  install -Dm 644 syntax-highlighting/vim/syntax/meson.vim -t "${pkgdir}/usr/share/vim/vimfiles/syntax"
}

# vim: ts=2 sw=2 et:
