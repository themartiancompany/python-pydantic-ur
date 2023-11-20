# Maintainer: David Runge <dvzrv@archlinux.org>
# Maintainer: Filipe Laíns (FFY00) <lains@archlinux.org>

_name=pydantic
pkgname=python-$_name
# WARNING: upstream pins pydantic-core down to the patch-level and using other versions breaks tests! only update in lock-step with python-pydantic-core!
pkgver=2.5.1
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
  python-cloudpickle
  python-devtools
  python-dirty-equals
  python-email-validator
  python-faker
  python-hypothesis
  python-pygments
  python-pytest
  python-pytest-benchmark
  python-pytest-examples
  python-pytest-mock
  python-sqlalchemy
)
optdepends=(
  'mypy: for type validation with mypy'
  'python-dotenv: for .env file support'
  'python-email-validator: for email validation'
  'python-hypothesis: for hypothesis plugin when using legacy v1'
)
source=($url/archive/v$pkgver/$_name-v$pkgver.tar.gz)
sha512sums=('7260d6496f2de69434c79eb36109e040b1307c53a15cf4c669fdeb4b3cded6c8b89cd449f24e6504b67e86641702664378801ad0ef631dcea858cdfaa529033b')
b2sums=('aafa69c00f3d5d099fd9e7e6602eee225d4efd14e99d0c59be51f0755aa3406fb56a91b13285c28e218e2149f5b1f888ee4c1613673c3f03d4b3d13dd200e713')

build() {
  cd $_name-$pkgver
  python -m build --wheel --no-isolation
}

check() {
  local pytest_options=(
    # disable broken tests: https://github.com/pydantic/pydantic/issues/8180
    --deselect tests/benchmarks/test_north_star.py::test_north_star_validate_json
    --deselect tests/benchmarks/test_north_star.py::test_north_star_validate_json_strict
    --deselect tests/benchmarks/test_north_star.py::test_north_star_dump_json
    --deselect tests/benchmarks/test_north_star.py::test_north_star_validate_python
    --deselect tests/benchmarks/test_north_star.py::test_north_star_validate_python_strict
    --deselect tests/benchmarks/test_north_star.py::test_north_star_dump_python
    --deselect tests/benchmarks/test_north_star.py::test_north_star_json_loads
    --deselect tests/benchmarks/test_north_star.py::test_north_star_json_dumps
    -vv
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
