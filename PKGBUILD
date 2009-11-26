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
            export GOARCH="amd64";;
        i686)
            export GOARCH="386";;
        *)
            echo "Cannot determine CPU Architecture"
            return 1
        esac
    export GOROOT=$srcdir/$_hgrepo
    export GOBIN=$GOROOT/bin
    export GOOS=linux
    if [ -d $GOROOT/.hg ]; then
        cd $GOROOT
        # Remove all files not under version control
        hg status -i -u | cut -d' ' -f2 | xargs rm -rf
        hg pull
        hg update
    else
        hg clone $_hgroot $GOROOT || return 1
    fi
    mkdir -p $GOBIN
    export PATH=$PATH:$GOBIN
    cd $GOROOT/src
    ./all.bash || return 1

    _targetdir=/opt/$pkgname
    _licensedir=$pkgdir/usr/share/licenses/$pkgname
    _profiledir=$pkgdir/etc/profile.d

    mkdir -p $pkgdir/$_targetdir $_licensedir $_profiledir

    cp -dR $GOROOT/{bin,doc,include,lib,misc,pkg} $pkgdir/$_targetdir
    cp $GOROOT/LICENSE $_licensedir

    cat > $_profiledir/go-lang.sh << EOF
export GOROOT=$_targetdir
export GOBIN=\$GOROOT/bin
export GOOS=$GOOS
export GOARCH=$GOARCH
export PATH=\$PATH:\$GOBIN
EOF
    chmod +x $_profiledir/go-lang.sh
}
