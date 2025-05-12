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

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>

# shellcheck disable=SC2034
_py="python"
_pkg=mkaudiocdrimg
_namespace="tallero"
_pkgbase="${_pkg}"
_pkgname="${_pkgbase}"
pkgbase="${_pkg}-git"
pkgname=(
  "${_pkg}-git"
)
pkgver="1.0+1+g92f8081"
pkgrel=1
_pkgdesc=(
  "Make an audio CD-R image"
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
  'shntool'
  "${_py}-appdirs"
)
makedepends=(
  "${_py}-setuptools"
)
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
_tarname="${_pkg}"
_uri="git+${_url}#${_tag_name}=${_tag}"
_src="${_tarname}::${_uri}"
source=(
  "${_src}"
)
sha256sums=(
  "SKIP"
)

pkgver() {
  cd \
    "${_pkg}"
  git \
    describe \
      --tags | \
    sed \
      's/-/+/g'
}

package() {
  cd \
    "${_pkg}"
  "${_py}" \
    "setup.py" \
      install \
      --root="${pkgdir}" \
      --optimize=1
}
