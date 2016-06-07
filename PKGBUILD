# Maintainer: Adrian Perez <aperez@igalia.com>
# Contributor: rway <rway07@gmail.com>
# Contributor: wabi <aschrafl@jetnet.ch>
# Contributor: Alexander RÃ¸dseth <rodseth@gmail.com>
# Contributor: Andreas Schrafl <aschrafl@gmail.com>
# Contributor: piojo <aur@zwell.net>
# Contributor: hack.augusto <hack.augusto@gmail.com>
# Contributor: Yen Chi Hsuan <yan12125@gmail.com>

pkgname=depot-tools-git
pkgver=r3323.3bff56b
pkgrel=2
pkgdesc='Build tools for working with Chromium development, include gclient'
arch=('any')
url='http://dev.chromium.org/developers/how-tos/install-depot-tools'
source=("${pkgname}::git+https://chromium.googlesource.com/chromium/tools/depot_tools.git"
	'depot_tools.sh' 'repo_fix.sh' 'fixshebangs.py')
license=('Custom')
depends=('python2' 'python2-colorama' 'python2-pylint')
makedepends=('git' 'scons' 'setconf')
provides=('depot_tools' 'gclient')
conflicts=('gclient-svn' 'depot_tools-svn')
options=('!strip')
install="depot_tools.install"
sha512sums=('SKIP'
            'dbd6e66dce2b142830c7f22df79f6956f7f2aa762e80c1121f1a12599a8d98230d67404319c86549f52da7e736c56231d857a0f6a2dd5139b94bf70f5d7526fa'
            'bde33ffcad42a4d554d5490b6562981ef4b9f3abebadbed909749ee05ba391da4b5acb31b901e785b6f019b4ed3f9c740ab92623dd6a87e67b4b599a0010374b'
            '1fb1cbca2540dfe98ad52fce493e6545e1c7803aaa6e83ce5cde1ffcb3ced9ad39d2ff1a7a6918e1f0c33e8260ea46c03692d276d1beea2bc96383f7061e8043')

scripts_to_fix_exec=(
	apply_issue
	cit
	clang-format
	commit_queue
	depot-tools-auth
	download_from_google_storage
	drover
	fetch
	gcl
	gclient
	git-runhooks
	gn
	roll-dep-svn
	roll-dep
)

pkgver () {
	cd "${pkgname}"
	printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare () {
	cd "${pkgname}"

	# This tools work with python2, but ArchLinux default is python3. Fix it.
	# pylint is in extra, ninja is an executable and it does not need any change.
	# gclient.py require a fix for work correctly with python2-colorama
	# Another way is make default python2, but I don't think is a good idea!
	# Fixing python scripts.
	msg "Patching scripts for python2 usage..."
	"${srcdir}/fixshebangs.py"

	# Fix gclient.py
	sed -i -r -e 's/from third_party import colorama/import colorama/' \
			  -e 's/from third_party.colorama import Fore/from colorama import Fore/' \
		gclient.py

	# Fixing scripts which use "exec python"
	for script in "${scripts_to_fix_exec[@]}"
	do
		sed -r -i -e 's/exec python/exec python2/' "${script}"
	done
}

package()
{
	# Creating directories
	install -d "${pkgdir}/opt"

	cp -r "${srcdir}/${pkgname}" "${pkgdir}/opt/depot_tools"

	# Export PATH
	install -Dm755 "${srcdir}/depot_tools.sh" "${pkgdir}/etc/profile.d/depot_tools.sh"

	# Install repo_fix.sh script
	install -Dm 755 "${srcdir}/repo_fix.sh" "${pkgdir}/opt/depot_tools"

	# Install License
	install -Dm644 "${pkgdir}/opt/depot_tools/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

	rm -rf "${pkgdir}/opt/depot_tools/.git"
}
