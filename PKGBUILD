# Maintainer: David Runge <dvzrv@archlinux.org>
# Maintainer: Filipe Laíns (FFY00) <lains@archlinux.org>

_name=pydantic
pkgname=python-$_name
pkgver=2.0.2
pkgrel=1
pkgdesc='Data parsing and validation using Python type hints'
arch=(any)
url="https://github.com/pydantic/pydantic"
license=(MIT)
depends=(
  python
  python-annotated-types
  python-pydantic-core
  python-typing-extensions
)
makedepends=(
  cython
  python-build
  python-installer
  python-importlib-metadata
  python-hatchling
  python-hatch-fancy-pypi-readme
  python-wheel
)
checkdepends=(
  python-ansi2html
  python-devtools
  python-dirty-equals
  python-email-validator
  python-faker
  python-hypothesis
  python-pytest
  python-pytest-examples
  python-pytest-mock
  python-sqlalchemy
)
optdepends=(
  'python-dotenv: for .env file support'
  'python-email-validator: for email validation'
)
source=($url/archive/v$pkgver/$_name-v$pkgver.tar.gz)
sha512sums=('91398ed16057ebd331cc2bb22c2651610b2acb42634597773331cf7b5f418d400329613f37f85f0ab3dc16c76cccac8d5e76d960c7b732c68d007c852d0a4cb1')
b2sums=('6d0506a7265dd7df078d87ee1538685e5eedc04c52e87b92014448e7e38955c04091345235f5fdef404e954ea548c644be4721e85c9f36e64ec09484d4c3395d')

build() {
  cd $_name-$pkgver
  python -m build --wheel --no-isolation
}

check() {
  local pytest_options=(
    -vv
    # https://github.com/pydantic/pydantic/issues/6656
    --deselect tests/test_docs.py::test_docstrings_examples
    --deselect tests/test_docs.py::test_docs_devtools_example
  )
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")

  cd $_name-$pkgver
  # install to temporary location, as importlib is used
  python -m installer --destdir=test_dir dist/*.whl
  export PYTHONPATH="$PWD/test_dir/$site_packages:$PYTHONPATH"
  pytest "${pytest_options[@]}"
}

package() {
  cd $_name-$pkgver
  python -m installer --destdir="$pkgdir" dist/*.whl
  install -vDm 644 LICENSE -t "$pkgdir/usr/share/licenses/$pkgname/"
}
