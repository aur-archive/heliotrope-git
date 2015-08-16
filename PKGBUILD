# Maintainer: uberushaximus <uberushaximus AT gmail DOT com>

_gemname=heliotrope
pkgname=heliotrope-git
pkgver=r288.dac4e54
pkgrel=1
pkgdesc="A personal, threaded, search-centric email server."
arch=(any)
url="https://github.com/sup-heliotrope/heliotrope"
license=('GPL')
depends=('ruby-trollop' 'ruby-whistlepig>=0.12' 'ruby-rmail>=1.0.0' 'ruby-leveldb-ruby>=0.13' 'ruby-locale' 'ruby-rest-client' 'ruby-rack' 'ruby-json' 'ruby-sinatra')
makedepends=('git')

_gitroot=https://github.com/sup-heliotrope/heliotrope.git
_gitname=heliotrope

pkgver() {
  cd "$srcdir/$_gitname"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [[ -d "$_gitname" ]]; then
    cd "$_gitname" && git pull origin
    msg "The local files are updated."
  else
    git clone "$_gitroot" "$_gitname"
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  rake gem
  mv "pkg/heliotrope-0.1.gem" "pkg/$_gemname-$pkgver.gem"
}

package() {
  cd "$srcdir/$_gitname-build/pkg"
  # _gemdir is defined inside package() because if ruby[gems] is not installed on
  # the system, makepkg will exit with an error when sourcing the PKGBUILD.
  local _gemdir="$(ruby -rubygems -e'puts Gem.default_dir')"

  gem install --no-user-install --ignore-dependencies -i "$pkgdir$_gemdir" -n "$pkgdir/usr/bin" \
    "$_gemname-$pkgver.gem"
}

# vim:set ts=2 sw=2 et:
