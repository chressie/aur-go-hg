# Maintainer: Christian Himpel <chressie gmail>
# Contributor: Andres Perera <andres87p gmail>
# Contributor: Ashok Gautham <ScriptDevil gmail>
# Contributor: Matthew Bauer <mjbauer95 gmail>
# Contributor: Vesa Kaihlavirta <vegai iki fi>

pkgname=go-hg
pkgver=5935
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
source=(go.sh goinst)

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

  # compile
  LC_ALL=C PATH=$PATH:$GOBIN ./make.bash || return 1

  cd $GOROOT

  # install all files
  mkdir -p $pkgdir/opt/go $pkgdir/usr/bin
  find * -type f ! -executable -print0 | xargs -0 -I {} install -Dm644 {} $pkgdir/opt/go/{}
  find * -type f -executable -print0 | xargs -0 -I {} install -Dm755 {} $pkgdir/opt/go/{}

  # install binaries in /usr/bin
  mv $pkgdir/opt/go/bin $pkgdir/usr

  # adjust permissions
  chmod -R g+w $pkgdir/opt/go
  find $pkgdir/opt/go -type d -print0 | xargs -0 chmod g+s

  install -Dm644 LICENSE $pkgdir/usr/share/licenses/go/LICENSE
  install -Dm644 misc/bash/go $pkgdir/etc/bash_completion.d/go
  install -Dm644 misc/emacs/go-mode-load.el $pkgdir/usr/share/emacs/site-lisp/go-mode-load.el
  install -Dm644 misc/emacs/go-mode.el $pkgdir/usr/share/emacs/site-lisp/go-mode.el
  install -Dm644 misc/vim/syntax/go.vim $pkgdir/usr/share/vim/vimfiles/syntax/go.vim
  install -Dm644 misc/vim/ftdetect/gofiletype.vim $pkgdir/usr/share/vim/vimfiles/ftdetect/go.vim
  install -Dm755 $srcdir/go.sh $pkgdir/etc/profile.d/go.sh
  echo export GOARCH=$GOARCH >> $pkgdir/etc/profile.d/go.sh
  install -Dm755 $srcdir/goinst $pkgdir/usr/bin/goinst
}
md5sums=('b7cd21a7a00922216036b3a992c60435'
         '304436b6ab490f98f0028c5ed4b82bbf')
