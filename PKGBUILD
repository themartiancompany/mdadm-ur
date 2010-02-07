# Maintainer: Tobias Powalowski <tpowa@archlinux.org>
# Contributor: Judd Vinet <jvinet@zeroflux.org>
pkgname=mdadm
pkgver=3.1.1
pkgrel=2
pkgdesc="A tool for managing/monitoring Linux md device arrays, also known as Software RAID"
arch=(i686 x86_64)
license=('GPL')
url="http://www.cse.unsw.edu.au/~neilb/source/mdadm/"
groups=('base')
conflicts=('mkinitcpio<0.5.99')
depends=('glibc')
backup=('etc/mdadm.conf')
source=(ftp://ftp.kernel.org/pub/linux/utils/raid/mdadm/mdadm-$pkgver.tar.bz2
        mdadm 
        mdadm.conf 
        mdadm_install
        mdadm_hook)
install=mdadm.install
replaces=('raidtools')
md5sums=('4fd8e375a2ee314becd3196c1a250d98'
         '6df172c8f77b280018cf87eb3d313f29'
         '00cbed931db4f15b6ce49e3e7d433966'
         '0c201efd85790fea6aaf7686a9b31510'
         '1a3eb63832cecd6550f5b0a21d58cfdb')

build() {
  cd $srcdir/$pkgname-$pkgver
  make || return 1
  make INSTALL=/bin/install DESTDIR=$pkgdir install
  install -D -m644 ../mdadm.conf $pkgdir/etc/mdadm.conf
  install -D -m755 ../mdadm $pkgdir/etc/rc.d/mdadm
  install -D -m644 ../mdadm_install $pkgdir/lib/initcpio/install/mdadm
  install -D -m644 ../mdadm_hook $pkgdir/lib/initcpio/hooks/mdadm
  # symlink for backward compatibility
  ln -sf /lib/initcpio/hooks/mdadm  $pkgdir/lib/initcpio/hooks/raid
  # build static mdassemble for Arch's initramfs
  make MDASSEMBLE_AUTO=1 mdassemble
  install -D -m755 mdassemble $pkgdir/sbin/mdassemble
}

