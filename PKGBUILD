# Maintainer: Christian Himpel <chressie at gmail dot com>
# Former maintainer: Ashok Gautham <ScriptDevil at gmail dot com>

pkgname=go-lang-hg
pkgver=4442
pkgrel=1
pkgdesc="Expressive, concurrent, garbage-collected systems programming language"
arch=(i686 x86_64)
url="http://golang.org/"
license=(BSD)
depends=()
makedepends=(bison ed mercurial)
options=(!strip)
md5sums=()

_hgroot="https://go.googlecode.com/hg"
_hgrepo="go"

build(){
    # Create local variables
    local _goarch _goroot _gobin _goos
    local _targetdir _licensedir _profiledir _srcdir

    # Setting build environment
    [ "$CARCH" = i686 ] && _goarch=386
    [ "$CARCH" = x86_64 ] && _goarch=amd64
    if [ -z "$_goarch" ]; then
        echo "Cannot determine CPU Architecture"
        return 1
    fi
    _goroot="$srcdir/$_hgrepo"
    _gobin="$_goroot/bin"
    _goos=linux
    mkdir -p "$_gobin"

    # Start build with correct environment
    cd "$_goroot/src"
    LC_ALL=C \
    PATH="$_gobin:$PATH" \
    GOROOT="$_goroot" \
    GOARCH=$_goarch \
    GOOS=linux \
    GOBIN="$_goroot/bin" \
    ./make.bash || return 1

    # Install package contents
    _targetdir=/opt/$pkgname
    _licensedir="$pkgdir/usr/share/licenses/$pkgname"
    _profiledir="$pkgdir/etc/profile.d"
    _srcdir="$pkgdir/$_targetdir/src"
    mkdir -p "$pkgdir/$_targetdir" "$_licensedir" "$_profiledir" "$_srcdir"
    cp -a "$_goroot/"{bin,doc,include,lib,misc,pkg} "$pkgdir/$_targetdir"
    cp "$_goroot/LICENSE" "$_licensedir"
    cat > "$_profiledir/go-lang.sh" << EOF
export GOROOT=$_targetdir
export GOBIN=\$GOROOT/bin
export GOOS=$_goos
export GOARCH=$_goarch
export PATH=\$PATH:\$GOBIN
EOF
    chmod +x "$_profiledir/go-lang.sh"
    install -m644 "$_goroot/src/Make."{$_goarch,cmd,pkg,conf} "$_srcdir"
    find "$pkgdir" -name ~place-holder~ -print0 | xargs -0 rm

    # Fix permissions in case they are messed up
    chmod -R u=rwX,g=rX,o=rX "$pkgdir"
}

# vim:et:sw=4:ts=4
