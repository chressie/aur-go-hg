# Contributer: Christian Himpel <chressie at gmail dot com>
# Former contributer: Ashok Gautham <ScriptDevil@gmail.com>

pkgname=go-lang-hg
pkgver=4225
pkgrel=1
pkgdesc="A New, Experimental, Concurrent, Garbage-Collected Systems Programming Language"
arch=(i686 x86_64)
url="http://golang.org/"
license=(BSD)
depends=()
makedepends=(mercurial bison ed time)
options=(!strip)
md5sums=()

_hgroot="https://go.googlecode.com/hg"
_hgrepo="go"

build(){
    # Setting build environment
    case $CARCH in
        x86_64)
            _goarch="amd64";;
        i686)
            _goarch="386";;
        *)
            echo "Cannot determine CPU Architecture"
            return 1
        esac
    _goroot=$srcdir/$_hgrepo
    _gobin=$_goroot/bin
    _goos=linux
    if [ -d $_goroot/.hg ]; then
        cd $_goroot
        # Remove all files not under version control
        hg status -i -u | cut -d' ' -f2 | xargs rm -rf
        hg pull
        hg update
    else
        hg clone $_hgroot $_goroot || return 1
    fi
    mkdir -p $_gobin
    cd $_goroot/src

    LC_ALL=C \
    PATH=$PATH:$_gobin \
    GOROOT=$_goroot \
    GOARCH=$_goarch \
    GOOS=linux \
    GOBIN=$_goroot/bin \
    ./all.bash || return 1

    _targetdir=/opt/$pkgname
    _licensedir=$pkgdir/usr/share/licenses/$pkgname
    _profiledir=$pkgdir/etc/profile.d

    mkdir -p $pkgdir/$_targetdir $_licensedir $_profiledir

    cp -dR $_goroot/{bin,doc,include,lib,misc,pkg} $pkgdir/$_targetdir
    cp $_goroot/LICENSE $_licensedir

    cat > $_profiledir/go-lang.sh << EOF
export GOROOT=$_targetdir
export GOBIN=\$GOROOT/bin
export GOOS=$_goos
export GOARCH=$_goarch
export PATH=\$PATH:\$GOBIN
EOF
    chmod +x $_profiledir/go-lang.sh
}
