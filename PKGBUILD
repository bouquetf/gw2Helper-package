# Maintainer: bouquetf <bouquet.frederic@gmail.com>
pkgname=next
pkgver=VERSION
pkgrel=1
depends=(tomcat7)
arch=('any')
pkgdesc="GW2 Helper webapp"
license=(GPL)
makedepends=(unzip)

backup=(etc/$pkgname/server.xml
        etc/$pkgname/web.xml
        etc/$pkgname/context.xml
       )

source=($pkgname-$pkgver.war
        web.xml
        server.xml
        context.xml
        systemd_next.service
       	systemd_tmpfiles.d_next.conf
)

noextract=($pkgname-$pkgver.war)

package() {
  #log
  install -dm775 -o 71 -g 19 ${pkgdir}/var/log/${pkgname}
  install -dm775 -o 71 -g 19 ${pkgdir}/usr/share/${pkgname}
  ln -s /var/log/${pkgname} ${pkgdir}/usr/share/${pkgname}/logs
  touch ${pkgdir}/var/log/${pkgname}/catalina.{out,err}
  chown 71:19 ${pkgdir}/var/log/${pkgname}/catalina.{out,err}

  #webapp
  install -dm775 ${pkgdir}/var/lib/${pkgname}
  install -dm775 ${pkgdir}/var/lib/${pkgname}/webapps
  cp $srcdir/$pkgname-$pkgver.war ${pkgdir}/var/lib/${pkgname}/webapps/
  unzip $srcdir/${pkgname}-${pkgver}.war -d ${pkgdir}/var/lib/${pkgname}/webapps/$pkgname-$pkgver
  ln -s $pkgname-$pkgver ${pkgdir}/var/lib/${pkgname}/webapps/ROOT
  chown -R 71:71 ${pkgdir}/var/lib/${pkgname}
  ln -s /var/lib/${pkgname}/webapps ${pkgdir}/usr/share/${pkgname}/webapps

  #conf
  install -dm775 ${pkgdir}/etc/${pkgname}
  install -g 71 -m640 ${srcdir}/web.xml ${pkgdir}/etc/${pkgname}
  install -g 71 -m640 ${srcdir}/server.xml ${pkgdir}/etc/${pkgname}
  install -g 71 -m640 ${srcdir}/context.xml ${pkgdir}/etc/${pkgname}
  install -d -g 71 -m775 ${pkgdir}/etc/${pkgname}/Catalina
  ln -s /etc/${pkgname} ${pkgdir}/usr/share/${pkgname}/conf

  #temp, work
  install -dm1777 ${pkgdir}/var/tmp
  install -dm775 -o 71 -g 71 ${pkgdir}/var/tmp/${pkgname}/{temp,work}
  ln -s /var/tmp/${pkgname}/temp ${pkgdir}/usr/share/${pkgname}/temp
  ln -s /var/tmp/${pkgname}/work ${pkgdir}/usr/share/${pkgname}/work

  #systemd scripts
  install -Dm644 ${srcdir}/systemd_$pkgname.service \
                 ${pkgdir}/usr/lib/systemd/system/$pkgname.service
  install -Dm644 ${srcdir}/systemd_tmpfiles.d_$pkgname.conf \
                 ${pkgdir}/usr/lib/tmpfiles.d/${pkgname}.conf
}

