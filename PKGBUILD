# Maintainer: Achilleas Michailidis <achmichail@gmail.com>
# Contributor: Gabriel Souza Franco <Z2FicmllbGZyYW5jb3NvdXphQGdtYWlsLmNvbQ==>
# Contributor: Nico Rumpeltin <$forename at $surname dot de>
# Contributor: Matthias Blaicher <matthias at blaicher dot com>
# Contributor: Danny Dutton <duttondj@vt.edu>

pkgname=quartus-devinfo-max10
# Keep dot in _patchver
_mainver=20.1; _patchver=.1; _buildver=720
pkgver=${_mainver}${_patchver}.${_buildver}
pkgrel=1
pkgdesc="Quartus Prime Lite - devinfo files for max10"
arch=('x86_64')
url="http://fpgasoftware.intel.com/?edition=lite"
license=('custom')

options=(!strip !debug)

# According to the installer script, these dependencies are needed for the installer
depends=('ld-lsb' 'lib32-expat' 'lib32-fontconfig' 'lib32-freetype2' 'lib32-glibc'
         'lib32-gtk2' 'lib32-libcanberra' 'lib32-libpng' 'lib32-libice' 'lib32-libsm'
         'lib32-util-linux' 'lib32-ncurses' 'lib32-ncurses5-compat-libs' 'lib32-zlib'
         'lib32-libx11' 'lib32-libxau' 'lib32-libxdmcp' 'lib32-libxext' 'lib32-libxft'
         'lib32-libxrender' 'lib32-libxt' 'lib32-libxtst')
provides=(quartus-devinfo)

source=("https://download.altera.com/akdlm/software/acdsinst/${_mainver}std${_patchver/.0/}/${_buildver}/ib_installers/max10-${pkgver}.qdz")
noextract=("max10-${pkgver}.qdz") # Will extract directly to pkgdir
md5sums=('fea82df785421cd0c0bf75ca94790804')

_alteradir="/opt/intelFPGA/${_mainver}"

package() {
   install -d "${pkgdir}${_alteradir}"
   bsdtar -xf "max10-${pkgver}.qdz" -C "${pkgdir}${_alteradir}"
}
