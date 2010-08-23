# Maintainer: Christian Himpel <chressie gmail>
# Contributor: Andres Perera <andres87p gmail>
# Contributor: Ashok Gautham <ScriptDevil gmail>
# Contributor: Matthew Bauer <mjbauer95 gmail>
# Contributor: Vesa Kaihlavirta <vegai iki fi>

pkgname=go-hg
pkgver=6063
pkgrel=1
pkgdesc='Google Go compiler and tools (hg tip)'
arch=(i686 x86_64)
url=http://golang.org/
license=(custom)
depends=()
makedepends=(ed mercurial)
provides=(go)
conflicts=(go)
options=(!strip !makeflags)
install=go.install
source=(go.sh goinst)

_hgroot=https://go.googlecode.com/hg/
_hgrepo=go

build() {
  unset GOARCH GOROOT GOROOT_FINAL GOOS GOBIN
  export GOARCH GOROOT GOROOT_FINAL GOOS GOBIN

  [ x$CARCH = xi686 ]   && GOARCH=386
  [ x$CARCH = xx86_64 ] && GOARCH=amd64
  [ x$CARCH = xarm ]    && GOARCH=arm
  GOROOT=$srcdir/$_hgrepo
  GOROOT_FINAL=/opt/go
  GOOS=linux
  GOBIN=$GOROOT/bin

  mkdir -p $GOBIN
  cd $GOROOT/src

  # compile
  LC_ALL=C ./make.bash

  cd ..

  # install all files
  mkdir -p $pkgdir/$GOROOT_FINAL
  find * -type f ! -executable -print0 | xargs -0 -I {} install -Dm644 {} $pkgdir/$GOROOT_FINAL/{}
  find * -type f -executable -print0 | xargs -0 -I {} install -Dm755 {} $pkgdir/$GOROOT_FINAL/{}

  # adjust permissions
  chmod -R g+w $pkgdir/$GOROOT_FINAL
  find $pkgdir/$GOROOT_FINAL -type d -print0 | xargs -0 chmod g+s

  install -Dm644 LICENSE $pkgdir/usr/share/licenses/go/LICENSE
  install -Dm644 misc/bash/go $pkgdir/etc/bash_completion.d/go
  install -Dm644 misc/emacs/go-mode-load.el $pkgdir/usr/share/emacs/site-lisp/go-mode-load.el
  install -Dm644 misc/emacs/go-mode.el $pkgdir/usr/share/emacs/site-lisp/go-mode.el
  install -Dm644 misc/vim/syntax/go.vim $pkgdir/usr/share/vim/vimfiles/syntax/go.vim
  install -Dm644 misc/vim/ftdetect/gofiletype.vim $pkgdir/usr/share/vim/vimfiles/ftdetect/go.vim
  install -Dm755 $srcdir/go.sh $pkgdir/etc/profile.d/go.sh
  echo export GOARCH=$GOARCH >> $pkgdir/etc/profile.d/go.sh
  install -Dm755 $srcdir/goinst $pkgdir/$GOROOT_FINAL/bin/goinst
}
md5sums=('c8ae7a640bc7cf79bc0366223f93b28a'
         '304436b6ab490f98f0028c5ed4b82bbf')
