# Maintainer: Gesh <gesh@gesh.uni.cx>
# based on pandoc-sile-git, by
# Contributor: Caleb Maclennan <caleb@alerque.com>
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: Arch Haskell Team <arch-haskell@haskell.org>

shopt -s extglob

pkgname=pandoc-static
_pkgname="${pkgname%-static}"
pkgver=2.19.2
pkgrel=1
pkgdesc='Conversion between markup formats (static build, dynamic Lua support)'
url='https://pandoc.org'
license=('GPL')
arch=('x86_64')
optdepends=('texlive-core: for pdf output')
conflicts=('haskell-pandoc' 'pandoc' 'pandoc-bin')
replaces=('haskell-pandoc' 'pandoc' 'pandoc-bin')
provides=("pandoc")
makedepends=('stack>=1.7.0')
source=("https://hackage.haskell.org/packages/archive/${_pkgname}/${pkgver}/${_pkgname}-${pkgver}.tar.gz")
sha512sums=('3628a9193d5138294bae562726bcd94567eec10fa0053d43739af04d4eba0a53bd49c2c000a5360afcac08153960a9bf2ee4be3c419cec7e5c13273e718edc80')

prepare() {
    cd "$_pkgname-$pkgver"
    # TODO: find a better solution
    sed -i "s|let env' = dynlibEnv ++ |let env' = dynlibEnv ++ [(\"LD_LIBRARY_PATH\", \"$PWD/dist/build\")] ++ |" test/Tests/Command.hs
}

build() {
    cd "$_pkgname-$pkgver"

    stack setup
    stack build \
        --install-ghc \
        --ghc-options='-fdiagnostics-color=always' \
        --flag 'pandoc:embed_data_files' \
        --fast
}

package() {
    cd "$_pkgname-$pkgver"
    find ./ -path '*/dist/*' -type f -name pandoc -perm /u+x \
        -execdir install -Dm755 -t "$pkgdir/usr/bin/" {} \;
    install -Dm644 man/pandoc.1 "${pkgdir}"/usr/share/man/man1/pandoc.1
}
