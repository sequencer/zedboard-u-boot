# Maintainer: Sequencer <liujiuyang1994@gmail.com>
pkgname=zedboard-u-boot
pkgver=2018.3
_srcname=u-boot-xlnx-xilinx-v${pkgver}
pkgrel=1
pkgdesc="u-boot bootloader for zedboard"
arch=(armv7h)
url=""
license=('GPLv2')
groups=(zedboard)
makedepends=('git' 'uboot-tools')
provides=()
conflicts=()
options=()
source=("https://github.com/Xilinx/u-boot-xlnx/archive/xilinx-v${pkgver}.zip"
        "zynq-zedboard.h"
        "ps7_init_gpl.c"
        "ps7_init_gpl.h"
        "config")
md5sums=("SKIP"
         "SKIP"
         "SKIP"
         "SKIP"
         "SKIP")
prepare() {
    cd "${srcdir}/${_srcname}"
    mkdir -p "${srcdir}/${_srcname}/board/xilinx/zynq/zynq-zedboard"
    cp ${srcdir}/config ./configs/zynq_zedboard_defconfig
    cp ${srcdir}/zynq-zedboard.h ./include/configs/zynq-zedboard.h
    # this should be copied from bsp
    cp ${srcdir}/ps7_init_gpl.h ./arch/arm/include/asm/arch-armv7/ps7_init_gpl.h
    cp ${srcdir}/ps7_init_gpl.h ./board/xilinx/zynq/zynq-zedboard/ps7_init_gpl.h
    cp ${srcdir}/ps7_init_gpl.c ./board/xilinx/zynq/zynq-zedboard/ps7_init_gpl.c
    # TODO: bsp device tree exist problem
    cp ./arch/arm/dts/zynq-zed.dts ./arch/arm/dts/zynq-zedboard.dts
    make zynq_zedboard_defconfig
    sed -i '1s/python$/python2/' tools/dtoc/dtoc
}

build() {
    cd "${srcdir}/${_srcname}"
    make PYTHON=python2
    mkimage -A arm -T zynqimage -d ./spl/u-boot-spl.bin BOOT.bin
}

package() {
    mkdir ${pkgdir}/boot
    install ${srcdir}/${_srcname}/u-boot.img ${pkgdir}/boot/u-boot.img
    install ${srcdir}/${_srcname}/BOOT.bin ${pkgdir}/boot/BOOT.bin
}
