# Maintainer: Stefan Husmann <stefan-husmann@t-online.de>
# Contributor: Lex Black <autumn-wind@web.de>
# Contributor: Mr. Outis <mroutis@protonmail.com>

pkgname=dvc
pkgver=1.5.0
pkgrel=1
pkgdesc="Open-source version control system for data science projects"
arch=('any')
license=('Apache')
url="https://github.com/iterative/${pkgname}"
depends=('python' 'python-appdirs' 'python-colorama' 'python-configobj'
         'python-distro' 'python-flufl-lock' 'python-funcy'
         'python-gitdb' 'python-gitpython' 'python-humanize'
         'python-inflect' 'python-jsonpath-ng' 'python-packaging'
         'python-pathspec' 'python-ply' 'python-pyasn1'
         'python-yaml' 'python-requests' 'python-ruamel-yaml'
         'python-setuptools' 'python-shortuuid' 'python-tqdm'
         'python-treelib' 'python-voluptuous' 'python-zc.lockfile'
	 'python-nanotime' 'python-grandalf' 'python-ntfs')
optdepends=('python-google-cloud-storage: support for Google Cloud'
            'python-google-api-python-client: support for GDrive'
            'python-pydrive: support for GDrive'
            'python-boto3: support for AWS S3 remote'
            'python-paramiko: support for SSH remote'
            'python-azure-storage: support for Azure remote'
            'python-oss2: support for Aliyun Object Storage Service (OSS)'
            'python-pyarrow: support for HDFS remote'
	    )
# OptDeps to upload on AUR: python-oss2, 
source=("${pkgname}-${pkgver}.tar.gz::${url}/archive/${pkgver}.tar.gz")
sha256sums=('73de46d8cf1eaaa9c431e5f18ea102a669e375f5bd67d813bca950b16c1400e8')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  python setup.py build
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  python setup.py install --prefix=/usr --root="${pkgdir}" --optimize=1 --skip-build
}
