# Maintainer: Achilleas Michailidis <achmichail@gmail.com>

pkgname=ihuos-keyring
pkgver=20221004
pkgrel=1
pkgdesc='IHU OS keyring'
arch=('any')
url='https://cs.ihu.gr'
license=('GPL')

source=('ihuos.gpg' 'ihuos-trusted' 'ihuos-revoked')
sha256sums=('6eed694545f9d20abfe71931dc2438eb6e0d90a178c36c31022bbb8ae5d7787f'
            '1a4dffede6d3b2bd25071ce4a3df48679451676589c309899cb6b3bc8bb5d4fd'
            'e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855')

package(){
	cd "$srcdir"
	install -dm755 "$pkgdir/usr/share/pacman/keyrings/"
	install -m0644 ihuos{.gpg,-trusted,-revoked} "$pkgdir/usr/share/pacman/keyrings/"
}
