# Maintainer: Christian Himpel <chressie at gmail dot com>
# Contributor: Andres Perera <andres87p gmail>
# Contributor: Ashok Gautham <ScriptDevil at gmail dot com>
# Contributor: Matthew Bauer <mjbauer95@gmail.com>
# Contributor: Vesa Kaihlavirta <vegai@iki.fi>

pkgname=go-lang-hg
pkgver=5096
pkgrel=1
pkgdesc='Google Go compiler and tools (hg tip)'
arch=(i686 x86_64)
url=http://golang.org/
license=(custom)
depends=()
makedepends=(mercurial)
provides=(go)
conflicts=(go)
options=(!strip !makeflags)
install=go.install
source=(go.sh)

_hgroot=https://go.googlecode.com/hg/
_hgrepo=go

build() {
  local GOARCH GOROOT GOOS GOBIN
  export GOARCH GOROOT GOOS GOBIN

  [ x$CARCH = xi686 ]   && GOARCH=386
  [ x$CARCH = xx86_64 ] && GOARCH=amd64
  [ x$CARCH = xarm ]    && GOARCH=arm
  GOROOT=$srcdir/$_hgrepo
  GOOS=linux
  GOBIN=$GOROOT/bin

  mkdir -p $GOBIN
  cd $GOROOT/src || return 1

  LC_ALL=C PATH=$PATH:$GOBIN ./make.bash || return 1

  cd $GOROOT

  # The install directory
  GOROOT=$pkgdir/usr/lib/go

  install -Dm644 LICENSE $pkgdir/usr/share/licenses/go/LICENSE
  install -Dm644 misc/bash/go $pkgdir/etc/bash_completion.d/go
  install -Dm644 misc/emacs/go-mode-load.el $pkgdir/usr/share/emacs/site-lisp/go-mode-load.el
  install -Dm644 misc/emacs/go-mode.el $pkgdir/usr/share/emacs/site-lisp/go-mode.el
  install -Dm644 misc/vim/go.vim $pkgdir/usr/share/vim/vimfiles/syntax/go.vim

  mkdir -p $GOROOT/{misc,lib}

  cp -r bin $pkgdir/usr
  cp -r pkg $GOROOT
  rm $GOROOT/pkg/~place-holder~

  cp -r doc $GOROOT
  cp -r lib/godoc $GOROOT/lib
  find src/{pkg,cmd} -name \*.go -exec install -Dm644 {} $GOROOT/{} \;
  install -Dm644 {,$GOROOT/}src/pkg/container/vector/Makefile
  install -Dm644 {,$GOROOT/}favicon.ico
  ln -s ../../share/licenses/go/LICENSE $GOROOT
  cp -r misc/cgo $GOROOT/misc

  cp src/Make.{$GOARCH,cmd,common,pkg,conf} $GOROOT/src
  cp src/pkg/runtime/{cgocall,runtime}.h $GOROOT/src/pkg/runtime

  install -Dm755 $srcdir/go.sh $pkgdir/etc/profile.d/go.sh
  echo export GOARCH=$GOARCH >> $pkgdir/etc/profile.d/go.sh
}
md5sums=('67c472bfcfdb760d1d1f0a87cfe3661f')
