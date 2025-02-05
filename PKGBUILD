# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: David Runge <dvzrv@archlinux.org>
# Maintainer: Filipe Laíns (FFY00) <lains@archlinux.org>

_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=pydantic
pkgname="${_py}-${_pkg}"
# WARNING: upstream pins pydantic-core
#          down to the patch-level and
#          using other versions breaks tests!
#          only update in lock-step with python-pydantic-core!
pkgver=2.5.3
pkgrel=1
_pkgdesc=(
  'Data parsing and validation'
  'using Python type hints'
)
pkgdesc="${_pkgdesc[*]}"
arch=(any)
_http="https://github.com"
_ns="${_pkg}"
url="${_http}/${_ns}/${_pkg}"
license=(
  'MIT'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-annotated-types"
  "${_py}-pydantic-core"
  "${_py}-typing-extensions"
)
makedepends=(
  "cython"
  "${_py}-build"
  "${_py}-installer"
  "${_py}-importlib-metadata"
  "${_py}-hatchling"
  "${_py}-hatch-fancy-pypi-readme"
  "${_py}-wheel"
)
checkdepends=(
  "${_py}-ansi2html"
  "${_py}-cloudpickle"
  "${_py}-devtools"
  "${_py}-dirty-equals"
  "${_py}-email-validator"
  "${_py}-faker"
  "${_py}-hypothesis"
  "${_py}-pygments"
  "${_py}-pytest"
  "${_py}-pytest-benchmark"
  "${_py}-pytest-examples"
  "${_py}-pytest-mock"
  "${_py}-sqlalchemy"
)
optdepends=(
  'mypy: for type validation with mypy'
  "${_py}-dotenv: for .env file support"
  "${_py}-email-validator: for email validation"
  "${_py}-hypothesis: for hypothesis plugin when using legacy v1"
)
source=(
  "${url}/archive/v${pkgver}/${_pkg}-v${pkgver}.tar.gz"
)
sha512sums=(
  '6c920e4ccbb4212e298731f9947dd9af4b7f44006a17c75f8084674e719108dd4e23e36bf9e3d948e4acb4db7f279c78ee6d769796abf0e4cc1f9c320d2f40db'
)
b2sums=(
  '20d990bfabbee242212c77b9270c091c202819a9a973afce06024078fc502dc2967194b812884a5941adcf91f2e7ea5b5e0d33142af958be6dbe8da10cfa4131'
)

build() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --no-isolation
}

check() {
  local \
    pytest_options=() \
    site_packages=()
  pytest_options=(
    # disable broken tests: https://github.com/pydantic/pydantic/issues/8459
    --deselect tests/benchmarks/test_north_star.py::test_north_star_validate_json
    --deselect tests/benchmarks/test_north_star.py::test_north_star_validate_json_strict
    --deselect tests/benchmarks/test_north_star.py::test_north_star_dump_json
    --deselect tests/benchmarks/test_north_star.py::test_north_star_validate_python
    --deselect tests/benchmarks/test_north_star.py::test_north_star_validate_python_strict
    --deselect tests/benchmarks/test_north_star.py::test_north_star_dump_python
    --deselect tests/benchmarks/test_north_star.py::test_north_star_json_loads
    --deselect tests/benchmarks/test_north_star.py::test_north_star_json_dumps
    # disable broken doc test: https://github.com/pydantic/pydantic/issues/8460
    --deselect 'tests/test_docs.py::test_docs_examples[docs/concepts/serialization.md:354-391]'
    -vv
  )
  site_packages="$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")"
  cd \
    "${_pkg}-${pkgver}"
  # install to temporary location, as importlib is used
  "${_py}" \
    -m \
      installer \
        --destdir="test_dir" \
        "dist/"*".whl"
  export \
    PYTHONPATH="${PWD}/test_dir/${site_packages}:${PYTHONPATH}"
  pytest \
    "${pytest_options[@]}"
}

package() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      installer \
        --destdir="${pkgdir}" \
        "dist/"*".whl"
  install \
    -vDm644 \
    "LICENSE" \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
}

# vim:set sw=2 sts=-1 et:
