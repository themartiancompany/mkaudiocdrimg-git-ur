# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2022, 2023, 2024, 2025  Pellegrino Prevete
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

# Maintainer:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer:
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>

# shellcheck disable=SC2034
if [[ ! -v "_git" ]]; then
  _git="true"
fi
if [[ ! -v "_docs" ]]; then
  _docs="true"
fi
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
_pkg=mkaudiocdrimg
_pkgbase="${_pkg}"
_pkgname="${_pkgbase}"
pkgbase="${_pkg}-git"
pkgname=(
  "${_pkg}-git"
)
if [[ "${_docs}" == "true" ]]; then
  pkgname+=(
    "${_pkg}-docs-git"
  )
fi
pkgver="1.0+1+g92f8081"
pkgrel=1
_pkgdesc=(
  "Produces an audio CD-R image"
  "from media files."
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'any'
)
_gl_http="https://gitlab.com"
_gl_ns="tallero"
_gh_http="https://github.com"
_gh_ns="themartiancompany"
url="${_gh_http}/${_gh_ns}/${_pkg}"
provides=(
  "${_pkg}=${pkgver}"
  "${_py}-${_pkg}=${pkgver}"
)
conflicts=(
  "${_pkg}"
  "${_py}-${_pkg}"
)
depends=(
  'ffmpeg'
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-appdirs"
  'shntool'
)
makedepends=(
  "${_py}"
  "${_py}-setuptools"
)
if [[ "${_docs}" == "true" ]]; then
  makedepends+=(
    "make"
  )
fi
if [[ "${_git}" == "true" ]]; then
  makedepends+=(
    "git"
  )
fi
license=(
  "AGPL3"
)
_url="${url}"
_branch="master"
_tag_name="branch"
_tag="${_branch}"
_tarname="${_pkg}-${_tag}"
if [[ "${_git}" == "true" ]]; then
  _uri="git+${_url}#${_tag_name}=${_tag}"
  _src="${_tarname}::${_uri}"
elif [[ "${_git}" == "false" ]]; then
  _uri="${_url}/archive/${_tag}.zip"
  _src="${_tarname}.zip::${_uri}"
fi
source=(
  "${_src}"
)
sha256sums=(
  "SKIP"
)

_nth() {
  local \
    _str="${1}" \
    _n="${2}"
  echo \
    "${_str}" | \
    awk \
      -F '+' \
      '{print $'"${_n}"'}'
}

_jq_pkgver() {
  local \
    _version \
    _rev \
    _commit
  _version="$( \
    curl \
      --silent \
      "${_gh_api}/tags" | \
      jq \
        '.[0].name')"
  _version_commit="$( \
    curl \
      --silent \
      "${_gh_api}/tags" | \
      jq \
        '.[0].commit.sha')"
  _rev="$( \
    curl \
      --silent \
      "${_gh_api}/commits" | \
      jq \
        'map(.sha == '${_version_commit}' ) | index(true)')"
  _commit="$( \
    curl \
      --silent \
      "${_gh_api}/commits" | \
      jq \
        '.[0].sha')"
  printf \
    "%s.r%s.g%s" \
    "${_version}" \
    "${_rev}" \
    "${_commit}"
}

_parse_ver() {
  local \
    _pkgver="${1}" \
    _out="" \
    _ver \
    _rev \
    _commit
  _ver="$( \
    _nth \
      "${_pkgver}" \
      "1")"
  _rev="$( \
    _nth \
      "${_pkgver}" \
      "2")"
  _commit="$( \
    _nth \
      "${_pkgver}" \
      "3")"
  _out=${_ver}
  [[ "${_rev}" != "" ]] && \
    _out+=".r${_rev}"
  [[ "${_commit}" != "" ]] && \
    _out+=".${_commit}"
  echo \
    "${_out}"
}

_git_pkgver() {
  local \
    _pkgver
  _pkgver="$( \
    git \
      describe \
      --tags \
      --long | \
      sed \
        's/-/+/g')"
  _parse_ver \
    "${_pkgver}"
}

pkgver() {
  cd \
    "${_tarname}"
  if [[ "${_git}" == true ]]; then
    _git_pkgver
  elif [[ "${_git}" == false ]]; then
    _jq_pkgver
  fi
}

package_mkaudiocdrimg-git() {
  cd \
    "${_tarname}"
  "${_py}" \
    "setup.py" \
      install \
      --root="${pkgdir}" \
      --optimize=1
  install \
    -Dm644 \
    "COPYING" \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
}

package_mkaudiocdrimg-docs-git() {
  local \
    _make_opts=()
  depends+=(
    "${_pkg}=${pkgver}"
  )
  provides=(
    "${_pkg}-docs=${pkgver}"
    "${_py}-${_pkg}-docs=${pkgver}"
  )
  conflicts=(
    "${_pkg}-docs"
    "${_py}-${_pkg}-docs"
  )
  _make_opts+=(
    DESTDIR="${pkgdir}"
    PREFIX="/usr"
  )
  cd \
    "${_tarname}"
  make \
    "${_make_opts[@]}" \
    install-man
}
