# Maintainer: Taijian <taijian@posteo.de>
# Contributors: Patrick Burroughs (Celti), Abbradar, Zephyr, Christian Autermann, Biginoz, Martin Lee, Ricardo Funke,
#               PirateJonno, lh, Cilyan Olowen, Shaffer, Brcha, Lyle Putnam, Det, Boohbah,
#               Lara Maia, Padfoot, Jorge Barroso, carstene1ns, Sebastian Lau

pkgname=plymouth-git
pkgver=0.9.5.r84.ge554475
pkgrel=2
pkgdesc="A graphical boot splash screen with kernel mode-setting support (Development version)"
url="https://www.freedesktop.org/wiki/Software/Plymouth/"
arch=('i686' 'x86_64')
license=('GPL')

depends=('libdrm' 'pango' 'systemd')
makedepends=('git' 'docbook-xsl')
optdepends=('ttf-dejavu: For true type font support'
        'xf86-video-fbdev: Support special graphic cards on early startup'
        'cantarell-fonts: True Type support for BGRT theme')
provides=('plymouth')
conflicts=('plymouth' 'plymouth-legacy' 'plymouth-nosystemd')
backup=('etc/plymouth/plymouthd.conf')

options=('!libtool' '!emptydirs')

source=("git+https://gitlab.freedesktop.org/plymouth/plymouth.git"
       'arch-logo.png'
       'plymouth.encrypt_hook'
       'plymouth.encrypt_install'
       'lxdm-plymouth.service'
       'lightdm-plymouth.service'
       'sddm-plymouth.service'
       'plymouth-deactivate.service' # needed for sddm
#       'plymouth-start.service.in.patch'
#       'plymouth-start.path'
       'plymouth.initcpio_hook'
       'plymouth.initcpio_install'
       'sd-plymouth.initcpio_install'
       'plymouth-quit.service.in.patch'
       'plymouth-update-initrd.patch'
       'plymouthd.conf.patch'
)

sha256sums=('SKIP'
            'de4369ad5a5511b684305e3a882c2c56204696514ea8ccdb556dd656eca062e7'
            '7afa97d21444cbac7a6213edda09d9fa73ecbef1a6cea1e745f56669760c6120'
            '373ec20fe4c47e693a0c45cc06dd906e35dd1d70a85546bd1d571391de11763a'
            '06b31999cf60f49e536c7a12bc1c4f75f2671feb848bf5ccb91a963147e2680d'
            '86d0230d9393c9d83eb7bb430e6b0fb5e3f32e78fcd30f3ecd4e6f3c30b18f71'
            'c39f526f7e99173bc8f012900f53257537a25e2d8c19e23df630f1fe9a7627ba'
            '3b17ed58b59a4b60d904c60bba52bae7ad685aa8273f6ceaae08a15870c0a9eb'
            '2a80e2cad8de428358647677afa166219589d3338c5f94838146c804a29e2769'
            'dedca6cc37d1408bac593b76dfe6049a3063080be5ec6ab7da25832ceadc5fa7'
            '1b91da56da743eda199b6ddfcaf60e4de5fe05d930f9e8711407fe06010304ea'
            'dec28b86ddea93704f8479d33e08f81cd7ff4ccaad57e9053c23bd046db2278a'
            '74908ba59cea53c6a9ab67bb6dec1de1616f3851a0fd89bb3c157a1c54e6633a'
            '71d34351b4313da01e1ceeb082d9776599974ce143c87e93f0a465f342a74fd2')

pkgver() {
  cd plymouth
  git describe --long | sed 's/-/.r/;s/-/./'
}

prepare() {
	cd plymouth
	patch -p1 -i $srcdir/plymouth-update-initrd.patch
	patch -p1 -i $srcdir/plymouth-quit.service.in.patch
# Does not actually seem needed???	
#	patch -p1 -i $srcdir/plymouth-start.service.in.patch
	patch -p1 -i $srcdir/plymouthd.conf.patch
}

build() {
	cd plymouth

	LDFLAGS="$LDFLAGS -ludev" ./autogen.sh \
		--prefix=/usr \
		--exec-prefix=/usr \
		--sysconfdir=/etc \
		--localstatedir=/var \
		--libdir=/usr/lib \
		--libexecdir=/usr/lib \
		--sbindir=/usr/bin \
		--enable-systemd-integration \
		--enable-drm \
		--enable-tracing \
		--enable-pango \
		--enable-gtk=no \
		--with-release-file=/etc/os-release \
		--with-logo=/usr/share/plymouth/arch-logo.png \
		--with-background-color=0x000000 \
		--with-background-start-color-stop=0x000000 \
		--with-background-end-color-stop=0x4D4D4D \
		--without-rhgb-compat-link \
		--without-system-root-install \
		--with-runtimedir=/run

	make
}

package() {
  cd plymouth

	make DESTDIR="$pkgdir" install

	install -Dm644 "$srcdir/arch-logo.png" "$pkgdir/usr/share/plymouth/arch-logo.png"

	install -Dm644 "$srcdir/plymouth.encrypt_hook" "$pkgdir/usr/lib/initcpio/hooks/plymouth-encrypt"
	install -Dm644 "$srcdir/plymouth.encrypt_install" "$pkgdir/usr/lib/initcpio/install/plymouth-encrypt"
	install -Dm644 "$srcdir/plymouth.initcpio_hook" "$pkgdir/usr/lib/initcpio/hooks/plymouth"
	install -Dm644 "$srcdir/plymouth.initcpio_install" "$pkgdir/usr/lib/initcpio/install/plymouth"
	install -Dm644 "$srcdir/sd-plymouth.initcpio_install" "$pkgdir/usr/lib/initcpio/install/sd-plymouth"

	for i in {sddm,lxdm,lightdm}-plymouth.service; do
		install -Dm644 "$srcdir/$i" "$pkgdir/usr/lib/systemd/system/$i"
	done
	
	ln -s "/usr/lib/systemd/system/gdm.service" "$pkgdir/usr/lib/systemd/system/gdm-plymouth.service"

	install -Dm644 "$srcdir/plymouth-deactivate.service" 	"$pkgdir/usr/lib/systemd/system/plymouth-deactivate.service"
#	install -Dm644 "$srcdir/plymouth-start.path" 	"$pkgdir/usr/lib/systemd/system/plymouth-start.path"
	install -Dm644 "$pkgdir/usr/share/plymouth/plymouthd.defaults" "$pkgdir/etc/plymouth/plymouthd.conf"
}
