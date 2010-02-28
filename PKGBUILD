# Maintainer: Christian Himpel <chressie at gmail dot com>
# Contributor: Andres Perera <andres87p gmail>
# Contributor: Ashok Gautham <ScriptDevil at gmail dot com>
# Contributor: Matthew Bauer <mjbauer95@gmail.com>
# Contributor: Vesa Kaihlavirta <vegai@iki.fi>

pkgname=go-lang-hg
pkgver=4965
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

  msg "Connecting to Mercurial server..."
  if [ -d $_hgrepo ]; then
    cd $_hgrepo
    hg pull -u || return 1
    msg2 "The local files have been updated"
  else
    hg clone $_hgroot $_hgrepo || return 1
    msg2 "Mercurial checkout done"
  fi


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

  mkdir -p $pkgdir/{etc/profile.d,usr/{share/go,lib/go,lib/go/src}}

  cp -r bin $pkgdir/usr
  cp -r doc misc -t $pkgdir/usr/share/go
  cp -r pkg $pkgdir/usr/lib/go

  # Headers for C modules
  install -Dm644 src/Make.{$GOARCH,cmd,pkg,conf} $pkgdir/usr/lib/go/src
  install -Dm644 src/pkg/runtime/runtime.h $pkgdir/usr/lib/go/src/pkg/runtime/runtime.h
  install -Dm644 src/pkg/runtime/cgocall.h $pkgdir/usr/lib/go/src/pkg/runtime/cgocall.h
  install  $srcdir/go.sh $pkgdir/etc/profile.d/

  echo export GOARCH=$GOARCH >> $pkgdir/etc/profile.d/go.sh
}
md5sums=('67c472bfcfdb760d1d1f0a87cfe3661f')
