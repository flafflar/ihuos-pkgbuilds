# Maintainer: Achilleas Michailidis <achmichail@gmail.com>
# Contributor: Gabriel Souza Franco <Z2FicmllbGZyYW5jb3NvdXphQGdtYWlsLmNvbQ==>
# Contributor: Nico Rumpeltin <$forename at $surname dot de>
# Contributor: Matthias Blaicher <matthias at blaicher dot com>
# Contributor: Danny Dutton <duttondj@vt.edu>

pkgname=quartus-help
# Keep dot in _patchver
_mainver=20.1; _patchver=.1; _buildver=720
# Latest HLS compiler was only released with Pro numbering
# _promain=20.3; _propatch=.0; _probuild=158; _prover=${_promain}${_propatch}.${_probuild}
pkgver=${_mainver}${_patchver}.${_buildver}
pkgrel=1
pkgdesc="Quartus Prime Lite - help files"
arch=('x86_64')
url="http://fpgasoftware.intel.com/?edition=lite"
license=('custom')
depends=()

_inteldir="/opt/intelFPGA/${_mainver}"

_base_url="https://download.altera.com/akdlm/software/acdsinst"
source=("${_base_url}/${_mainver}std${_patchver/.0/}/${_buildver}/ib_installers/QuartusHelpSetup-${pkgver}-linux.run")
sha256sums=('17e1d546dcfae51ff079ec9b1225993608c192fe7c89b67788a9fed5029ac1c3')

options=('!strip' '!debug') # Stripping will takes ages, I'd avoid it
PKGEXT=".pkg.tar.zst" # ZSTD is fast enough for compression

prepare() {
    echo "Notice: Requires around 1GB of free space during package building!"
    echo "Notice: The package files also requires around 0.5GB of free space"

    chmod +x QuartusHelpSetup-${pkgver}-linux.run
}

package() {
    DISPLAY="" ./QuartusHelpSetup-${pkgver}-linux.run --mode unattended --unattendedmodeui none --accept_eula 1 --installdir "${pkgdir}${_inteldir}"

    # Remove uninstaller and install logs since we have a working package management
    rm -r "${pkgdir}${_inteldir}/"{uninstall,logs}

    # Link license file
    install -d -m755 "${pkgdir}/usr/share/licenses/${pkgname}"
    ln -s "${_inteldir}/licenses/license.txt" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
