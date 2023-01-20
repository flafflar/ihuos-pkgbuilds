# Maintainer: Achilleas Michailidis <achmichail@gmail.com>
# Contributor: Gabriel Souza Franco <Z2FicmllbGZyYW5jb3NvdXphQGdtYWlsLmNvbQ==>
# Contributor: Nico Rumpeltin <$forename at $surname dot de>
# Contributor: Matthias Blaicher <matthias at blaicher dot com>
# Contributor: Danny Dutton <duttondj@vt.edu>
#
# NOTE: If you plan on using the usbblaster make sure you are member of the plugdev group.
#
pkgname=quartus-lite
# Keep dot in _patchver
_mainver=20.1; _patchver=.1; _buildver=720
pkgver=${_mainver}${_patchver}.${_buildver}
pkgrel=1
pkgdesc="Quartus Prime Lite design software for Intel FPGAs"
arch=('x86_64')
url="http://fpgasoftware.intel.com/?edition=lite"
license=('custom')

_alteradir="/opt/intelFPGA/${_mainver}"

# According to the installer script, these dependencies are needed for the installer
depends=('ld-lsb' 'lib32-expat' 'lib32-fontconfig' 'lib32-freetype2' 'lib32-glibc'
         'lib32-gtk2' 'lib32-libcanberra' 'lib32-libpng' 'lib32-libice' 'lib32-libsm'
         'lib32-util-linux' 'lib32-ncurses' 'lib32-ncurses5-compat-libs' 'lib32-zlib'
         'lib32-libx11' 'lib32-libxau' 'lib32-libxdmcp' 'lib32-libxext' 'lib32-libxft'
         'lib32-libxrender' 'lib32-libxt' 'lib32-libxtst' 
         'libxcrypt-compat')
optdepends=("eclipse: For Nios II EDS")

_base_url="https://download.altera.com/akdlm/software/acdsinst"
source=("${_base_url}/${_mainver}std${_patchver/.0/}/${_buildver}/ib_installers/QuartusLiteSetup-${pkgver}-linux.run"
        'quartus.sh' 'quartus.desktop' '51-usbblaster.rules')
sha1sums=('49012e487889041962575c1cde81863457c105f5'
          '43429b170804b3960794f8c8c47bc183c8c8a14d'
          '2c7a5e22fc35eb67b0d5112a42642de341facb79'
          '66c4548944467103198f37be6ad2d8dd826020a0')

options=(!strip !debug) # Stripping will takes ages, I'd avoid it
PKGEXT=".pkg.tar.zst" # ZSTD is fast enough for compression

prepare() {
    echo "Notice: Requires around 12GB of free space during package building!"
    echo "Notice: The package files also requires around 3GB of free space"

    chmod +x QuartusLiteSetup-${pkgver}-linux.run
}

package() {
    DISPLAY="" ./QuartusLiteSetup-${pkgver}-linux.run \
        --disable-components quartus_help,devinfo,modelsim_ase,modelsim_ae \
        --mode unattended \
        --unattendedmodeui none \
        --accept_eula 1 \
        --installdir "${pkgdir}${_alteradir}"

    # Remove uninstaller and install logs since we have a working package management
    rm -r "${pkgdir}${_alteradir}/uninstall"
    rm -r "${pkgdir}${_alteradir}/logs"

    # Remove useless unzip binaries
    find "${pkgdir}${_alteradir}" \( -name "unzip" -or -name "unzip32" \) -delete

    # Remove duplicated file from help
    rm -r "${pkgdir}${_alteradir}/quartus/common/help/webhelp"

    # Fix missing permissions
    find "${pkgdir}${_alteradir}" \! -perm /o+rwx -exec chmod o=g {} \;

    # Replace altera directory in integration files
    sed -i "s,_alteradir,${_alteradir},g" quartus.sh
    sed -i "s,_alteradir,${_alteradir},g" quartus.desktop

    # Remove pkgdir reference in sopc_builder
    sed -i "s,${pkgdir},,g" "${pkgdir}${_alteradir}/quartus/sopc_builder/.sopc_builder"

    # Fix world writable permissions
    find "${pkgdir}${_alteradir}/nios2eds/documents" -perm -o+w -exec chmod go-w {} \+
    find "${pkgdir}${_alteradir}/quartus/common/tcl" -perm -o+w -exec chmod go-w {} \+
    find "${pkgdir}${_alteradir}/quartus/linux64" -perm -o+w -exec chmod go-w {} \+
    find "${pkgdir}${_alteradir}/quartus/sopc_builder/bin/europa" -perm -o+w -exec chmod go-w {} \+

    # Copy license file
    install -D -m644 "${pkgdir}${_alteradir}/licenses/license.txt" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

    # Install integration files
    install -D -m755 quartus.sh "${pkgdir}/etc/profile.d/quartus.sh"
    install -D -m644 51-usbblaster.rules "${pkgdir}/etc/udev/rules.d/51-usbblaster.rules"
    install -D -m644 quartus.desktop "${pkgdir}/usr/share/applications/quartus.desktop"
}
