# Maintainer: Judd Vinet <jvinet@zeroflux.org>
pkgname=mdadm
pkgver=3.1.1
pkgrel=1
pkgdesc="A tool for managing/monitoring Linux md device arrays, also known as Software RAID"
arch=(i686 x86_64)
license=('GPL')
url="http://www.cse.unsw.edu.au/~neilb/source/mdadm/"
groups=('base')
depends=('glibc')
backup=('etc/mdadm.conf')
source=(ftp://ftp.kernel.org/pub/linux/utils/raid/mdadm/mdadm-$pkgver.tar.bz2
        mdadm 
        mdadm.conf 
        mdadm_install
        mdadm_hook)
install=mdadm.install
replaces=('raidtools')
options=('force')

build() {
  cd $srcdir/$pkgname-$pkgver
  make || return 1
  make INSTALL=/bin/install DESTDIR=$pkgdir install
  install -D -m644 ../mdadm.conf $pkgdir/etc/mdadm.conf
  install -D -m755 ../mdadm $pkgdir/etc/rc.d/mdadm
  install -D -m644 ../mdadm_install $pkgdir/lib/initcpio/install/mdadm
  install -D -m644 ../mdadm_hook $pkgdir/lib/initcpio/hooks/mdadm
  # build static mdassemble for Arch's initramfs
  make mdassemble.auto
  install -D -m755 mdassemble.auto $pkgdir/sbin/mdassemble.static
}
md5sums=('4fd8e375a2ee314becd3196c1a250d98'
         '6df172c8f77b280018cf87eb3d313f29'
         '00cbed931db4f15b6ce49e3e7d433966'
         '5067783b0051dedc95d159af22f0c681'
         'ef76d9dda597ffca4ef1934fe801cb60')
