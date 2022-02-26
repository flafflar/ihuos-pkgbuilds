# Maintainer: Achilleas Michailidis <achmichail@gmail.com>

pkgname=ihuos-keyring
pkgver=20220226
pkgrel=1
pkgdesc='IHU OS keyring'
arch=('any')
url='https://cs.ihu.gr'
license=('GPL')

source=('ihuos.gpg' 'ihuos-trusted' 'ihuos-revoked')
sha256sums=('ab91830e27dc4418f9f76d481e802b403adb279c63909bbcc8a3cc7ba2ae8c2e'
            '1a4dffede6d3b2bd25071ce4a3df48679451676589c309899cb6b3bc8bb5d4fd'
            'e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855')

package(){
	cd "$srcdir"
	install -dm755 "$pkgdir/usr/share/pacman/keyrings/"
	install -m0644 ihuos{.gpg,-trusted,-revoked} "$pkgdir/usr/share/pacman/keyrings/"
}
