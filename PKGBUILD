# Maintainer: Aleksander Boiko <brdcom@ya.ru>
pkgname=bgbilling60
_basename=bgbilling
_pkgname=BGBillingServer
_major=6.0
_minor=1793
pkgver=$_major.$_minor
pkgrel=3
pkgdesc="The billing system BGBilling, designed to be installed in parallel with another installation"
arch=('i686' 'x86_64') 
url="http://bgbilling.ru"
license=('custom')
depends=('mariadb' 'activemq' 'java-runtime>=6' 'pbzip2')
makedepends=('unzip' 'dos2unix' 'patch')
backup=("opt/${pkgname}/data/lic.properties")
source=("ftp://bgbilling.ru/pub/${_basename}/${_major}/data/${_pkgname}_${_major}_${_minor}.zip"
        'bgbilling60.conf'
        'bgbilling60.service'
        'bgdataloader60.service'
        'bgscheduler60.service'
        'setenv.sh.patch'
        'update.sh.patch')
install=bgbilling60.install
md5sums=('ee97649e53ca57629b18e1e5986ac44f'
         '19d65cde085416c52cb00b1249b65126'
         'a5a5ed34440e1a3f3fd8af8f4fdb0fa2'
         'dbbad996ba9076205f1930a7c41bd6b1'
         'a468f093f1bac2b12acdb5211bf03b5a'
         '0f7f773fb2e39637f1db752e439561e9'
         'fdb6481f8c548109a20e99cb7bc86395')

package() {
  install -d -m0755 ${pkgdir}/opt
  mv ./${_pkgname} ${pkgdir}/opt/${pkgname}

  for d in bgbilling60; do
    install -D -m 644 $pkgname.conf "$pkgdir/etc/conf.d/$d"
    backup+=("etc/conf.d/$d")
  done

  for d in bgbilling60 bgscheduler60 bgdataloader60; do
    install -D -m 644 $d.service "$pkgdir/usr/lib/systemd/system/$d.service"
  done
  
  install -D -m0644 ./dump.sql ${pkgdir}/usr/share/${pkgname}/dump.sql
  #install -D -m0644 ./lic.properties ${pkgdir}/usr/share/licenses/${pkgname}/test_license/lic.properties

# putting the appropriate access rights to the startup scripts
  cd ${pkgdir}/opt/${pkgname}
  chmod 0744 *.sh

# patch
  patch -p0 <"${srcdir}/setenv.sh.patch"
  patch -p0 <"${srcdir}/update.sh.patch"

# converting files to Unix format
  dos2unix *.sh

# remove win files
  rm -rf *.ini
  rm -rf *.bat
  rm -rf *.exe

# remove junk files
  rm -rf ./script
}

# vim:set ts=2 sw=2 ft=sh et:
