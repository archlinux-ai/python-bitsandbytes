# Maintainer: Daniel Bershatsky <bepshatsky@yandex.ru>
_pkgname=bitsandbytes
pkgbase=python-${_pkgname}
pkgname=("$pkgbase" "${pkgbase}-cuda")
pkgver=0.41.0
pkgrel=1
_pkgdesc='8-bit CUDA functions for PyTorch'
pkgdesc="${_pkgdesc}"
arch=('x86_64')
url='https://github.com/TimDettmers/bitsandbytes'
license=('MIT')
groups=()
depends=('python-numpy' 'python-scipy')
makedepends=('cuda' 'python-build' 'python-installer' 'python-setuptools'
             'python-wheel')
checkdepends=('python-pytest')
optdepends=()
source=("$_pkgname-$pkgver.tar.gz::$url/archive/refs/tags/$pkgver.tar.gz")
sha256sums=('79f24d75d607c30040ed78b13fc621a488f57462e86aed5667e049f8b346642c')

build() {
    cd $srcdir/$_pkgname-$pkgver

    CUDA_VERSION= make cpuonly
    python -m build -nw -o dist/cpuonly

    CUDA_VERSION=123 make cuda12x
    python -m build -nw -o dist/cuda12x
}

_package() {
    local dist_dir=$1
    python -m installer \
        --compile-bytecode 1 \
        --destdir $pkgdir\
        $dist_dir/$_pkgname-$pkgver-*-*.whl

    install -Dm644 \
        "$srcdir/$_pkgname-$pkgver/LICENSE" \
        "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

package_python-bitsandbytes() {
    pkgdesc="${_pkgdesc}"
    depends+=('python-pytorch')
    conflicts=($pkgname)
    provides=($pkgname=${pkgver})
    _package $srcdir/$_pkgname-$pkgver/dist/cpuonly
}

package_python-bitsandbytes-cuda() {
    pkgdesc="${_pkgdesc} (with CUDA)"
    depends+=('python-pytorch-cuda')
    conflicts=($pkgname)
    provides=($pkgname=${pkgver})
    _package $srcdir/$_pkgname-$pkgver/dist/cuda12x

    # Remove CPU-only target extension.
    find $pkgdir/usr/lib -name libbitsandbytes_cpu.so -delete
}
