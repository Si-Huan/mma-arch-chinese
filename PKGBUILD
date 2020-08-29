# Maintainer: Joshua Ellis <josh@jpellis.me>
# Contributor: Anish Tondwalkar <anish@tjhsst.edu>
# Contributor: Ghost91 <m_graeb11@cs.uni-kl.de>
# Contributor: Michael Pusterhofer <pusterhofer at student dot tugraz dot at>
# Contributor: Raphael Scholer <rscholer@gmx.de>
# Contributor: kjslag <kjslag at gmail dot com>
# Contributor: teratomata <teratomat@gmail.com>

pkgname=mathematica
pkgver=12.1.1
pkgrel=1
pkgdesc="A computational software program used in scientific, engineering, and mathematical fields and other areas of technical computing."
arch=('i686' 'x86_64')
url="http://www.wolfram.com/mathematica/"
license=('proprietary')
depends=(
    'openmp'
)
optdepends=(
    ## The following list of dependencies was inferred from namcap's output.  If
    ## you believe there is an error, please let me know.  Also feel free to
    ## contribute description to dependencies if you know what they do.
    'alsa-lib'
    'atk'
    'cairo'
    'ffmpeg'
    'fontconfig'
    'gdk-pixbuf2'
    'glib2'
    'glu'
    'gmime'
    'gmp'
    'gtk2'
    'harfbuzz'
    'intel-tbb'
    'java-environment'
    'java-runtime'
    'leptonica'
    'libbson'
    'libffi'
    'libmongoc'
    'libogg'
    'libpng12'
    'libselinux'
    'libsm'
    'libssh2'
    'libutil-linux'
    'libx11'
    'libxcomposite'
    'libxml2'
    'libxrandr'
    'libxslt'
    'libxss'
    'libxtst'
    'libxxf86vm'
    'mesa-demos: for improved graphics output'
    'ncurses'
    'nvidia-utils'
    'openssl-1.0'
    'pango'
    'pixman'
    'portaudio'
    'postgresql-libs'
    'python'
    'qt5-declarative'
    'qt5-multimedia'
    'qt5-webengine'
    'qt5-xmlpatterns'
    'r'
    'tesseract'
    'zlib'
)

## Edit by sihuan for Chinese edition
# source=("local://Mathematica_${pkgver}_LINUX.sh")
# md5sums=('e54fa11c905e2ed4a562b4743ebc0ebf')
source=("local://Mathematica_${pkgver}_Chinese_LINUX.sh")
# md5sums=('c6bcc60e55fd998e029c6985b6f3fe0c')
md5sums=('SKIP')
options=("!strip")

## To build this package you need to place the mathematica-installer into your
## startdir If you don't own the installer you can download a trial version at
## http://www.wolfram.com/mathematica/trial

## The documentation takes up the majority of the disk space.  If you do not wish
## to keep it, uncomment the relevant lines at the bottom of this PKGBUILD.

## The final package can be very large (especially if documentation is kept) and
## compression can be quite slow.  In most cases, the package is installed
## straight away and the package need not be kept, so compression is disabled.
PKGEXT='.pkg.tar'

prepare() {
    warning "Building Mathematica takes more than 20GiB of space for 'makepkg', and another 10GiB for the pkg tarball."
    warning "Building in a tmpfs (e.g. /tmp when mounted into RAM) may not work."

    if [ $(echo "${srcdir}" | wc -w) -ne 1 ]; then
        msg2 "ERROR: The Mathematica installer doesn't support directory names with spaces."
        msg2 "Current build directory: ${srcdir}"
        exit 1
    fi

    ## Edit by sihuan for Chinese edition
    # chmod +x ${srcdir}/Mathematica_${pkgver}_LINUX.sh
    chmod +x ${srcdir}/Mathematica_${pkgver}_Chinese_LINUX.sh
}

