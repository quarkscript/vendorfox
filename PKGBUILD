pkgname='vendor-fox'
pkgver='0'
pkgrel='1'
pkgdesc='Make firefox more secure.'
arch=('any')
license=('MIT')
depends=('firefox')
source=("https://raw.github.com/pyllyukko/user.js/master/user.js")
sha512sums=('SKIP')
worked_dir='/usr/lib/firefox/browser/defaults/preferences'
# For firefox-based browsers worked_dir may differ
prepare() {
if [ ! -e "${startdir}/vendor.js" ]; then
    msg ' Customized vendor.js is not found, then will be used alternative from '
    msg ' https://github.com/pyllyukko/user.js '
    msg ' You can package your own vendor.js just place it with PKGBUILD '
    cat user.js | sed 's/user_pref(/pref(/g' >vendor.js.modd
else
    msg ' Your customized vendor.js will be used '
    cp ${startdir}/vendor.js vendor.js.modd
fi
}
package() {
cd "${srcdir}"
install -Dm 644 "vendor.js.modd" "${pkgdir}${worked_dir}/vendor.js.modd"
cat<<EOF>"${startdir}/${pkgname}.install"
post_install () {
if [ ! -e "${worked_dir}/vendor.js.backup" ]&&[ -e "${worked_dir}/vendor.js" ]; then
    echo "Backup vendor.js"
    mv -f ${worked_dir}/vendor.js ${worked_dir}/vendor.js.backup
fi
cp -f ${worked_dir}/vendor.js.modd ${worked_dir}/vendor.js
echo ' 
Every reinstall or update of firefox 
eliminate effects of this package
so you need to reinstall $pkgname too. 
Be aware!'
}
post_upgrade () {
cp -f ${worked_dir}/vendor.js.modd ${worked_dir}/vendor.js
}
post_remove() {
if [ -e "${worked_dir}/vendor.js.backup" ]; then
    echo "Restore vendor.js"
    mv -f ${worked_dir}/vendor.js.backup ${worked_dir}/vendor.js
fi
}
EOF
true && install=${pkgname}.install  
}
