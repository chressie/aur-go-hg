# Contributer: Christian Himpel <chressie at gmail dot com>
# Former contributer: Ashok Gautham <ScriptDevil at gmail dot com>

pkgname=go-lang-hg
pkgver=4225
pkgrel=1
pkgdesc="A New, Experimental, Concurrent, Garbage-Collected Systems Programming Language"
arch=(i686 x86_64)
url="http://golang.org/"
license=(BSD)
depends=()
makedepends=(bison ed mercurial time)
options=(!strip)
md5sums=()

_hgroot="https://go.googlecode.com/hg"
_hgrepo="go"

build(){
    # Setting build environment
    [ "$CARCH" = i686 ] && _goarch=386
    [ "$CARCH" = x86_64 ] && _goarch=amd64
    if [ -z "$_goarch" ]; then
        echo "Cannot determine CPU Architecture"
        return 1
    fi
    _goroot=$srcdir/$_hgrepo
    _gobin=$_goroot/bin
    _goos=linux
    mkdir -p $_gobin

    # Keep source tree as is, if --noextract is given
    if [ "$NOEXTRACT" -ne 1 ]; then
        if [ -d $_goroot/.hg ]; then
            # Remove all files not under version control
            hg --cwd $_goroot status -i -u | cut -d' ' -f2 | xargs rm -rf
            hg --cwd $_goroot pull
            hg --cwd $_goroot update -C -r $pkgver || return 1
        else
            hg clone $_hgroot $_goroot || return 1
            hg --cwd $_goroot update -C -r $pkgver || return 1
        fi
    fi

    # Start build with correct environment
    cd $_goroot/src
    LC_ALL=C \
    PATH=$_gobin:$PATH \
    GOROOT=$_goroot \
    GOARCH=$_goarch \
    GOOS=linux \
    GOBIN=$_goroot/bin \
    ./all.bash || return 1

    # Install package contents
    _targetdir=/opt/$pkgname
    _licensedir=$pkgdir/usr/share/licenses/$pkgname
    _profiledir=$pkgdir/etc/profile.d
    mkdir -p $pkgdir/$_targetdir $_licensedir $_profiledir
    cp -a $_goroot/{bin,doc,include,lib,misc,pkg} $pkgdir/$_targetdir
    cp $_goroot/LICENSE $_licensedir
    cat > $_profiledir/go-lang.sh << EOF
export GOROOT=$_targetdir
export GOBIN=\$GOROOT/bin
export GOOS=$_goos
export GOARCH=$_goarch
export PATH=\$PATH:\$GOBIN
EOF
    chmod +x $_profiledir/go-lang.sh

    # Fix permissions in case they are messed up
    chmod -R u=rwX,g=rX,o=rX $pkgdir
}

# vim:et:sw=4:ts=4
