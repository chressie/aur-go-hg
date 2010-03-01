# Maintainer: Christian Himpel <chressie at gmail dot com>
# Contributor: Andres Perera <andres87p gmail>
# Contributor: Ashok Gautham <ScriptDevil at gmail dot com>
# Contributor: Matthew Bauer <mjbauer95@gmail.com>
# Contributor: Vesa Kaihlavirta <vegai@iki.fi>

pkgname=go-lang-hg
pkgver=4967
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
  cd $srcdir

  ARCH=$(uname -m)
  [ "$ARCH" == "i686" ]   && GOARCH=386
  [ "$ARCH" == "arm" ]    && GOARCH=arm
  [ "$ARCH" == "x86_64" ] && GOARCH=amd64
  export GOARCH

  export GOROOT=$srcdir/$_hgrepo
  export GOOS=linux
  export GOBIN=$GOROOT/bin
  export PATH=$PATH:$GOBIN

  mkdir $GOROOT/bin &> /dev/null
  cd $GOROOT/src || return 1

  LC_ALL=C ./make.bash || return 1

  cd $GOROOT || return 1
  install -Dm644 LICENSE $pkgdir/usr/share/licenses/go/LICENSE
  install -Dm644 misc/bash/go $pkgdir/etc/bash_completion.d/go
  install -Dm644 misc/emacs/go-mode-load.el $pkgdir/usr/share/emacs/site-lisp/go-mode-load.el
  install -Dm644 misc/emacs/go-mode.el $pkgdir/usr/share/emacs/site-lisp/go-mode.el
  install -Dm644 misc/vim/go.vim $pkgdir/usr/share/vim/vimfiles/syntax/go.vim

  mkdir -p $pkgdir/usr/bin
  mkdir -p $pkgdir/etc/profile.d
  mkdir -p $pkgdir/opt/go/{lib,src}

  cp -r bin $pkgdir/usr
  cp -r doc pkg $pkgdir/opt/go
  cp -r lib/godoc $pkgdir/opt/go/lib

  # Headers for C modules
  install -m644 src/Make.{$GOARCH,cmd,pkg,conf} $pkgdir/opt/go/src
  install -Dm644 src/pkg/runtime/runtime.h $pkgdir/opt/go/src/pkg/runtime/runtime.h
  install -Dm644 src/pkg/runtime/cgocall.h $pkgdir/opt/go/src/pkg/runtime/cgocall.h
  install -Dm755 $srcdir/go.sh $pkgdir/etc/profile.d
  find src/pkg -name '*.go' -exec install -Dm644 {} $pkgdir/opt/go/{} \;

  echo export GOARCH=$GOARCH >> $pkgdir/etc/profile.d/go.sh
}
md5sums=('b7cd21a7a00922216036b3a992c60435')
