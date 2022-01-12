pkgname=ihuos-calamares
pkgver=3.2.49.1
pkgrel=1
pkgdesc="Installer for IHU-OS, based on Calamares"
arch=("x86_64")
url="https://calamares.io"
license=("GPL")
depends=("boost-libs>=1.55" "qt5-base>=5.9" "kcoreaddons" "kpmcore>=3.3" "kio" "yaml-cpp>=0.5.1" "python>=3.3" "ckbcomp")
makedepends=("cmake>=3.3" "extra-cmake-modules>=5.18" "boost>=1.55" "qt5-tools" "git")
checkdepends=("python-toml")
optdepends=()
backup=()
source=("git+https://github.com/flafflar/calamares.git#branch=pacstrap"
	"git+https://github.com/flafflar/ihuos-calamares-config.git#commit=91c1f75929639576eac37a4b4bc5401c9d35ac5d"
)
sha256sums=("SKIP" "SKIP")

prepare() {
	cd calamares
	mkdir -p build
}

build() {
	cd calamares/build
	cmake -DCMAKE_BUILD_TYPE=None -DCMAKE_INSTALL_PREFIX=/usr -Wno-dev ..
	make
}

check() {
	cd calamares/build
	ctest --output-on-failure
}

package() {
	cd calamares/build
	make DESTDIR="$pkgdir/" install

	cd ../../ihuos-calamares-config

	install -Dm644 settings.conf -t "$pkgdir/usr/share/calamares/"

	install -Dm644 fstab.conf -t "$pkgdir/usr/share/calamares/modules/"
	install -Dm644 initcpio.conf -t "$pkgdir/usr/share/calamares/modules/"
	install -Dm644 locale.conf -t "$pkgdir/usr/share/calamares/modules/"
	install -Dm644 pacstrap.conf -t "$pkgdir/usr/share/calamares/modules/"
	install -Dm644 partition.conf -t "$pkgdir/usr/share/calamares/modules/"
	install -Dm644 services.conf -t "$pkgdir/usr/share/calamares/modules/"
	install -Dm644 users.conf -t "$pkgdir/usr/share/calamares/modules/"
}
