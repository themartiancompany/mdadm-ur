# Maintainer: Tobias Powalowski <tpowa@archlinux.org>
# Contributor: Judd Vinet <jvinet@zeroflux.org>
pkgname=mdadm
pkgver=3.2.2
pkgrel=3
pkgdesc="A tool for managing/monitoring Linux md device arrays, also known as Software RAID"
arch=(i686 x86_64)
license=('GPL')
url="http://www.cse.unsw.edu.au/~neilb/source/mdadm/"
groups=('base')
conflicts=('mkinitcpio<0.7')
depends=('glibc')
backup=('etc/mdadm.conf')
source=(ftp://ftp.kernel.org/pub/linux/utils/raid/mdadm/mdadm-$pkgver.tar.bz2
        mdadm 
        mdadm.conf 
        mdadm_install
        mdadm_hook
        disable-werror.patch
        linux-3.0.patch)
install=mdadm.install
replaces=('raidtools')
md5sums=('12ee2fbf3beddb60601fb7a4c4905651'
         '6df172c8f77b280018cf87eb3d313f29'
         '00cbed931db4f15b6ce49e3e7d433966'
         '4bb000166fb13e82ceaa2422fdfaac7e'
         '36f7cc564ed3267888d90208e0eb7adc'
         '4ad87b74a4bc9a34621280abe0e0c3e4'
         'c499b3edbf2c400c8a1984e18c7ce7fa')

build() {
  cd $srcdir/$pkgname-$pkgver
  patch -Np0 -i ../disable-werror.patch
  patch -Np1 -i ../linux-3.0.patch
  make CXiFLAGS="$CFLAGS"
}

package() {
  cd $srcdir/$pkgname-$pkgver
  make INSTALL=/bin/install DESTDIR=$pkgdir install
  install -D -m644 ../mdadm.conf $pkgdir/etc/mdadm.conf
  install -D -m755 ../mdadm $pkgdir/etc/rc.d/mdadm
  install -D -m644 ../mdadm_install $pkgdir/lib/initcpio/install/mdadm
  install -D -m644 ../mdadm_hook $pkgdir/lib/initcpio/hooks/mdadm
  # symlink for backward compatibility
  ln -sf /lib/initcpio/hooks/mdadm  $pkgdir/lib/initcpio/hooks/raid
}
