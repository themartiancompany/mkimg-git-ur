# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer:  Truocolo <truocolo@aol.com>
# Maintainer:  Pellegrino Prevete <pellegrinoprevete@gmail.com>

_git=false
_local=false
_proj="hip"
_pkgname=mkimg
pkgname="${_pkgname}-git"
pkgver="0.0.0.0.0.0.0.0.0.0.0.0.0.1".r0.g"0a66419696f3191ea9b1523c46d2d2072aa59060"
pkgrel=1
_pkgdesc=(
  "Generate image files in various formats."
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  any
)
_gl="gitlab.com"
_gh="github.com"
_host="https://${_gh}"
_ns='themartiancompany'
_local="${HOME}/${_pkgname}"
url="${_host}/${_ns}/${_pkgname}"
_gh_api="https://api.${_gh}/repos/${_ns}/${_pkgname}"
license=(
  AGPL3
)
depends=(
  "e2fsprogs"
  "libcrash-bash"
)
_os="$( \
  uname \
    -o)"
[[ "${_os}" != "GNU/Linux" ]] && \
[[ "${_os}" == "Android" ]] && \
  depends+=(
  )
makedepends=(
  make
)
checkdepends=(
  shellcheck
)
optdepends=(
  'blk-utils: swap volume support'
  "erofs-utils: 'enhanced read only file system' support"
  'luks-tools: LUKS volumes support'
  'mdadm: RAID images support'
  'mtools: FAT file system support'
  'squashfs-tools-ng: Squash file system support'
)
[[ "${_os}" == "GNU/Linux" ]] && \
[[ "${_os}" != "Android" ]] && \
  optdepends+=(
    "btrfs-progs: 'better file system' support"
  )
provides=(
  "${_pkgname}=${pkgver}"
)
conflicts=(
  "${_pkgname}"
)
groups=(
 "${_proj}"
 "${_proj}-git"
)
_url="${url}"
[[ "${_local}" == true ]] && \
  _url="${_local}"
source=()
_branch="master"
[[ "${_git}" == true ]] && \
  makedepends+=(
    git
  ) && \
  source+=(
    "${_pkgname}-${_branch}::git+${_url}#branch=${_branch}"
  )
[[ "${_git}" == false ]] && \
  makedepends+=(
    curl
    jq
  ) && \
  source+=(
    "${_pkgname}.tar.gz::${_url}/archive/refs/heads/${_branch}.tar.gz"
  )
sha256sums=(
  SKIP
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
    "${_pkgname}-${_branch}"
  if [[ "${_git}" == true ]]; then
    _git_pkgver
  elif [[ "${_git}" == false ]]; then
    _jq_pkgver
  fi
}

package() {
  cd \
    "${_pkgname}-${_branch}"
  make \
    DESTDIR="${pkgdir}" \
    PREFIX="/usr" \
    install
}

# vim: ft=sh syn=sh et