package() {
    msg2 "Running Mathematica installer"
    # https://reference.wolfram.com/language/tutorial/InstallingMathematica.html#650929293
    ## Edit by sihuan for Chinese edition
    # sh ${srcdir}/Mathematica_${pkgver}_LINUX.sh -- \
    sh ${srcdir}/Mathematica_${pkgver}_Chinese_LINUX.sh -- \
             -execdir=${pkgdir}/usr/bin \
             -targetdir=${pkgdir}/opt/Mathematica \
             -auto
    msg2 "Errors related to 'xdg-icon-resource' and 'xdg-desktop-menu' are to be expected during Mathematica's installation."

    msg2 "Fixing symbolic links"
    cd ${pkgdir}/opt/Mathematica/Executables
    rm wolframscript
    if [ "${CARCH}" = "x86_64" ]; then
        ln -s /opt/Mathematica/SystemFiles/Kernel/Binaries/Linux-x86-64/wolframscript
    else
        ln -s /opt/Mathematica/SystemFiles/Kernel/Binaries/Linux/wolframscript
    fi
    cd ${pkgdir}/usr/bin
    rm *
    ln -s /opt/Mathematica/Executables/math
    ln -s /opt/Mathematica/Executables/mathematica
    ln -s /opt/Mathematica/Executables/Mathematica
    ln -s /opt/Mathematica/Executables/MathKernel
    ln -s /opt/Mathematica/Executables/mcc
    ln -s /opt/Mathematica/Executables/wolfram
    ln -s /opt/Mathematica/Executables/WolframKernel
    if [ "${CARCH}" = "x86_64" ]; then
        ln -s /opt/Mathematica/SystemFiles/Kernel/Binaries/Linux-x86-64/ELProver
        ln -s /opt/Mathematica/SystemFiles/Kernel/Binaries/Linux-x86-64/wolframscript
    else
        ln -s /opt/Mathematica/SystemFiles/Kernel/Binaries/Linux/ELProver
        ln -s /opt/Mathematica/SystemFiles/Kernel/Binaries/Linux/wolframscript
    fi

    msg2 "Setting up WolframScript"
    mkdir -p ${srcdir}/WolframScript
    mkdir -p ${pkgdir}/usr/share/
    cd ${srcdir}/WolframScript
    ## Edit by sihuan for Chinese edition
    #  bsdtar -xf ${pkgdir}/opt/Mathematica/SystemFiles/Installation/wolframscript_1.4.0+2020061801_amd64.deb data.tar.xz
    bsdtar -xf ${pkgdir}/opt/Mathematica/SystemFiles/Installation/wolframscript_1.4.0+2020071501_amd64.deb data.tar.xz
    tar -xf data.tar.xz -C ${pkgdir}/usr/share/ --strip=3 ./usr/share/


    msg2 "Copying menu and mimetype information"
    mkdir -p \
          ${pkgdir}/usr/share/applications \
          ${pkgdir}/usr/share/desktop-directories \
          ${pkgdir}/usr/share/mime/packages
    cd ${pkgdir}/opt/Mathematica/SystemFiles/Installation
    desktopFile='wolfram-mathematica12.desktop'
    sed -Ei 's|^(\s*TryExec=).*|\1/usr/bin/Mathematica|g' $desktopFile
    sed -Ei 's|^(\s*Exec=).*|\1/usr/bin/Mathematica %F|g' $desktopFile
    printf 'Categories=Science;Math;NumericalAnalysis;DataVisualization;' >> $desktopFile
    printf 'StartupWMClass=Mathematica;' >> $desktopFile
    cp $desktopFile ${pkgdir}/usr/share/applications/
    cp Wolfram.directory ${pkgdir}/usr/share/desktop-directories/
    cp *.xml ${pkgdir}/usr/share/mime/packages/

    msg2 "Copying icons"
    mkdir -p ${pkgdir}/usr/share/icons/hicolor/{32x32,64x64,128x128}/{apps,mimetypes}
    cd ${pkgdir}/opt/Mathematica/SystemFiles/FrontEnd/SystemResources/X
    for i in 32 64 128; do
        cp App-${i}.png ${pkgdir}/usr/share/icons/hicolor/${i}x${i}/apps/wolfram-mathematica.png
        for mimetype in $(ls vnd.* | cut -d '-' -f1 | uniq); do
            cp ${mimetype}-${i}.png ${pkgdir}/usr/share/icons/hicolor/${i}x${i}/mimetypes/application-${mimetype}.png
        done
    done

    msg2 "Copying man pages"
    mkdir -p ${pkgdir}/usr/share/man/man1
    cd ${pkgdir}/opt/Mathematica/SystemFiles/SystemDocumentation/Unix
    cp *.1 ${pkgdir}/usr/share/man/man1

    msg2 "Fixing file permissions"
    chmod go-w -R ${pkgdir}/*

    if [ "${CARCH}" = "x86_64" ]; then
        msg2 "Removing files for i686"
        rm -rf \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/WSMCore/SystemModeler/SystemFiles/Activation/Linux" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/WSMCore/SystemModeler/SystemFiles/Libraries/Linux" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/RLink/SystemFiles/Libraries/Linux"
    else
        msg2 "Removing files for x86_64"
        rm -rf \
            "${pkgdir}/opt/Mathematica/AddOns/Applications/DocumentationSearch/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/AddOns/Applications/StandardOceanData/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Activation/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Autoload/PacletManager/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/Chemistry/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/Chemistry/Resources/OpenBabelLink/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/Compile/CompileResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/CUDADriverLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/DataStructure/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/GraphStore/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/HTTPHandling/Resources/Binaries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/LLVMLink/ExecutableResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/LLVMLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/LLVMLink/LLVMResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/MachineLearning/Resources/Binaries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/MXNetLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/NumericArrayUtilities/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/ONNXUtilities/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/PredictiveInterface/Kernel/Predictions.mx/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/PredictiveInterface/Kernel/PredictiveInterfaceCode.mx/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/SemanticImport/Binaries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/SpellCorrect/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/TextSearch/Binaries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/WebUnit/Resources/DriverBinaries/ChromeDriver/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/WebUnit/Resources/DriverBinaries/GeckoDriver/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/WolframNTL/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/WSMCore/SystemModeler/Mathematica/WSMLink/SystemFiles/Libraries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/WSMCore/SystemModeler/SystemFiles/Activation/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/WSMCore/SystemModeler/SystemFiles/Libraries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Components/WSMCore/SystemModeler/SystemFiles/WSM/Binaries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Converters/Binaries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/FrontEnd/Binaries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Java/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Kernel/Binaries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Kernel/SystemResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Libraries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/ArchiveTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/AudioFileStreamTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/AudioTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/CalendarTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/CloudObject/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/CURLLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/DAALLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/Databases/Python/executables/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/DICOMTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/DTWTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/FDLLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/FFmpegTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/GeometryTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/GIFTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/GPUTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/HDF5Tools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/HTTPLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/ImageFileTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/IMAQTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/IPOPTLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/ITKLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/JLink/Kernel/SystemResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/JLink/SystemFiles/Libraries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/JSONTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/KeychainLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/LibraryLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/LightGBMLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/MathLink/DeveloperKit/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/MIDITools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/MIMETools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/MongoLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/MP3Tools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/MQTTLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/MQTTLink/Resources/Binaries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/OpenCascadeLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/OpenCLLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/OpenCVLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/OpenSURF/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/ProcessLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/ProtobufLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/RAWTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/RLink/SystemFiles/Libraries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/SCSLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/SDPLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/SecureShellLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/SerialLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/SocketLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/SoundFileTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/SpeechSynthesisTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/SpeechVocoderTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/StreamLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/SVTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/SystemTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/TesseractTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/TetGenLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/TinkerForgeWeatherStationTools/Binaries/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/TINSLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/TriangleLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/UUID/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/VernierLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/WebpTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/WebRTCLink/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/WSTP/DeveloperKit/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/XLTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/XMPTools/LibraryResources/Linux-x86-64" \
            "${pkgdir}/opt/Mathematica/SystemFiles/Links/ZeroMQLink/LibraryResources/Linux-x86-64"
    fi

    ## The documentation takes up the majority of the disk space (6.8G+).  If you
    ## do not wish to have the documentation installed, uncomment the following
    ## lines.
    # msg2 "Removing documentation"
    # rm -rf "${pkgdir}/opt/Mathematica/Documentation"
}

# Local Variables:
# mode: sh
# End:
